---
description: ' 在 Open 3D Engine 中分配和跟踪内存。 '
title: 在 O3DE 中使用内存分配器
---

O3DE 的内存管理系统决定了内存的分配方式。所有内存分配都通过一条流水线进行，并且可以跟踪内存分配情况。这样就能更方便快捷地找出内存泄漏点或优化内存使用，从而提高游戏性能。这一改进对于移动平台尤为重要，因为移动平台的内存资源通常比 PC 环境更为紧张。

O3DE 支持几种已知的内存分配方案。您可以使用 O3DE 的分配器对分配进行分类，或将类似的分配放在一起，以提高定位性或减少碎片。

{{< note >}}
有关在 O3DE 中管理内存的最佳 C++ 实践，请参阅 [内存管理](/docs/user-guide/programming/memory-management)。
{{< /note >}}

## AZ内存分配器 

下图说明了 AZ 内存分配器的层次结构。


![AZ memory allocator hierarchy](/images/user-guide/programming/memory/memory-allocators.svg)

+ **`OSAllocator`** - 作为操作系统内存的接口，应该用于在 C 堆上直接分配操作系统内存。在首次调用 `AllocatorInstance<OSAllocator>::Get()` 时，会对 `OSAllocator` 进行懒惰初始化。

  `OSAllocator` 使用操作系统系统调用分配内存。这些调用默认情况下不会被记录或跟踪，但可以通过 [分配器标记指南 wiki 启动配置](https://github.com/o3de/o3de/wiki/Allocator-Tagging-Guide#startupcfg) 部分进行记录或跟踪。其他分配器使用 `OSAllocator` 从操作系统获取内存。
+ **`SystemAllocator`** - 系统分配器是 AZ 内存库的通用分配器。它通过首次调用 `AllocatorInstance<SystemAllocator>::Get()` 进行初始化。所有其他分配器都使用 `SystemAllocator` 进行内部分配。
+ **`PoolAllocator`** - 执行极快的小对象内存分配。`PoolAllocator` 可分配的内存大小范围为 `m_minAllocationSize` 至 `m_maxPoolSize`。

    {{< note >}}
`PoolAllocator` 不是线程安全的。如果需要线程安全版本，请使用 `ThreadPoolAllocator` 或从 `ThreadPoolBase` 继承，然后编写自定义代码来处理同步。
{{< /note >}}

+ **`ThreadPoolAllocator`** - 线程安全池分配器。如果想创建自己的线程池堆，请继承自 `ThreadPoolBase`，因为 O3DE 要求为分配器类型创建一个唯一的静态变量。

## 在类中应用分配器

要在类中应用分配器，请在类中使用`AZ_CLASS_ALLOCATOR`宏或直接调用`AZ::AllocatorInstance`<*some\_allocator*>。

AZCore 依靠 `AZ_CLASS_ALLOCATOR` 为类指定默认分配器，或依靠在签名中指定分配器的显式 `azcreate` 和 `azdestroy` 调用。
+ 如果您的类没有实现 `AZ_CLASS_ALLOCATOR`，对 `new` 或 `delete` 的调用将使用全局的 `operator new` 或 `operator delete`。
+ 如果您的类没有实现 `AZ_CLASS_ALLOCATOR`，而您又调用了 `aznew` 时，编译错误将被触发，表明 `operator new` 必须被覆盖。

## AZ 分配器模式

每个分配器通常都会实现 `IAllocator` 接口，并使用模式来实现分配算法和簿记。通过这种策略，可以在多个分配器中使用相同的模式。



**分配器模式**

| 模式 | 说明 |
| --- | --- |
| AZ::HphaSchema | 这是首选模式。它结合了用于小规模分配的小块分配器和用于大规模分配的[红黑树](https://en.wikipedia.org/wiki/Red-black_tree)。这提供了良好的通用性能。如果不确定使用哪种模式，请使用该模式。 HphaSchema “基于 Dimitar Lazarov 的 ”高性能堆分配器"（Game Programming Gems 7，Charles River Media，2008 年，第 15-23 页）。   |
| AZ::ChildAllocatorSchema |  作为另一个分配器的直通模式。使用此模式可在现有分配器（如 `SystemAllocator`）的基础上创建新的分配器。为正确标记每个 gem 或逻辑子系统分配的内存，每个 gem 或子系统可创建自己的子分配器。更多信息，请参阅 [创建分配器](#memory-allocators-creating-an-allocator)。  |
| AZ::PoolSchema |  一种实现小块分配器的专用模式，用于管理小规模、高吞吐量的分配。对象池通常以使用更多内存为代价。 `PoolSchema` 不是线程安全的。如果需要线程安全版本，请使用 `ThreadPoolSchema` 或编写自定义代码来处理同步。  |
| AZ::ThreadPoolSchema |  线程安全池模式，使用线程本地存储为每个线程实现一个小块分配器。 由于线程池分配器会为每个线程创建单独的池，因此会占用更多内存，尤其是在池大小固定的情况下。  |

## 创建分配器

我们建议每个 O3DE gem 或逻辑子系统创建一个`ChildAllocatorSchema`，以正确标记其分配的内存。这种做法可以更容易地预算资源使用情况，并获得整体视图。
标记的内存使用情况可以通过几种方式查看或写入文件，包括运行`sys_DumpAllocators`或`sys_DumpAllocationRecordsToFile`控制台命令。

如果您选择编写自己的模式，请注意缓存大量内存可能会造成问题。这种缓存会妨碍其他系统适应游戏内容的能力。除非您有特殊要求，否则我们建议您创建一个最终使用`SystemAllocator`的 `ChildAllocatorSchema`。使用 `ChildAllocatorSchema` 可确保您的内存尽可能可恢复和可重用。

**创建分配器**

1. 选择要使用的模式、编写自定义模式或选择要修改的现有分配器。更多信息，请参阅 [AZ 分配器模式］(#allocator-schemas)。

1. 从 `AllocatorBase`<*your\_schema*> 继承，创建你的 `Allocator` 类。

1. 添加 `AZ_RTTI`，以便 `AllocatorInstance<>` 可以正确管理您的类型。

### 从容器中使用自己的分配器

要从容器中使用自己的分配器，请将分配器封装在 `AZ::AZStdAlloc`中，如下面的示例。

```
using CustomAllocator_for_std_t = AZ::AZStdAlloc<CustomAllocator>;
AZStd::vector<MyClass, CustomAllocator_for_std_t>;
```

### 子代分配器示例

有关如何使用 `ChildAllocatorSchema` 的示例，请参阅 O3DE Wiki 上的 [分配器标记指南](https://github.com/o3de/o3de/wiki/Allocator-Tagging-Guide)。

