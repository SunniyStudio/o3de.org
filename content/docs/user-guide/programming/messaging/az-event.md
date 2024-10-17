---
linktitle: AZ::Event
title: AZ::Event
description: 使用 AZ::Event 模板类订阅和发布跨游戏组件的信息。
weight: 500
---

`AZ::Event` 模板类用于在游戏的不同组件中订阅和发布单值消息。它旨在取代目前使用 EBus 实现的基于值的事件发布/子模式，只是语法要简单得多。这种新系统有很多优点，包括代码更简单、文件更少、去除了处理程序只关心事件子集的聚合接口，以及在向已注册的处理程序分派值变化时提高了运行时性能。

`AZ::Event` 被定义为 C++ 模板 \(`template <typename... Params>`\) ，位于以下头文件中：`%INSTALL-ROOT%\dev\Code\Framework\AzCore\AzCore\Ebus\Event.h`

`AZ::Event `限制包括以下方面：
* 事件系统只支持单线程。处理程序应在调度事件的同一线程上进行`Connect()` 和 `Disconnect()`。
* 处理程序只能绑定到现有的事件实例。在事件创建之前，无法绑定到该事件（就像通过 ID EBus 绑定地址一样）。
* 处理程序只能绑定到一个事件。一个处理程序不能绑定多个事件。
* 处理程序没有返回结果。处理程序函数签名的返回结果必须为空。
* 没有事件队列。队列可以作为模块化处理程序包装器构建，但在单线程实现中，所有事件都会立即分派给所有处理程序。

`AZ::Event` 提供了一个 `Handler` 类和以下显式构造函数：
+ `Handler(std::nullptr_t)`
+  `Handler(Callback callback)`
+ `Handler(const Handler& rhs)`
+ `Handler(Handler&& rhs)`

`AZ::Event::Handler`中定义了以下方法：
+ 要连接到`Handler`实例: `void Connect(Event<Params...>& event);`
+ 要从`Handler`实例断开连接：`void Disconnect();`

**使用示例**
+ 要创建要处理的事件，请使用以下 C++ 语法声明一个 `AZ::Event` 实例：

  `AZ::Event<{type}> {name_of_event};`

  例如，声明一个可以发布布尔值的事件：

  `AZ::Event<bool> isPlayerActive;`
+ 声明一个处理程序，在事件发出信号时对其进行处理：

  `AZ::Event<bool>::Handler playerActiveHandler([]({type} value) {});`

  例如，为上一个示例中的事件创建一个处理程序：

  `AZ::Event<bool>::Handler playerActiveHandler([](bool value) {});`

在头文件中声明事件和处理程序后，就可以连接到事件并发出信号。下面是一个使用前面示例中的声明和调用的简单示例：

```
// Declaration in your header
AZ::Event<bool> isPlayerActive; // Declare the event
AZ::Event<bool>::Handler playerActiveHandler([](bool value) {}); // Declare our handler

// Usage in your code
handler.Connect(isPlayerActive); // Connect the handler to to our event
// ...
isPlayerActive.Signal(true); // Signal the event to inform subscribers that the player is active
```

下面是一个更复杂的示例，它通过一个类来处理多个事件信号：

```
class ExampleEventComponent
   : public AZ::Component
{
public:
    using Event1Type = AZ::Event<const AZ::Vector3&>;
    using Event2Type = AZ::Event<float, float>;

    void Tick()
    {
        // Update component state
        if (value1Changed)
        {
            m_event1.Signal(value1);
        }

        if (value2Changed)
        {
            m_event2.Signal(value2.x, value2.y);
        }
    }

    void ConnectEvent1Handler(Event1Type::Handler& handler) { handler.Connect(m_event1); }
    void ConnectEvent2Handler(Event2Type::Handler& handler) { handler.Connect(m_event2); }
private:
    Event1Type m_event1;
    Event2Type m_event2;
};

class ExampleHandlerComponent
   : public AZ::Component
{
public:
    ExampleHandlerComponent()
       : m_handler1([this](const AZ::Vector3& value) { this->OnEvent1Invoked(value); })
       , m_handler2([this](float value2x, float value2y) { this->m_value2x = value2x; this->m_value2y = value2y;})
    {
    }

    void Activate()
    {
        ExampleEventComponent* eventComponent = GetEntity()->FindComponent<ExampleEventComponent>();
        if (eventComponent)
        {
            eventComponent->ConnectEvent1Handler(m_handler1);
            eventComponent->ConnectEvent2Handler(m_handler2);
        }
    }

    void OnEvent1Invoked(int32_t value) { // do something with value }
private:
    ExampleEventComponent::Event1Type::Handler m_handler1;
    ExampleEventComponent::Event2Type::Handler m_handler2;
};
```

