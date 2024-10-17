---
linktitle: 深入EBus
title: 深入事件总线
description: 了解有关 Open 3D Engine 中 EBus 的详细信息。
weight: 300
---

事件总线（EBus）是一种调度信息的通用系统。EBus 有许多优点：

* **抽象** - 尽量减少系统间的硬性依赖。
* **事件驱动编程** - 消除轮询模式，使软件更具可扩展性和高性能。
* **更简洁的应用代码** - 安全地发送信息，而无需担心是谁在处理这些信息，或者这些信息是否正在被处理。
* **并发** - 对来自不同线程的事件进行排队，以便在另一个线程或分布式系统应用中安全执行。
* **预测性** - 支持对特定总线上的处理程序进行排序。
* **调试** - 拦截信息，用于报告、剖析和反省。

您可以通过多种不同方式使用 EBus。以下是一些例子：

* 作为直接的全局函数调用
* 将处理分派给多个处理程序
* 对所有调用进行排队，就像命令缓冲区一样
* 作为可寻址邮箱
* 用于强制发送
* 用于队列传送
* 自动将函数调用编入网络信息或其他命令缓冲区

EBus 源代码可在 O3DE 目录中找到`Code/Framework/AZCore/AZCore/EBus/EBus.h`。

## 总线配置

您可以为各种使用模式配置 EBus。本节将介绍常见的配置及其应用。

### 单个处理程序

最简单的配置是多对一（或零）通信总线，很像单机模式。

{{< image-width src="/images/user-guide/programming/messaging/ebus/ebus-in-depth-1.png" width="700" alt="Many to one pattern" >}}

处理程序最多只有一个，任何发送者都可以向其发送事件。发送者无需手动检查和取消引用指针。如果没有处理程序连接到总线，事件将被直接忽略。

```cpp
// One handler is supported.
static const AZ::EBusHandlerPolicy HandlerPolicy = AZ::EBusHandlerPolicy::Single;

// The EBus uses a single address.
static const AZ::EBusAddressPolicy AddressPolicy = AZ::EBusAddressPolicy::Single;
```

### 多个处理程序

另一种常见的配置是可以有许多处理程序的配置。您可以使用这种配置来实现观察者模式、系统事件订阅或通用广播。

{{< image-width src="/images/user-guide/programming/messaging/ebus/ebus-in-depth-2.png" width="700" alt="Many handlers" >}}


处理程序可按已定义或未定义的顺序接收事件。您可以在 `HandlerPolicy` 特质中指定哪种顺序。

#### 无处理程序排序示例

要不按特定顺序处理事件，只需在 `HandlerPolicy` 特质中使用 `Multiple` 关键字，如下例所示：

```cpp
// Multiple handlers. Events received in undefined order.
static const AZ::EBusHandlerPolicy HandlerPolicy = AZ::EBusHandlerPolicy::Multiple;

// The EBus uses a single address.
static const AZ::EBusAddressPolicy AddressPolicy = AZ::EBusAddressPolicy::Single;
```

#### 处理程序排序示例

要按特定顺序处理事件，可在 `HandlerPolicy` 特质中使用 `MultipleAndOrdered` 关键字，然后实现自定义处理程序排序功能，如下例所示：

```cpp
// Multiple handlers. Events received in defined order.
static const AZ::EBusHandlerPolicy HandlerPolicy = AZ::EBusHandlerPolicy::MultipleAndOrdered;

// The EBus uses a single address.
static const AZ::EBusAddressPolicy AddressPolicy = AZ::EBusAddressPolicy::Single;

// Implement a custom handler-ordering function
struct BusHandlerOrderCompare : public AZStd::binary_function<MyBusInterface*, MyBusInterface*, bool>
{
    AZ_FORCE_INLINE bool operator()(const MyBusInterface* left, const MyBusInterface* right) const { return left->GetOrder() < right->GetOrder();  }
};
```

