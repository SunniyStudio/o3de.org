---
title: "分配器标记指南"
description: ""
toc: false
---

O3DE 分配器设置为允许使用层次结构来标记来自特定基本分配器的内存。 

O3DE 的主要碱基分配器是
1. The [OS Allocator](https://github.com/o3de/o3de/blob/fc7f51a5748612eb069ec4623908311e2af99915/Code/Framework/AzCore/AzCore/Memory/OSAllocator.h#L19-L25) which delegates to the operating System `malloc` call.
1. The [System Allocator](https://github.com/o3de/o3de/blob/212d3c4d32cfcca4d960654b82d68de3d3b5f924/Code/Framework/AzCore/AzCore/Memory/SystemAllocator.h#L17-L25) which uses the memory [High Performance Allocator Schema](https://github.com/o3de/o3de/blob/fc7f51a5748612eb069ec4623908311e2af99915/Code/Framework/AzCore/AzCore/Memory/HphaAllocator.h#L16-L28) under the hood for allocating memory.
1. The [Pool Allocator](https://github.com/o3de/o3de/blob/fc7f51a5748612eb069ec4623908311e2af99915/Code/Framework/AzCore/AzCore/Memory/PoolAllocator.h#L196-L202) which uses a pool of memory for allocating specific block sizes of memory in O(1)
1. The [Thread Pool Allocator](https://github.com/o3de/o3de/blob/212d3c4d32cfcca4d960654b82d68de3d3b5f924/Code/Framework/AzCore/AzCore/Memory/PoolAllocator.h#L221-L226) which is a thread safe implementation of the pool allocator, where separate pools are allocated on different threads.

本指南提供了有关如何继承 O3DE 分配器并按顺序跟踪特定系统（例如动画或预制件）的内存的示例

#### 必要提交
此处提到的指南要求将以下提交集成到 users 分支中。
| 提交链接 |需要的理由 |
|----|----|
|https://github.com/o3de/o3de/commit/eb3887215998f300a9e700b8294db99ac539c135 | 添加了对 `sys_DumpAllocators` 命令的支持。|
https://github.com/o3de/o3de/commit/60e195e001e4cd40881ac1140ef851bfeb066336 | 903936edc736ef3a0ba8ea1d7668a01c185d3841的依赖提交 |
|https://github.com/o3de/o3de/commit/903936edc736ef3a0ba8ea1d7668a01c185d3841 | 212d3c4d32cfcca4d960654b82d68de3d3b5f924的依赖提交|
|https://github.com/o3de/o3de/commit/67763caae64d05b2effa0ec5f7173dc93c98e329 | 212d3c4d32cfcca4d960654b82d68de3d3b5f924的依赖提交|
|https://github.com/o3de/o3de/commit/212d3c4d32cfcca4d960654b82d68de3d3b5f924 | 添加了对启动配置文件、新的 `sys_DumpAllocationRecords*` 控制台命令和子分配器的准确内存跟踪的支持。|


### 搭载 Existing 分配器

要捎带现有分配器，可以将基分配器类包装在`AZ::ChildAllocatorSchema`模板中。
然后应该添加一个继承的类，该类实现 `AZ_RTTI` 宏，该宏允许为新的分配器本身提供用户可读的名称。
封装这些步骤的 `AZ_CHILD_ALLOCATOR` 宏

#### 例如，为 PrefabSystem 创建一个分配器，该分配器使用 SystemAllocator 进行分配
```c++
AZ_CHILD_ALLOCATOR(PrefabSystemAllocator, "{CD8443AE-BC56-4C46-AF3A-6633C2C0E694}", AZ::SystemAllocator);
```

上述宏创建`PrefabSystemAllocator`，它继承自 ChildAllocatorSchema 类模板和标准容器分配器别名`PrefabSystemAllocator_for_std_t`。
`PrefabSystemAllocator` 可用于使用 `AZ_CLASS_ALLOCATOR` 宏为类指定分配器。
`PrefabSystemAllocator_for_std_t`用于 AZStd 容器类型，例如 `AZStd::vector`、`AZStd::basic_string`、`AZStd::unordered_map`等...用作标准容器分配器。

旁注：应为新分配器生成一个随机 UUID。
这可以在 Windows 上使用 Visual Studio Tools -> Create Guid 菜单中提供的创建 GUID 工具来完成。

![image](https://github.com/o3de/o3de/assets/56135373/b02d6a62-535c-46e3-ba17-e6cfc42d25a7)

或者，可以使用 Python [UUID](https://docs.python.org/3/library/uuid.html#command-line-usage) 模块使用以下一行代码创建随机 UUID

```bash
python -c "import uuid; print(f'{{{str(uuid.uuid4()).upper()}}}');"
```

### 将 PrefabSystemAllocator 用于预制件类/结构

PrefabSystemComponent.h
```c++
class PrefabLoader final
    : public PrefabLoaderInterface
{
    public:
        AZ_TYPE_INFO_WITH_NAME_DECL(PrefabLoader);
        AZ_RTTI_NO_TYPE_INFO_DECL();
        AZ_CLASS_ALLOCATOR_DECL;
};

class PrefabSystemComponent
    : public AZ::Component
{
public:
    AZ_COMPONENT_DECL(PrefabSystemComponent);
private:
    using ContainerUsingCustomAllocator = AZStd::vector<int, PrefabSystemAllocator_for_std_t>;
    ContainerUsingCustomAllocator m_customContainer;
};
```

PrefabSystemComponent.cpp
```c++
AZ_TYPE_INFO_WITH_NAME_IMPL(PrefabLoader, "PrefabLoader", "{A302B072-4DC4-4B7E-9188-226F56A3429C}");
AZ_RTTI_NO_TYPE_INFO_IMPL(PrefabLoader, PrefabLoaderInterface);
AZ_CLASS_ALLOCATOR_IMPL(PrefabLoader, PrefabSystemAllocator)

AZ_COMPONENT_IMPL_WITH_ALLOCATOR(PrefabSystemComponent, "{27203AE6-A398-4614-881B-4EEB5E9B34E9}", PrefabSystemAllocator);
```

### 输出分配器在运行时使用的总内存

 [sys_DumpAllocator](https://github.com/o3de/o3de/pull/16864)控制台命令可通过 AZ Console 系统用于转储任何活动分配的内存分配。

控制台变量用户的示例输出如下。
它被设置为能够放入 CSV 文件中以帮助进行数据分析。
```
[CONSOLE] Executing console command 'sys_DumpAllocators'
Index,Name,Used KiB,Reserved KiB,Consumed KiB,Parent Allocator
0,OSAllocator,1383.67,2097152.00,2097152.00,
1,SystemAllocator,346025.41,2097152.00,2097152.00,
2,RapidJSONAllocator,1345.22,2097152.00,2097152.00,OSAllocator
3,EntityAllocator,2.93,2097152.00,2097152.00,SystemAllocator
4,ComponentAllocator,123.48,2097152.00,2097152.00,SystemAllocator
5,PhysXAllocator,321.34,2097152.00,2097152.00,SystemAllocator
6,ThreadPoolAllocator,1039.00,2097152.00,2097152.00,
7,LuaSystemAllocator,3714.58,2097152.00,2097152.00,SystemAllocator
8,AudioSystemAllocator,148.19,2097152.00,2097152.00,SystemAllocator
9,LuaSystemAllocator,264.08,2097152.00,2097152.00,SystemAllocator
10,RHISystemAllocator,0.66,2097152.00,2097152.00,SystemAllocator
11,AzClothAllocator,0.14,2097152.00,2097152.00,SystemAllocator
12,AWSNativeSDKAllocator,99.48,2097152.00,2097152.00,SystemAllocator
13,EMotionFXAllocator,487.15,2097152.00,2097152.00,SystemAllocator
14,EMotionFX::AttributeAllocator,0.34,2097152.00,2097152.00,
15,EMotionFXManagerAllocator,0.27,2097152.00,2097152.00,SystemAllocator
16,ImporterAllocator,0.80,2097152.00,2097152.00,SystemAllocator
17,ActorManagerAllocator,0.20,2097152.00,2097152.00,SystemAllocator
18,ActorUpdateAllocator,0.12,2097152.00,2097152.00,SystemAllocator
19,MotionManagerAllocator,0.16,2097152.00,2097152.00,SystemAllocator
20,MotionAllocator,0.82,2097152.00,2097152.00,SystemAllocator
21,MotionEventManagerAllocator,0.16,2097152.00,2097152.00,SystemAllocator
22,SoftSkinManagerAllocator,0.02,2097152.00,2097152.00,SystemAllocator
23,AnimGraphManagerAllocator,0.16,2097152.00,2097152.00,SystemAllocator
24,BlendSpaceManagerAllocator,0.05,2097152.00,2097152.00,SystemAllocator
25,AnimGraphAllocator,0.14,2097152.00,2097152.00,SystemAllocator
26,RecorderAllocator,0.58,2097152.00,2097152.00,SystemAllocator
27,PoseAllocator,0.12,2097152.00,2097152.00,SystemAllocator
28,ThreadDataAllocator,6.07,2097152.00,2097152.00,SystemAllocator
29,EditorAllocator,2.88,2097152.00,2097152.00,SystemAllocator
30,PropertyWidgetAllocator,0.09,2097152.00,2097152.00,SystemAllocator
31,CommandAllocator,6.72,2097152.00,2097152.00,SystemAllocator
32,PoolAllocator,0.00,2097152.00,2097152.00,
-,Totals,354975.03,69206016.00,69206016.00,
33 allocators active
```

**注意：目前最大限制为 [100 个已注册的分配器](https://github.com/o3de/o3de/blob/212d3c4d32cfcca4d960654b82d68de3d3b5f924/Code/Framework/AzCore/AzCore/Memory/AllocatorManager.h#L155-L156)**.

## 如何为分配器添加分配记录跟踪

在 O3DE 中，分配器支持记录使用特定分配器时发生的每个分配的地址、大小和调用堆栈。 
这可以通过打开 allocation record tracking 以及 allocator 本身的分析来完成。
默认情况下，仅当将 [LeakDectionFixture](https://github.com/o3de/o3de/blob/212d3c4d32cfcca4d960654b82d68de3d3b5f924/Code/Framework/AzCore/AzCore/UnitTest/TestTypes.h#L109-L115)  用于 Googletest 装置时，仅在 C++ Unit Test 框架中默认启用此功能。

为了在其他运行时应用程序（如 GameLauncher 和 Editor）中启用分配器记录，O3DE 中添加了对启动配置文件的支持，该文件可以读取 Windows 样式的 INI 文件，该文件指定在 Allocation 系统中注册的分配器的名称以及应该发生的分配记录级别的值。

在配置时，对加载启动配置文件的支持可以被 [O3DE_STARTUP_CFG_FILE_CHECK_OVERRIDES](https://github.com/o3de/o3de/blob/212d3c4d32cfcca4d960654b82d68de3d3b5f924/Code/Framework/AzCore/CMakeLists.txt#L40-L43)  CMake Cache 变量覆盖。
默认情况下，启动配置文件只会在 `debug` 和 `profile` 配置中加载，而不是在  `release` 配置中加载，但可以通过指定上面的 `O3DE_STARTUP_CFG_FILE_CHECK_OVERRIDES` CMake 缓存选项来覆盖。

### 启动配置文件搜索位置

在以下位置搜索启动配置文件的路径：
1. ~/.o3de/Registry/startup.cfg (`~` 代表用户主目录。例如`/home/<user>` 用于 Linux, `C:/Users/<user>` 用于 Windows, `/Users/<user>` 用于 MacOS.
1. `<executable-directory>/Registry/startup.cfg`.
1. `O3DE_STARTUP_CFG_FILE`环境变量的值，如果已设置。
1. `--startup-cfg-file`命令行参数的值。

### 分配器记录跟踪设置

为分配器启用分配跟踪的细节包括向以下形式的启动配置文件添加设置：
`allocator_tracking_<allocator-name> = <AllocationRecordMode>`  

“\<AllocationRecordMode>” 值必须与 [AllocationRecordMode](https://github.com/o3de/o3de/blob/212d3c4d32cfcca4d960654b82d68de3d3b5f924/Code/Framework/AzCore/AzCore/Memory/AllocationRecords.h#L89-L92) 枚举的选项之一匹配。

当 `allocator_tracking_<allocator-name>` 键的值设置为高于 `RECORD_NO_RECORDS` 枚举的值时，分配记录跟踪将启用。

以下是 startup.cfg 文件示例，说明了如何打开 Allocation tracking。

分配器的名称可以在运行时使用[上一节](#output-the-total-memory-for-allocators-uses-at-runtime-memory-at-runtime) 中的 `sys_DumpAllocators` 命令找到.

#### startup.cfg
```ini
allocator_tracking_PoolAllocator = RECORD_NO_RECORDS
allocator_tracking_PoolAllocator = 0 ; // Same as the RECORD_NO_RECORDS enum
allocator_tracking_ThreadPoolAllocator = RECORD_STACK_NEVER
allocator_tracking_ThreadPoolAllocator = 1 ; // Same as the RECORD_STACK_NEVER enum
allocator_tracking_OSAllocator = RECORD_STACK_IF_NO_FILE_LINE
allocator_tracking_OSAllocator = 2 ; // Same as the RECORD_STACK_IF_NO_FILE_LINE enum
allocator_tracking_SystemAllocator = RECORD_FULL
allocator_tracking_SystemAllocator = 3 ; // Same as the RECORD_FULL enum

```

### 在运行时转储分配器的 Allocation 记录

还有几个 Console 命令，允许用户
|控制台命令|用法 |描述 |示例 |
|---|---|---|---|
|`sys_DumpAllocationRecordsToStdout`| `sys_DumpAllocationRecordsToStdout [<allocator name> ...]` | 将一组指定分配器（或所有分配器，如果提供了 no allocator name 参数）的分配记录转储到 stdout。| `sys_DumpAllocationRecordsToStdout`<br/>`sys_DumpAllocationRecordsToStdout SystemAllocator`|
|`sys_DumpAllocationRecordsToFile`| `sys_DumpAllocationRecordsToFile <filepath> [<allocator name> ...]` | 将一组指定分配器（或所有分配器，如果提供了 no allocator name 参数，则所有分配器）的分配记录转储到提供的 filepath。<br>相对文件路径是相对于应用程序的当前工作目录进行处理的。| `sys_DumpAllocationRecordsToFile foo.txt`<br/>`sys_DumpAllocationRecordsToFile bar.txt OSAllocator`|
|`sys_DumpAllocationRecordsToDevWriteStorage`| `sys_DumpAllocationRecordsToDevWriteStorage [<allocator name> ...]` | 将一组指定分配器（或所有分配器，如果提供了 no allocator name 参数）的分配记录转储到开发人员可写存储目录。<br/>在 [Windows， Linux 和 MacOS](https://github.com/o3de/o3de/blob/67bab671e1eb66120f812e1e7b70e57402076c41/Code/Framework/AzCore/AzCore/Settings/SettingsRegistryMergeUtils.cpp#L740-L753) 的主机平台上，开发者的可写存储目录是 `<project-root>/user` 目录。<br/>在 [Android](https://github.com/o3de/o3de/blob/67bab671e1eb66120f812e1e7b70e57402076c41/Code/Framework/AzCore/Platform/Android/AzCore/Android/AndroidEnv.h#L110-L112) 和 [IOS](https://github.com/o3de/o3de/blob/67bab671e1eb66120f812e1e7b70e57402076c41/Code/Framework/AzCore/Platform/iOS/AzCore/Utils/Utils_iOS.mm#L26) 等移动平台上，它是 OS 在设备上安装应用程序时提供的公共存储。<br/><br/>具体而言，分配记录的文件路径位于：<br/>`<dev-write-storage>/application_records/records.<ISO8601Timestamp>.<process-id>.log`| `sys_DumpAllocationRecordsToDevWriteStorage`<br/>`sys_DumpAllocationRecordsToDevWriteStorage PoolAllocator`|
|`sys_DumpAllocationRecordsInRange`| `sys_DumpAllocationRecordsInRange <min-inclusive-index> <max-exclusive-index> [<allocator name> ...]` | 使用前两个参数将指定索引范围内的分配记录转储到一组指定分配器（如果提供了 no allocator name 参数，则为所有分配器）的控制台命令中。<br/>此命令将转储指定范围内的分配，直到 stdout 的最大索引。| `sys_DumpAllocationRecordsToStdout 0 100`<br/>`sys_DumpAllocationRecordsInRange 0 1000 EntityAllocator`|

#### 运行 console 命令转储 OSAllocator 分配记录
控制台输出可能如下所示
```
sys_DumpAllocationRecordsToDevWriteStorage OSAllocator
[CONSOLE] Executing console command 'sys_DumpAllocationRecordsToDevWriteStorage OSAllocator'
(mem) - Printing allocation records for allocator OSAllocator. Estimated time to print all records is 1 seconds
(mem) - Printed 142 allocations in 1 seconds for allocator "OSAllocator" (71 records per seconds)
```

运行 console 命令将导致日志文件输出到`<project-root>/user/allocation_records`目录。

![image](https://github.com/o3de/o3de/assets/56135373/f11bbfae-4f08-4a38-a68c-745cd41bcf92)

以下是转储的记录文件的示例： [records.2023-11-03T205715Z.51804.log](https://github.com/o3de/o3de/files/13254688/records.2023-11-03T205715Z.51804.log)
