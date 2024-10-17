---
linktitle: AZ::Interface
title: AZ::Interface
description:  使用 AZ::Interface<T> 模板类为 Open 3D Engine 创建全局消息请求总线。
weight: 400
---

使用 `AZ::Interface<T>` 模板类创建支持 `type T` 系统的全局或应用程序生命周期消息请求总线。该模板类用于实现跨模块边界的注册单件访问。在这种情况下，单例是继承了 `AZ::Interface::Registrar`的类型的实例。一旦注册了单例，就可以通过在单例上执行的代码访问环境变量。您还可以对环境变量进行更改，以便游戏组件的其他部分可以查看这些更改。

一般情况下，当您想从另一个组件调用核心系统（如呈现器或控制台）的方法时，应使用 `AZ::Interface`。

系统是继承了`AZ::Interface`中`Registrar`方法的类的实例。使用`AZ::Interface`注册的系统旨在取代目前使用 EBus 实现的全局或应用程序生命周期请求总线。这一新系统有许多优点，包括性能大大提高以及与集成开发环境标准代码自动完成功能兼容。

{{< note >}}
在这种用法中，**系统**是 O3DE 的关键部分。例如渲染器、控制台、音频系统、输入系统和人工智能寻路系统。使用 `AZ::Interface`，您可以用这种简化的语法访问这些系统：

`AZ::Interface<{system-interface-here}>->Get()->PerformCommand`
例如，`AZ::Interface<IAudio>->Get()->PlaySound();`

同样，对于使用 [AZ::Console](./az-console) 声明的控制台函数（cfuncs），也可使用此语法跨系统调用行为。
{{< /note >}}

`AZ::Interface<T>` 与使用单一处理程序 EBus 相比，它有许多重大改进，例如
+ 提高性能。对单例的调用是一种虚拟函数调用，编译器通常甚至可以将其去虚拟化，而不是对虚拟调用进行锁/列表遍历/函数分派。
+ 提高调试能力。`AZ::Interface` 本质上只是一个`AZ::Environment`变量包装器，可在 O3DE 中实现可扩展的单子。
+ 与 Visual Studio 中的代码自动完成功能兼容。

`AZ::Interface` 被定义为C++模板 \(`template <T>`\)，位于以下头文件中：`%INSTALL-ROOT%dev\Code\Framework\AzCore\AzCore\Interface\Interface.h`

**使用AZ::Interface**
这是使用 `AZ::Interface` 为系统注册单例线程的过程
+ 获取用于注册的 `type T` 类实例的原始接口指针。您可以假定注册系统的寿命将超过任何缓存引用。
+ 在初始化时，通过调用接口引用的 `Register()`，将系统与接口注册在一起。
+ 等待，如果注册成功，就成功连接了一个 `AZ::Environment`（包含游戏的环境变量），并准备接收更新环境变量的信息。

要取消注册系统，请在 `AZ::Interface` 上调用 `Unregister()`。

`AZ::Interface` 定义了以下静态方法：
+ `static void Register(T* type)` - 注册`type T`实例到`AZ::Interface`.
+ `static void Unregister(T* type)` - 从`AZ::Interface`取消注册`type T`。
+ `static T* Get() `- 获取注册到`AZ::Interface`的`type T`的实例的引用。

它还定义了一个辅助类`Registrar`，可分别在`AZ::Interface`类的构造函数和析构函数中实现注册和注销。

```
/**
         * A helper utility RAII mixin class that will register / unregister within the constructor / destructor, respectively.
         *
         * Example Usage:
         * @code{.cpp}
         *      class System
         *          : public Interface<ISystem>::Registrar
         *      {
         *      };
         * @endcode
         */
        class Registrar
            : public T
        {
        public:
            Registrar();
            virtual ~Registrar();
        };--
```

在大多数情况下，您需要使用 `Registrar` 而不是直接实现注册。

下面是一个使用 `AZ::Interface::Registrar`注册一个系统的例子，该系统只定义了一个方法 `DoSomething()`。

```
class ISystem
{
    public:
        virtual ~ISystem();
        virtual void DoSomething() = 0;
};

class System
    : public AZ::Interface<ISystem>::Registrar
{
    public:
        void DoSomething() override;
};

// In client code.

// Check that the pointer is valid before use.
if (ISystem* system = AZ::Interface<ISystem>::Get())
{
    system->DoSomething();
}
```

{{< important >}}
对 `AZ::Interface` 的限制与单处理器 EBus 类似：
仅在长期存在的实例上使用 `AZ::Interface`，例如带有全局变量的实例，这些变量在模块或应用程序的整个生命周期中都存在。

由于`AZ::Interface`跨越 DLL 边界使用`AZ::Environment`变量，因此您只能在成功注册后附加`AZ::Environment`实例后才能注册/取消注册。
`AZ::Interface` 可与 EBus 配合使用，您可以通过为同组请求提供一个 `AZ::Interface<T>` 处理程序来软迁移 EBus 代码。

***线程安全是您的责任***。使用 `AZ::Interface<T>`并不能保证线程的安全。
{{< /important >}}

**与AZ::Event相比**
`AZ::Event`是一个发布/订阅（pub/sub）事件处理程序，当你想在同一线程上订阅另一个组件的通知时可以使用它。另一方面，`AZ::Interface`可以替代单子，用于调用核心系统（如渲染器或控制台）的方法。

## 从 EBus 实现转换