### 具有地址和单一处理程序的 EBus

EBus 还支持基于自定义 ID 的寻址。指向某个 ID 的事件会被连接到该 ID 的处理程序接收。如果广播的事件不带 ID，则所有地址的处理程序都会接收到该事件。

这种方法通常用于单个实体的组件之间或独立但相关实体的组件之间的通信。在这种情况下，实体 ID 就是地址。

{{< image-width src="/images/user-guide/programming/messaging/ebus/ebus-in-depth-3.png" width="700" alt="Addressing based on specific IDs" >}}

#### 地址排序示例

在下面的示例中，带有 ID 的报文不按特定顺序到达每个地址。

```cpp
// One handler per address is supported.
static const AZ::EBusHandlerPolicy HandlerPolicy = AZ::EBusHandlerPolicy::Single;

// The EBus has multiple addresses. Addresses are not ordered.
static const AZ::EBusAddressPolicy AddressPolicy = AZ::EBusAddressPolicy::ById;

// Messages are addressed by EntityId.
using BusIdType = AZ::EntityId;
```

#### 地址排序示例

在下面的示例中，带有 ID 的广播信息会按指定顺序到达每个地址。

```cpp
// One handler per address is supported.
static const AZ::EBusHandlerPolicy HandlerPolicy = AZ::EBusHandlerPolicy::Single;

// The EBus has multiple addresses. Addresses are ordered.
static const AZ::EBusAddressPolicy AddressPolicy = AZ::EBusAddressPolicy::ByIdAndOrdered;

// Messages are addressed by EntityId.
using BusIdType = AZ::EntityId;

// Addresses are ordered by EntityId.
using BusIdOrderCompare = AZStd::greater<BusIdType>;
```

### 带地址和多个处理程序的 EBus

在前面的配置中，每个地址只允许一个处理程序。这通常适用于强制执行特定 ID 的 EBus 所有权，如上面的单例。但是，如果希望每个地址有一个以上的处理程序，可以对 EBus 进行相应的配置：

{{< image-width src="/images/user-guide/programming/messaging/ebus/ebus-in-depth-4.png" width="700" alt="More than one handler per address" >}}

#### 示例： 无地址排序

在下面的示例中，带有 ID 的报文不按特定顺序到达每个地址。在每个地址上，处理程序接收信息的顺序由 `EBusHandlerPolicy` 定义，在本例中只是 `ById`：

```cpp
// Allow any number of handlers per address.
static const AZ::EBusHandlerPolicy HandlerPolicy = AZ::EBusHandlerPolicy::Multiple;

// The EBus has multiple addresses. Addresses are not ordered.
static const AZ::EBusAddressPolicy AddressPolicy = AZ::EBusAddressPolicy::ById;

// Messages are addressed by EntityId.
using BusIdType = AZ::EntityId;
```

#### 示例： 地址排序

在下面的示例中，带有 ID 的广播消息会按指定顺序到达每个地址。在每个地址，处理程序接收消息的顺序由 `EBusHandlerPolicy` 定义，在本例中为 `ByIdAndOrdered`。

```cpp
// Allow any number of handlers per address.
static const AZ::EBusHandlerPolicy HandlerPolicy = AZ::EBusHandlerPolicy::Multiple;

// The EBus has multiple addresses. Addresses are ordered.
static const AZ::EBusAddressPolicy AddressPolicy = AZ::EBusAddressPolicy::ByIdAndOrdered;

// We address the bus EntityId.
using BusIdType = AZ::EntityId;

// Addresses are ordered by EntityId.
using BusIdOrderCompare = AZStd::greater<BusIdType>;
```

## 多线程调度

EBuses 可配置为在多线程环境中使用。锁定策略适用于许多常见用例。

### 单线程

默认情况下，EBus 配置为单线程使用。如果试图在多个线程中使用，则会出现断言。可通过将 `MutexType` 设置为 `NullMutex` 来定义此配置。

