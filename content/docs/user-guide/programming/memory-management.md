---
description: ' 了解 Open 3D Engine 中内存管理的最佳实践。 '
title: 内存管理
---

在 O3DE 中管理内存时，请使用 AZ 内存管理调用，避免使用构造函数分配内存或连接 EBus 的静态变量。

## 内存分配

分配内存时，请使用以下推荐做法：
+ 不要直接使用 `new`、`malloc` 和类似的分配器。相反，请使用 AZ 内存管理器调用 `aznew`、`azmalloc`、`azfree`、`azcreate` 和 `azdestroy`。
+ 在每个类中指定分配器。
+ 使用子分配器。为标记和跟踪资源使用情况，新的 gem 和子系统应创建自己的分配器，或创建一个引用现有分配器的 `ChildAllocatorSchema` 。有关创建子分配器的示例，请参阅 [创建分配器](/docs/user-guide/programming/memory/allocators/)。

**原因**: O3DE 的核心 AZ 系统提供了一个内存管理环境，而不是像 C\# 或 Java 中的托管内存那样的原始系统分配器。在 O3DE 中，核心 AZ 系统提供了速度、安全性和跟踪内存使用情况的功能。

有关 O3DE 分配器和新分配器方案的信息，请参阅 [手动分配内存](/docs/user-guide/programming/memory/allocators/)。

## O3DE内存系统初始化

在 [代码](https://github.com/o3de/o3de/blob/development/Code/Framework/AzCore/AzCore/Memory/AllocatorInstance.h#L73-L86) 中首次调用 `AZ::AllocatorInstance<AllocatorType>::Get()` 时，O3DE 通过在 AZ 环境变量中存储 分配器 实例来初始化其内存系统。

为确保分配器在运行应用程序和任何动态加载的 Gem 模块的静态初始化和静态去初始化过程中可用，分配器 AZ 环境变量存储在 `O3DEKernel`共享库中，该共享库是[AzCore 目标](https://github.com/o3de/o3de/blob/development/Code/Framework/AzCore/CMakeLists.txt#L60-L98)的链接库依赖项。

所有 Gem 和 O3DE 应用程序都静态链接`O3DEKernel`目标，因为它们依赖于`AzCore`静态库目标。
在应用程序初始启动时，`O3DEKernel`库是第一个加载的共享库。

由于静态链接共享库的加载发生在静态初始化之前，而卸载发生在应用程序静态去初始化之后，因此 O3DE 分配器在应用程序的整个运行过程中都是可用的。

只存在一个 [AZ::Environnment](https://github.com/o3de/o3de/blob/development/Code/Framework/AzCore/AzCore/Module/Environment.h#L114-L115)实例，并通过 O3DEKernel 通过 `O3DEKernel.dll` (Windows)、`libO3DEKernel.so` (Linux/Android) 或 `libO3DEKernel.dylib` (MacOS/iOS) 中的 DLL 接口访问，以强制使用一个 `AZ::Environment`。