下面是将全局请求总线转换为`AZ::Interface<T>`的示例。

**示例 原有的原始 EBus 基线**

```
// Bus interface
class EBusRequests
    : public AZ::EBusTraits
{
public:
    using MutexType = NullMutex;
    static const AZ::EBusHandlerPolicy HandlerPolicy = AZ::EBusHandlerPolicy::Single;
    static const AZ::EBusAddressPolicy AddressPolicy = AZ::EBusAddressPolicy::Single;
    virtual void Request(int value) = 0;
};
using EBusEventExampleBus = AZ::EBus<EBusEventExample>;

// Bus implementation
class EBusEventExampleImpl
    : public EBusPerfBaselineBus::Handler
{
public:
    EBusEventExampleImpl() { EBusEventExampleBus::Handler::BusConnect(); }
    ~EBusEventExampleImpl() { EBusEventExampleBus::Handler::BusDisconnect(); }
    void Request(int value) override;
};


// Invoke a request
EBusRequestsBus::Broadcast(&EBusRequests::Request, 1);
```

**转换为使用 AZ::Interface 的 EBus 实现示例**
要从全局 EBus 转换为使用`AZ::Interface`但仍可与 Script Canvas 交互的全局请求总线，必须进行一些更改：
+ 创建纯虚接口，无需任何 EBus 代码。
+ 创建一个继承自 `AZ::EBusTraits` 的 EBus 封装器，并将 EBus 声明为 `AZ::EBus<{your-interface-class-name}, {your-ebus-wrapper-name}>`
+ 创建接口的实现，从 EBus 封装处理程序继承，并执行以下操作：
  + 在类的构造函数中，调用 `Register()` 和 `BusConnect()`。
  + 在类的析构函数中，调用 `Unregister()` 和 `BusDisconnect()`。

```
// Our pure-virtual interface only
class IRequests
{
public:
    virtual void Request(int value) = 0;
};

// EBus stuff
class EBusStuff
    : public AZ::EBusTraits
{
public:
    using MutexType = NullMutex;
    static const AZ::EBusHandlerPolicy HandlerPolicy = AZ::EBusHandlerPolicy::Single;
    static const AZ::EBusAddressPolicy AddressPolicy = AZ::EBusAddressPolicy::Single;
};
using EBusStuffBus = AZ::EBus<IRequests, EBusStuff>; // Note we specify the pure virtual interface first, and then the EBus stuff after

// Implementation, inherit from the pure-virtual interface
class RequestsImpl
    : public EBusStuffBus::Handler // Note we inherit from the bus handler
{
public:
    AZ_RTTI(RequestsImpl, "{some guid}", IRequests); // AZ type info is required

    RequestsImpl()
    {
        AZ::Interface<IRequests>::Register(this);
        EBusStuffBus::Handler::BusConnect();
    }

    ~RequestsImpl()
    {
        EBusStuffBus::Handler::BusDisconnect();
        AZ::Interface<IRequests>::Unregister(this);
    }

    void Request(int value) override;
};

// Invoke a request
AZ::Interface<IRequests>::Get()->Request(1);
```

**示例**
如果不需要与 Script Canvas 交互，则可以完全避免使用 EBus，如本例所示。

```
// Our pure-virtual interface only
class IRequests
{
public:
    virtual void Request(int value) = 0;
};

// Implementation
class RequestsImpl
    : public IRequests
{
public:
    AZ_RTTI(RequestsImpl, "{some guid}", IRequests); // AZ type info is required

    RequestsImpl()
    {
        AZ::Interface<IRequests>::Register(this);
    }

    ~RequestsImpl()
    {
        AZ::Interface<IRequests>::Unregister(this);
    }

    void Request(int value) override;
};

// Invoke a request
AZ::Interface<IRequests>::Get()->Request(1);
```

## 单元测试 

`AZ::Interface`系统包含大量单元测试，用于验证行为的正确性。

要执行单元测试，可向 `AzTestRunner` 提供以下命令行参数：

%INSTALL-ROOT%\\dev\\Bin64vc141.Test\\AzCoreTests.dll AzRunUnitTests --pause-on-completion --gtest\_break\_on\_failure --gtest\_filter=InterfaceTest\*

你应该会看到类似这样的单元测试输出：

```
[==========] Running 6 tests from 1 test case.
[----------] Global test environment set-up.
[----------] 6 tests from InterfaceTest
[ RUN      ] InterfaceTest.EmptyInterfaceTest
[       OK ] InterfaceTest.EmptyInterfaceTest (2 ms)
[ RUN      ] InterfaceTest.EmptyAfterDisconnectTest
[       OK ] InterfaceTest.EmptyAfterDisconnectTest (0 ms)
[ RUN      ] InterfaceTest.ValidInterfaceTest
[       OK ] InterfaceTest.ValidInterfaceTest (0 ms)
[ RUN      ] InterfaceTest.RegistrarTest
[       OK ] InterfaceTest.RegistrarTest (0 ms)
[ RUN      ] InterfaceTest.RegisterTwiceAssertTest
[       OK ] InterfaceTest.RegisterTwiceAssertTest (1 ms)
[ RUN      ] InterfaceTest.RegisterMismatchTest
[       OK ] InterfaceTest.RegisterMismatchTest (0 ms)
[----------] 6 tests from InterfaceTest (4 ms total)
BM_EventPerf_EBusIncrementLambda      24516 ns      24554 ns      28000
```