**性能**
`AZ::Event` 甚至比 EBus 的 lambda 语法还要快 20%，比 EBus 的成员函数指针模型快 40% 以上。这些性能差距与处理程序的数量成线性关系，因此无论有 1,000 个处理程序还是 1,000,000 个处理程序，`AZ::Event` 都比使用标准 EBus 成员函数指针快 40%。

为了将 EBus 处理器的实现代码与`AZ::Event`进行比较，下面是一个使用 EBus 发出单个值变化信号的代码示例。

```
// Single-value message handler using EBus

// Bus interface
class EBusEventExample
    : public AZ::EBusTraits
{
public:
    using MutexType = NullMutex;
    static const AZ::EBusHandlerPolicy HandlerPolicy = AZ::EBusHandlerPolicy::Multiple;
    static const AZ::EBusAddressPolicy AddressPolicy = AZ::EBusAddressPolicy::Single;
    virtual void OnSignal(int32_t) = 0;
};
using EBusEventExampleBus = AZ::EBus<EBusEventExample>;

// Bus implementation
class EBusEventExampleImpl
    : public EBusPerfBaselineBus::Handler
{
public:
    EBusEventExampleImpl() { EBusEventExampleBus::Handler::BusConnect(); }
    ~EBusEventExampleImpl() { EBusEventExampleBus::Handler::BusDisconnect(); }
    void OnSignal(int32_t) override {}
};

// Usage
EBusEventExampleImpl handler;
EBusEventExampleBus::Broadcast(&EBusEventExample::OnSignal, 1);
```

下面是一个使用 `AZ::Event`执行相同工作的示例。

```
// Single-value message handler implemented using AZ::Event
AZ::Event<int32_t> event; // Declare the event
AZ::Event<int32_t>::Handler handler([](int32_t value) {}); // Declare our handler

// Usage
handler.Connect(event); // Connect the handler to our event
event.Signal(1); // Signal an event, this will invoke our handler's lambda
```

请注意代码行数的减少以及整体代码模式的简化。请将您当前的一些 EBus 消息处理程序移植到使用 `AZ::Event`，然后使用我们内置的单元测试和基准进行测试。

## 单元测试和基准测试

`AZ::Event`系统包含大量单元测试和基准测试，以验证行为的正确性，并确认与同等 EBus 实现相比的性能优势。

要执行单元测试，可向 `AzTestRunner` 提供以下命令行参数：

%INSTALL-ROOT%\\dev\\Bin64vc141.Test\\AzCoreTests.dll AzRunBenchmarks --pause-on-completion --benchmark\_filter=BM\_EventPerf\*

你应该会看到类似这样的单元测试输出。

```
[==========] Running 7 tests from 1 test case.
[----------] Global test environment set-up.
[----------] 7 tests from EventTests
[ RUN      ] EventTests.TestHasCallback
[       OK ] EventTests.TestHasCallback (0 ms)
[ RUN      ] EventTests.TestScopedConnect
[       OK ] EventTests.TestScopedConnect (0 ms)
[ RUN      ] EventTests.TestEvent
[       OK ] EventTests.TestEvent (1 ms)
[ RUN      ] EventTests.TestEventMultiParam
[       OK ] EventTests.TestEventMultiParam (0 ms)
[ RUN      ] EventTests.TestConnectDuringEvent
[       OK ] EventTests.TestConnectDuringEvent (0 ms)
[ RUN      ] EventTests.TestDisconnectDuringEvent
[       OK ] EventTests.TestDisconnectDuringEvent (0 ms)
[ RUN      ] EventTests.TestDisconnectDuringEventReversed
[       OK ] EventTests.TestDisconnectDuringEventReversed (1 ms)
[----------] 7 tests from EventTests (9 ms total)
```

要执行基准，可向 `AzTestRunner` 提供以下命令行参数：

%INSTALL-ROOT%\\dev\\Bin64vc141.Test\\AzCoreTests.dll AzRunBenchmarks --pause-on-completion --benchmark\_filter=BM\_EventPerf\*

您应该会看到类似这样的基准输出。

```
Benchmark name                     benchmark time   cpu time   iterations
BM_EventPerf_EventEmpty               16869 ns      16881 ns      40727
BM_EventPerf_EventIncrement           20124 ns      20508 ns      37333
BM_EventPerf_EBusEmpty                29421 ns      29157 ns      23579
BM_EventPerf_EBusIncrement            29686 ns      29297 ns      22400
BM_EventPerf_EBusIncrementLambda      24516 ns      24554 ns      28000
```
