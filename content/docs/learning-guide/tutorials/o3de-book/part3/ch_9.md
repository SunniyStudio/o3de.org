---
linkTitle: 第9章 使用AZ::TickBus
title: 第9章 使用AZ::TickBus
description: 第9章 使用AZ::TickBus
---
# 第9章 使用AZ::TickBus
Tick: 你不会发疯的。你在一个疯狂的世界里变得理智了！
——蜱虫（TV Series 1994-1997）

## AZ::TickBus 简介

{{<note>}}
您可以在 GitHub 上找到本章的代码：
https://github.com/AMZN-Olex/O3DEBookCode2111/tree/ch09_oscillator
{{</note>}}

本章包括以下内容：
* 如何使用 TickBus。
* 使用 TickBus 写入 OscillatorComponent。
* 更深入地研究 Activate 和 Deactivate 方法。
* 声明同一实体的组件之间的依赖关系。

除了在另一个组件上调用事件总线（例如调用 TransformBus）之外，我们还需要知道如何注册事件总线。要注册的常见事件总线是 AZ::TickBus。它允许在每个游戏帧上执行一个作，在 O3DE 中也称为游戏更新。

学习如何使用 EBus 的一个好方法是实现一个使用 EBus 的组件。本章将编写一个新的 C++ 组件： Oscillator 组件。它将使用 TransformBus 和 TickBus 振荡它所附加到的实体的位置。

查看 C:\O3DE\21.11.2\Code\Framework\AzCore\AzCore\Component\TickBus.h 中 AZ::TickBus 的定义：
```c++
/**
* The EBus for tick notification events.
* The events are defined in the AZ::TickEvents class.
*/
typedef AZ::EBus<TickEvents>    TickBus;
```

查看上面评论中提到的 AZ::TickEvents：
```c++
class TickEvents : public AZ::EBusTraits
{
  ...
  /**
  * Signals that the application has issued a tick.
  * @param deltaTime The delta (in seconds) from
  *        the previous tick and the current time.
  * @param time The current time.
  */
  virtual void OnTick(float deltaTime, ScriptTimePoint time) = 0;
```

OnTick 是我们覆盖以获取 tick 事件的回调。让我们看看如何创建一个处理它的组件。

## OscillatorComponent
我们将创建一个新组件 OscillatorComponent。以下是头文件的一部分，用于入门。

{{<note>}}
请参阅前面的章节如何创建新组件。
{{</note>}}

例 9.1.OscillatorComponent.h 代码段
```c++
// An example of singing up to an Ebus, TickBus in this case
 class OscillatorComponent
        : public AZ::Component
        , public AZ::TickBus::Handler // for ticking events
    {
 public:
   // be sure this guid is unique, avoid copy-paste errors!
          AZ_COMPONENT(OscillatorComponent,
   "{302AE5A0-F7C4-4319-8023-B1ADF53E1E72}");
   protected:
   // AZ::Component overrides
   void Activate() override;
   void Deactivate() override;
   // AZ::TickBus overrides
   void OnTick(float dt, AZ::ScriptTimePoint) override;
   // what other components does this component require?
   static void GetRequiredServices(
              AZ::ComponentDescriptor::DependencyArrayType& req);
   ...
    };
```

{{<note>}}
这就是我喜欢事件总线的原因 - 您可以保护所有方法，并且没有人可以与您的组件通信，除非它们通过一个经过批准的接口，即其事件总线。这就像有个律师一样！
{{</note>}}

### 从 EBus 处理程序继承
如果您希望注册事件总线事件，则必须从其处理程序继承，例如：
AZ::TickBus::Handler

### AZ::Component 重写
这是一个组件，因此它必须继承自 AZ::Component。Activate 和 Deactivate 是纯虚拟方法，必须重写和实现。我们之前已经浏览了这些方法，所以让我们花点时间更深入地了解它们。

{{<note>}}
AZ::Component 在 C:\O3DE\21.11.2\Code\Framework\AzCore\AzCore\Component\Component.h 中定义
{{</note>}}

Activate() 的源代码注释指出：“[Activate()] 将组件置于活动状态。系统在激活拥有该组件的每个实体期间调用此函数一次。您必须覆盖此函数。仅当组件所依赖的所有服务和组件都存在且处于活动状态时，系统才会调用该组件的 Activate() 函数。使用 GetProvidedServices 和 GetDependentServices 指定这些依赖项。

对于 Deactivate()方法：“停用组件。当业主实体被停用时，系统会调用此函数。您必须覆盖此函数。作为最佳实践，请确保此函数将组件返回到最小的占用空间。停用的顺序与激活顺序相反，因此您的组件在它所依赖的组件之前被停用。系统始终在销毁组件之前调用组件的 Deactivate()函数。但是，停用并不总是
然后是组件的销毁。实体及其组件可以在不被销毁的情况下被停用和激活。确保您的 Deactivate()实现可以处理这种情况。