```cpp
// This EBus only supports single-threaded usage.
using MutexType = NullMutex;
```

### 多线程与阻塞式派送

要配置 EBus 以允许多个线程进行总线连接、断开连接和事件派发，可将 MutexType 设置为 `AZStd::mutex` 或 `AZStd::recursive_mutex`。EBus 上的每个操作都将锁定互斥，以防止多个线程同时执行。这种配置可确保总线处理程序在处理不同线程上的事件时无法断开连接。对于简单的多线程情况，可以使用 `AZStd::mutex`。但是，如果总线处理程序在处理同一总线上的事件时发送新事件或连接/断开总线，则应选择 `AZStd::recursive_mutex`，以确保单个线程不会陷入死锁。

```cpp
// This EBus supports multi-threaded usage, though only one thread will execute at a time.
using MutexType = AZStd::recursive_mutex;
```

### 共享锁

继承自 `EBusSharedDispatchTraits` 以配置 EBus，使其在总线连接和断开时使用专用锁，但在事件派发时使用共享锁。共享锁允许多个并发事件派发，同时还能确保在事件派发期间不会发生总线连接/断开。这种配置适用于为来自多个线程的请求提供服务的 EBus，以及在应用程序生命周期中频繁连接和断开连接的处理程序。`EBusSharedDispatchTraits`会将 MutexType 和相关的 LockGuard 类型设置为自定义策略，以启用并发事件派发，并确保只有在无事件派发时才会发生连接/断开。

```cpp
// This EBus supports concurrent multi-threaded event dispatches and protects
// from connects / disconnects occuring during event dispatches.
class MyBus : public AZ::EBusSharedDispatchTraits<MyBus>
{
    ...
}
```

### 无锁定派发

要配置 EBus，使其仅在总线连接和断开时锁定，而不在事件派发时锁定，请将`LocklessDispatch` 设为 true。还需要设置 MutexType，以便将 EBus 配置为多线程 EBus 并防止并发连接/断开。如果 EBus 的处理程序只在启动时连接，关闭时断开，并且在总线使用时处理程序不会改变连接状态，那么这一点就非常有用。无锁定派发允许多个事件并发执行，事件派发开销最小。

```cpp
// Locking primitive to use for connects and disconnects.
using MutexType = AZStd::recursive_mutex;

// This EBus supports concurrent multi-threaded event dispatches but does not protect
// from connects / disconnects occuring during event dispatches.
static const bool LocklessDispatch = true;
```

## 同步与异步

EBus 支持同步和异步（队列）消息传递。

### 同步消息

当 EBus 事件被调用时，同步信息会被发送到所有处理程序。同步报文限制了异步编程的机会，但它们具有以下优点：

* 它们不需要存储闭包。参数直接转发给调用者。
* 可以从处理程序（事件返回值）中获取即时结果。
* 没有延迟。

### 异步信息

异步信息具有以下优点：

* 它们为并行性创造了更多机会，也更能适应未来的需要。
* 它们支持从任何线程排队发送消息，并在安全线程（如主线程或您选择的任何线程）上分派消息。
* 用于编写它们的代码本身就能容忍延迟，并能轻松迁移到行为者模型和其他分布式系统中。
* 启动事件的代码的性能并不取决于处理事件的代码的效率。
* 在对性能要求很高的代码中，异步信息可以提高 i-cache 和 d-cache 的性能，因为它们需要调用的虚拟函数更少。

## 其他功能

电子总线包含其他功能，可解决各种模式和用例：

* **缓存可发送信息的指针** - 这对于有 ID 的 EBus 来说非常方便。您可以使用缓存指针来加快调度速度，而不必根据 ID 为每个事件查找 EBus 地址。
* **在 EBus 上队列任何可调用函数** - 使用队列消息传递时，可以针对 EBus 对 Lambda 函数或绑定函数进行队列，以便在另一个线程上执行。这对于通用线程安全队列非常有用。
