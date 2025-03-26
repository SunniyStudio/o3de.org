---
title: "Allocator-Tagging-Guide"
description: ""
toc: false
---

O3DE Allocators are setup to allow using a hierarchy structure for tagging memory from specific base allocators.  

The main base allocators of O3DE are
1. The [OS Allocator](https://github.com/o3de/o3de/blob/fc7f51a5748612eb069ec4623908311e2af99915/Code/Framework/AzCore/AzCore/Memory/OSAllocator.h#L19-L25) which delegates to the operating System `malloc` call.
1. The [System Allocator](https://github.com/o3de/o3de/blob/212d3c4d32cfcca4d960654b82d68de3d3b5f924/Code/Framework/AzCore/AzCore/Memory/SystemAllocator.h#L17-L25) which uses the memory [High Performance Allocator Schema](https://github.com/o3de/o3de/blob/fc7f51a5748612eb069ec4623908311e2af99915/Code/Framework/AzCore/AzCore/Memory/HphaAllocator.h#L16-L28) under the hood for allocating memory.
1. The [Pool Allocator](https://github.com/o3de/o3de/blob/fc7f51a5748612eb069ec4623908311e2af99915/Code/Framework/AzCore/AzCore/Memory/PoolAllocator.h#L196-L202) which uses a pool of memory for allocating specific block sizes of memory in O(1)
1. The [Thread Pool Allocator](https://github.com/o3de/o3de/blob/212d3c4d32cfcca4d960654b82d68de3d3b5f924/Code/Framework/AzCore/AzCore/Memory/PoolAllocator.h#L221-L226) which is a thread safe implementation of the pool allocator, where separate pools are allocated on different threads.

This guide provides examples on how to inherit an O3DE Allocator and order to track memory for a specific system(such as Animation or Prefabs)

#### Neccessary commits
The guide mentioned here requires the following commits to be integrated into the users branch.
|commit link | reason it is needed |
|----|----|
|https://github.com/o3de/o3de/commit/eb3887215998f300a9e700b8294db99ac539c135 | adds support for the `sys_DumpAllocators` command.|
https://github.com/o3de/o3de/commit/60e195e001e4cd40881ac1140ef851bfeb066336 | dependent commit for 903936edc736ef3a0ba8ea1d7668a01c185d3841 |
|https://github.com/o3de/o3de/commit/903936edc736ef3a0ba8ea1d7668a01c185d3841 | dependent commit for 212d3c4d32cfcca4d960654b82d68de3d3b5f924|
|https://github.com/o3de/o3de/commit/67763caae64d05b2effa0ec5f7173dc93c98e329 | dependent commit for 212d3c4d32cfcca4d960654b82d68de3d3b5f924|
|https://github.com/o3de/o3de/commit/212d3c4d32cfcca4d960654b82d68de3d3b5f924 | adds support for the startup configuration file, new `sys_DumpAllocationRecords*` console commands and accurate memory tracking of child allocators.|


### Piggybacking off of an Existing allocator

To piggyback off of an existing allocator, The base allocator class can be wrapped in the `AZ::ChildAllocatorSchema` template.  
An inherited class should then be added that implements the `AZ_RTTI` macro which allows providing a user readable name for the new allocator itself.
The `AZ_CHILD_ALLOCATOR` macro that encapsulates these steps

#### Ex. Create an allocator for the PrefabSystem that uses the SystemAllocator for it's allocation
```c++
AZ_CHILD_ALLOCATOR(PrefabSystemAllocator, "{CD8443AE-BC56-4C46-AF3A-6633C2C0E694}", AZ::SystemAllocator);
```

The above macro creates the `PrefabSystemAllocator`, which inherits from the ChildAllocatorSchema class template and standard container allocator alias of `PrefabSystemAllocator_for_std_t`.  
The `PrefabSystemAllocator` can be used to specify the allocator for class using the `AZ_CLASS_ALLOCATOR` macro.  
The `PrefabSystemAllocator_for_std_t` is for use with the AZStd container types such as an `AZStd::vector`, `AZStd::basic_string`, `AZStd::unordered_map`, etc... for use as a standard container allocator.

Side Note: A random UUID should be generated for the new allocator.
This can be done on Windows using the Create GUID tool which is available in the Visual Studio Tools -> Create Guid menu.  
![image](https://github.com/o3de/o3de/assets/56135373/b02d6a62-535c-46e3-ba17-e6cfc42d25a7)

Alternatively a random UUID can be created using the Python [UUID](https://docs.python.org/3/library/uuid.html#command-line-usage) module using the  following one liner
```bash
python -c "import uuid; print(f'{{{str(uuid.uuid4()).upper()}}}');"
```

### Using the PrefabSystemAllocator for prefab classes/structures

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

### Output the total memory for allocators uses at runtime memory at runtime

The [sys_DumpAllocator](https://github.com/o3de/o3de/pull/16864) Console Command can be used via the AZ Console system to dump the memory allocations of any active allocations.

Sample output for the user of the Console Variable is below.
It is setup to be able to placed into a CSV file to aid in data analysis.
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

**NOTE: There is currently a max limit of [100 registered allocators](https://github.com/o3de/o3de/blob/212d3c4d32cfcca4d960654b82d68de3d3b5f924/Code/Framework/AzCore/AzCore/Memory/AllocatorManager.h#L155-L156)**.

## How to add allocation record tracking for allocators

In O3DE Allocator have support for recording the address, size and callstack of each allocation that occurs when using a specific allocator.  
This can be done by turning on allocation record tracking as well as profiling for the allocator itself.  
By default this is only enabled by default in the C++ Unit Test framework when using the [LeakDectionFixture](https://github.com/o3de/o3de/blob/212d3c4d32cfcca4d960654b82d68de3d3b5f924/Code/Framework/AzCore/AzCore/UnitTest/TestTypes.h#L109-L115) for Googletest fixtures.  

In order to enable allocator recording in other runtime applications such as the GameLauncher and Editor, support for a startup configuration file has been added to O3DE that can read a Windows-style INI file that specifies the name of allocator that is registered with the Allocation system and a value for level of allocation recording that should occur.

Support for loading of the startup configuration file can be overridden by the [O3DE_STARTUP_CFG_FILE_CHECK_OVERRIDES](https://github.com/o3de/o3de/blob/212d3c4d32cfcca4d960654b82d68de3d3b5f924/Code/Framework/AzCore/CMakeLists.txt#L40-L43) CMake Cache variable when configuring.  
By default the startup config file would only be loaded in `debug` and `profile` configurations and not in `release` configurations, but can overridden by specifying the `O3DE_STARTUP_CFG_FILE_CHECK_OVERRIDES` CMake cache option above.  

### Startup config file search locations

The path to the startup config file is searched in the following locations:  
1. ~/.o3de/Registry/startup.cfg (`~` stands for the user home directory. i.e `/home/<user>` on Linux, `C:/Users/<user>` on Windows, `/Users/<user>` on MacOS.
1. `<executable-directory>/Registry/startup.cfg`.
1. The value of the `O3DE_STARTUP_CFG_FILE` environment variable if set.
1. The value of the `--startup-cfg-file` command line argument.

### Allocator record tracking settings

The specifics of enabling allocation tracking for an allocator involves adding a setting to the startup configuration file of the form:  
`allocator_tracking_<allocator-name> = <AllocationRecordMode>`  

The "\<AllocationRecordMode>" value must be match one of the options of the [AllocationRecordMode](https://github.com/o3de/o3de/blob/212d3c4d32cfcca4d960654b82d68de3d3b5f924/Code/Framework/AzCore/AzCore/Memory/AllocationRecords.h#L89-L92) enum.

When the value of the `allocator_tracking_<allocator-name>` key is set to a value above the `RECORD_NO_RECORDS` enum, allocation record tracking becomes enabled.

The following is a sample startup.cfg file that illustrates how to turn on Allocation tracking.

The names of allocators can be found at runtime by using the `sys_DumpAllocators` command in the previous [section](#output-the-total-memory-for-allocators-uses-at-runtime-memory-at-runtime).

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

### Dump Allocation records for allocators at runtime

There are also several Console Commands, that allows users to 
|Console Command| Usage | Description | Examples |
|---|---|---|---|
|`sys_DumpAllocationRecordsToStdout`| `sys_DumpAllocationRecordsToStdout [<allocator name> ...]` | dumps allocation records for a set of specified allocators (or all allocators if the no allocator name arguments are provided) to stdout.| `sys_DumpAllocationRecordsToStdout`<br/>`sys_DumpAllocationRecordsToStdout SystemAllocator`|
|`sys_DumpAllocationRecordsToFile`| `sys_DumpAllocationRecordsToFile <filepath> [<allocator name> ...]` | dumps allocation records for a set of specified allocators (or all allocators if the no allocator name arguments are provided) to the supplied filepath.<br>relative files paths are treated relative to current working directory of the application.| `sys_DumpAllocationRecordsToFile foo.txt`<br/>`sys_DumpAllocationRecordsToFile bar.txt OSAllocator`|
|`sys_DumpAllocationRecordsToDevWriteStorage`| `sys_DumpAllocationRecordsToDevWriteStorage [<allocator name> ...]` | dumps allocation records for a set of specified allocators (or all allocators if the no allocator name arguments are provided) to the developer writable storage directory.<br/>The developer writable storage directory on the Host platforms of [Windows, Linux and MacOS](https://github.com/o3de/o3de/blob/67bab671e1eb66120f812e1e7b70e57402076c41/Code/Framework/AzCore/AzCore/Settings/SettingsRegistryMergeUtils.cpp#L740-L753) are the `<project-root>/user` directory.<br/>On mobile platforms such as [Android](https://github.com/o3de/o3de/blob/67bab671e1eb66120f812e1e7b70e57402076c41/Code/Framework/AzCore/Platform/Android/AzCore/Android/AndroidEnv.h#L110-L112) and [IOS](https://github.com/o3de/o3de/blob/67bab671e1eb66120f812e1e7b70e57402076c41/Code/Framework/AzCore/Platform/iOS/AzCore/Utils/Utils_iOS.mm#L26) it is the public storage provided by OS when the app is installed on the device.<br/><br/>Specifically the file path for the allocation records are located at:<br/>`<dev-write-storage>/application_records/records.<ISO8601Timestamp>.<process-id>.log`| `sys_DumpAllocationRecordsToDevWriteStorage`<br/>`sys_DumpAllocationRecordsToDevWriteStorage PoolAllocator`|
|`sys_DumpAllocationRecordsInRange`| `sys_DumpAllocationRecordsInRange <min-inclusive-index> <max-exclusive-index> [<allocator name> ...]` | dumps allocation records in the specified index range using the first two parameters to the console command for a set of specified allocators (or all allocators if the no allocator name arguments are provided).<br/>This command will dump the allocations in the specified range up to the max index to stdout.| `sys_DumpAllocationRecordsToStdout 0 100`<br/>`sys_DumpAllocationRecordsInRange 0 1000 EntityAllocator`|

#### Running the console command to dump the OSAllocator allocation records
The console output could looks as follows
```
sys_DumpAllocationRecordsToDevWriteStorage OSAllocator
[CONSOLE] Executing console command 'sys_DumpAllocationRecordsToDevWriteStorage OSAllocator'
(mem) - Printing allocation records for allocator OSAllocator. Estimated time to print all records is 1 seconds
(mem) - Printed 142 allocations in 1 seconds for allocator "OSAllocator" (71 records per seconds)
```

Running the console command would result in a log file being output to the `<project-root>/user/allocation_records` directory.
![image](https://github.com/o3de/o3de/assets/56135373/f11bbfae-4f08-4a38-a68c-745cd41bcf92)

Here is a sample of a dumped record file: [records.2023-11-03T205715Z.51804.log](https://github.com/o3de/o3de/files/13254688/records.2023-11-03T205715Z.51804.log)