换句话说，当您的组件开始工作并期望执行其工作时，O3DE 将调用 Activate。而 Deactivate 告诉您的组件它必须停止工作。它可能会在之后被删除，或者在一段时间内保持未使用状态，稍后再次激活。

### 覆盖事件总线事件
为了接收 OnTick 事件，必须覆盖它们。OnTick 是一种虚拟方法。

### 依赖项声明
GetRequiredServices 是四 （4） 个静态方法中的一种特殊静态方法，任何组件都可以声明这些方法来定义它与同一实体上其他组件的关系。在这种情况下，Oscillator Component 将表示它需要实体上存在 TransformComponent。其实现如下：

```c++
void OscillatorComponent::GetRequiredServices(AZ::ComponentDescriptor::DependencyArrayType& req)
{
  // OscillatorComponent requires TransformComponent
  req.push_back(AZ_CRC_CE("TransformService"));
}
```

我们如何知道使用 “TransformService” 值？要知道这一点，唯一的方法是查看 TransformComponent：：GetProvidedServices：
```c++
void TransformComponent::GetProvidedServices(AZ::ComponentDescriptor::DependencyArrayType& provided)
{
  provided.push_back(AZ_CRC_CE("TransformService"));
}
```

通常，组件有四种不同的方式来表示与其他组件的各种关系，方法是：GetRequiredServices、GetProvidedServices、GetDependentServices 和 GetIncompatibleServices。O3DE 代码中的注释非常适合解释这些方法的作用，因此它们在这里：
* GetRequiredServices
```c++
/**
* Specifies the services that the component requires.
* The system activates the required services before it activates
* this component. It also deactivates the required services
* after it deactivates this component. If a required service is
* missing before this component is activated, the system returns
* an error and does not activate this component.
*/
```

* GetProvidedServices
```c++
/**
* Specifies the services that the component provides.
* The system uses this information to determine when to
* create the component.
*/
```

* GetDependentServices
```
/**
* Specifies the services that the component depends on, but does
* not require. The system activates the dependent services before
* it activates this component. It also deactivates the dependent
* services after it deactivates this component. If a dependent
* service is missing before this component is activated,
* the system does not return an error and still activates this
* component.
*/
```

* GetIncompatibleServices
```
/**
* Specifies the services that the component cannot operate with.
* For example, if two components provide a similar service and
* the system cannot use the services simultaneously, each of
* those components would specify the other component as an
* incompatible service.
*/
```

{{<note>}}
所有这 4 个 static 方法都有相同的 input 参数：
```c++
  AZ::ComponentDescriptor::DependencyArrayType&
```
{{</note>}}

## 实现 Oscillator 组件
### OscillatorComponent 的设计
通常，振荡意味着使用正弦或余弦数学函数。但是，本章将采用不同的方法。原因如下：为了强制实体沿着三角函数的输出，我们必须保存原点并在每次报价时强制实体的位置。这适用于许多情况。

但是，假设您希望能够在实体振荡时影响实体的位置。例如，您编写了另一个组件 ShoveComponent，该组件将实体横向移动。如果假设的 ShoveComponent 和 OscillatorComponent 可以一起工作，那不是很酷吗？我认为是的，因此 OscillatorComponent 的设计计算了每个价格变动所需的位置变化，并且不保存起始位置。

### 激活 OscillatorComponent
与前面的示例组件 MyComponent 相比，此组件中的主要新功能是 Activate 和 Deactivate 方法的自定义实现。

```c++
void OscillatorComponent::Activate()
{
  // We must connect, otherwise OnTick() will never be called.
  // Forgetting this call is a common error in O3DE!
  AZ::TickBus::Handler::BusConnect();
}
void OscillatorComponent::Deactivate()
{
  // good practice on cleanup to disconnect
  AZ::TickBus::Handler::BusDisconnect();
}
```

MyComponent 是一个空组件，它什么都不做，所以它在激活时没有任何可做的事情。OscillatorComponent 需要注册 AZ：：TickBus。默认情况下，继承不会连接到 bus。组件准备就绪后，您必须手动执行此作。最早可以在 Activate 或更晚的时间中。

{{<note>}}
对于初学者 O3DE 开发人员来说，最常见的错误是忘记连接到总线！如果某些作不起作用，请检查是否已连接到要为其处理事件的总线。
{{</note>}}

{{<note>}}
在您调用 BusConnect 之前，不会调用 OnTick。调用 BusDisconnect 后，将不再调用 OnTick。
{{</note>}}

### 处理 OnTick 事件
例 9.2.处理 AZ::TickBus::Handler::OnTick 的示例

```c++
void OscillatorComponent::OnTick(float dt, AZ::ScriptTimePoint)
{
  m_currentTime += dt;
  // get current position
  AZ::Vector3 position;
  AZ::TransformBus::EventResult(position, GetEntityId(),
          &AZ::TransformBus::Events::GetWorldTranslation);
   // the amount of change per tick
   const float change = (dt / m_period) * m_amplitude;
   // move up during the first half of the period
   if (m_currentTime < m_period / 2)
      {
          position.SetZ(position.GetZ() + change);
          AZ::TransformBus::Event(GetEntityId(),
              &AZ::TransformBus::Events::SetWorldTranslation,
              position);
      }
   // move down during the second half of the period
   else if (m_currentTime < m_period)
      {
          position.SetZ(position.GetZ() - change);
          AZ::TransformBus::Event(GetEntityId(),
              &AZ::TransformBus::Events::SetWorldTranslation,
              position);
      }
   else // reset the time to start the next cycle
      {
          m_currentTime = 0;
      }
 }
```

我们已经在前面的章节中看到了 SetWorldTranslation 和 GetWorldTranslation 的用法。这就是它们在实际用途中的用途。这是移动实体的常见模式。首先，您获取其当前位置。其次，将 position 设置为所需的值。

## 小结

{{<note>}}
您可以在 GitHub 上找到本章的代码：
https://github.com/AMZN-Olex/O3DEBookCode2111/tree/ch09_oscillator
{{</note>}}

本章展示了一个示例，如何连接到事件总线 AZ::TickBus，并将其与 TransformBus 结合使用以移动实体。

为了使用此组件，请将其附加到 Root Entity。

![](/images/learning-guide/tutorials/o3de-book/Part3/o3de_book_3_11.PNG)

附加到实体的振荡器组件

例 9.3.OscillatorComponent.h 完整列表

```c++
#pragma once
 #include <AzCore/Component/Component.h>
 #include <AzCore/Component/TickBus.h>
 namespace MyProject
 {
   // An example of singing up to an Ebus, TickBus in this case
   class OscillatorComponent
          : public AZ::Component
          , public AZ::TickBus::Handler // for ticking events
      {
       public:
       // be sure this guid is unique, avoid copy-paste errors!
              AZ_COMPONENT(OscillatorComponent, "{302AE5A0-F7C4-4319-8023-B1ADF53E1E72}");
       protected:
         // AZ::Component overrides
         void Activate() override;
         void Deactivate() override;
         // AZ::TickBus overrides
         void OnTick(float dt, AZ::ScriptTimePoint) override;
         // Provide runtime reflection, if any
         static void Reflect(AZ::ReflectContext* reflection);
         // what other components does this component require?
         static void GetRequiredServices(
                    AZ::ComponentDescriptor::DependencyArrayType& req);
       private:
         float m_period = 3.f;
         float m_currentTime = 0.f;
         float m_amplitude = 10.f;
    };
 }
```

例 9.4.OscillatorComponent.cpp完整列表
```c++
#include "OscillatorComponent.h"
#include <AzCore/Serialization/EditContext.h>
#include <AzCore/Component/TransformBus.h>
using namespace MyProject;
void OscillatorComponent::Activate()
{
  // We must connect, otherwise OnTick() will never be called.
  // Forgetting this call is the common error in O3DE!
  AZ::TickBus::Handler::BusConnect();
}
void OscillatorComponent::Deactivate()
{
  // good practice on cleanup to disconnect
  AZ::TickBus::Handler::BusDisconnect();
}
void OscillatorComponent::OnTick(float dt, AZ::ScriptTimePoint)
{
  m_currentTime += dt;
  // get current position
  AZ::Vector3 position;
  AZ::TransformBus::EventResult(position, GetEntityId(),
    &AZ::TransformBus::Events::GetWorldTranslation);
  // the amount of change per tick
  const float change = (dt / m_period) * m_amplitude;
  // move up during the first half of the period
  if (m_currentTime < m_period / 2)
  {
    position.SetZ(position.GetZ() + change);
    AZ::TransformBus::Event(GetEntityId(),
      &AZ::TransformBus::Events::SetWorldTranslation,
      position);
  }
  // move down during the second half of the period
  else if (m_currentTime < m_period)
  {
    position.SetZ(position.GetZ() - change);
    AZ::TransformBus::Event(GetEntityId(),
      &AZ::TransformBus::Events::SetWorldTranslation,
      position);
  }
  else // reset the time to start the next cycle
  {
    m_currentTime = 0;
  }
}
void OscillatorComponent::Reflect(AZ::ReflectContext* reflection)
{
  auto sc = azrtti_cast<AZ::SerializeContext*>(reflection);
  if (!sc) return;
  sc->Class<OscillatorComponent, Component>()
    ->Version(1);
  AZ::EditContext* ec = sc->GetEditContext();
  if (!ec) return;
  using namespace AZ::Edit::Attributes;
  // reflection of this component for O3DE Editor
  ec->Class<OscillatorComponent>("Oscillator Component", "[oscillates the entity]")
    ->ClassElement(AZ::Edit::ClassElements::EditorData, "")
    ->Attribute(AppearsInAddComponentMenu, AZ_CRC_CE("Game"))
    ->Attribute(Category, "My Project");
}
void OscillatorComponent::GetRequiredServices(AZ::ComponentDescriptor::DependencyArrayType& req)
{
  // OscillatorComponent requires TransformComponent
  req.push_back(AZ_CRC_CE("TransformService"));
}
```
