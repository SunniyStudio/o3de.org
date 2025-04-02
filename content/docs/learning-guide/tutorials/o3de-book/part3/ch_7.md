---
linkTitle: 第7章 AZ::Event是什么
title: 第7章 AZ::Event是什么
description: 第7章 AZ::Event是什么
---
# 第7章 AZ::Event是什么

## 介绍

{{<note>}}
AZ::Event 的官方参考可以在这里找到：
https://www.o3de.org/docs/user-guide/programming/az-event/
{{</note>}}

AZ::Event 是在组件之间实现通知系统的绝佳工具，其中一个组件充当事件的发布者，而其他组件充当订阅者。同一发布者上可以有许多订阅者。AZ::Event 不适合用作 getter 接口，但非常适合通知调用，例如通知任何侦听组件世界转换已发生更改的 Transform 组件。

以下是用于创建 AZ::Event 的 C++ 构建基块。

图 7.1.设置 AZ::Event

![](/images/learning-guide/tutorials/o3de-book/Part3/o3de_book_3_7.PNG)

1. 使用AZ::Event<>定义事件
```c++
   AZ::Event<int> myEvent;
```
2. 使用带有回调的 AZ::Event<>::Handler 定义处理程序。
```c++
   AZ::Event<int>::Handler myHandler([](int value)
   {
   /*process*/
   });
```
3. 将处理程序连接到事件。
```c++
   myHandler.Connect(myEvent);
```
4. 从事件发送通知。
```c++
   myEvent.Signal(42);
```

{{<tip>}}
您可以在 AZ::Event 的模板参数中定义任何参数或多个参数<T>。处理程序必须与这些类型匹配。Signal 也必须使用相同类型的参数。
{{</tip>}}

例如，每当 TransformComponent 的实体移动或更改其旋转或缩放时，都会发出通知。它通过在 C：\git\o3de\Code\Framework\AzCore\AzCore\Component\TransformBus.h 中定义以下事件来实现此目的：
```c++
using TransformChangedEvent = AZ::Event<const Transform&, const Transform&>;
```

必须有一种方法可以将处理程序连接到此事件，这是通过调用 TransformComponent::BindTransformChangedEventHandler 来完成的：
```c++
void TransformComponent::BindTransformChangedEventHandler(AZ::TransformChangedEvent::Handler& handler)
{
  handler.Connect(m_transformChangedEvent);
}
```

如果你查看 TransformComponent.cpp 的代码，你会发现它在适当的时间向事件发出信号：
```c++
m_transformChangedEvent.Signal(m_localTM, m_worldTM);
```

## 例
了解了介绍性细节后，我们可以构建一个新组件，每当实体移动时，该组件都会打印调试消息。这里有四个步骤。
1. 声明正确的处理程序类型，并选择性地声明通知的回调方法。
```c++
   AZ::TransformChangedEvent::Handler m_movementHandler;
   void OnWorldTransformChanged(const AZ::Transform& world);
```

2. 在组件的构造函数中，定义调用回调方法 OnWorldTransformChanged 的 lambda。
```c++
MyEventComponent::MyEventComponent()
: m_movementHandler(
[this](
  const AZ::Transform& /*local*/,
  const AZ::Transform& world)
   {
      OnWorldTransformChanged(world);
   })
{
}
```

图 7.2. 使用 AZ::Event的TransformComponent通知

![](/images/learning-guide/tutorials/o3de-book/Part3/o3de_book_3_8.PNG)

3. 实现 callback 方法以打印消息。
```c++
   void MyEventComponent::OnWorldTransformChanged(
   const AZ::Transform& world)
   {
   AZ_Printf("MyEvent", "now at %f %f %f",
   world.GetTranslation().GetX(),
   world.GetTranslation().GetY(),
   world.GetTranslation().GetZ());
   }
```
{{<note>}}
   也可以只在 lambda 中完成所有工作。
{{</note>}}

4. 最后，处理程序需要通过 BindTransformChangedEventHandler 连接到事件。
```c++
void MyEventComponent::Activate()
{
  AZ::Entity* e = GetEntity();
  TransformComponent* tc = e->FindComponent<TransformComponent>();
  // track the movement of the entity
  tc->BindTransformChangedEventHandler(m_movementHandler);
}
```

{{<tip>}}
不同的组件可能会提供不同的方法来将处理程序连接到其事件，但方法名称通常具有相似的名称架构：BindSomethingEventHandler。
{{</tip>}}

## 小结
{{<note>}}
您可以在 GitHub 上找到本章的代码和项目更改：
https://github.com/AMZN-Olex/O3DEBookCode2111/tree/ch06_az_interface
{{</note>}}

以下是新组件 MyEventComponent 的完整源代码。

例 7.1. MyEventComponent.h
```c++
 #pragma once
 #include <AzCore/Component/Component.h>
 #include <AzCore/Component/TransformBus.h>
 namespace MyProject
 {
   // An example of listening to movement events
   // of TransformComponent using an AZ::Event
   class MyEventComponent : public AZ::Component
   {
   public:
          AZ_COMPONENT(MyEventComponent, "{934BF061-204B-4695-944A-23A1BA7433CB}");
          static void GetRequiredServices(AZ::ComponentDescriptor::DependencyArrayType& required)
          {
              required.push_back(AZ_CRC_CE("TransformService"));
          }
          MyEventComponent();
         // AZ::Component overrides
         void Activate() override;
         void Deactivate() override {}
         // Provide runtime reflection, if any
         static void Reflect(AZ::ReflectContext* rc);
   private:
         AZ::TransformChangedEvent::Handler m_movementHandler;
         void OnWorldTransformChanged(const AZ::Transform& world);
   };
 }
```

例 7.2. MyEventComponent.cpp
```c++
#include "MyEventComponent.h"
#include <AzCore/Component/Entity.h>
#include <AzCore/Serialization/EditContext.h>
#include <AzFramework/Components/TransformComponent.h>
using namespace MyProject;
MyEventComponent::MyEventComponent()
: m_movementHandler(
  [this](
  const AZ::Transform& /*local*/,
  const AZ::Transform& world)
  {
  })
{
}
OnWorldTransformChanged(world);
void MyEventComponent::OnWorldTransformChanged(const AZ::Transform& world)
{
  AZ_Printf("MyEvent", "now at %f %f %f",
    world.GetTranslation().GetX(),
    world.GetTranslation().GetY(),
    world.GetTranslation().GetZ());
}
void MyEventComponent::Activate()
{
  AZ::Entity* e = GetEntity();
  using namespace AzFramework;
  TransformComponent* tc = e->FindComponent<TransformComponent>();
  // track the movement of the entity
  tc->BindTransformChangedEventHandler(m_movementHandler);
}
void MyEventComponent::Reflect(AZ::ReflectContext* rc)
{
  auto sc = azrtti_cast<AZ::SerializeContext*>(rc);
  if (!sc) return;
  sc->Class<MyEventComponent, Component>()
    ->Version(1);
  AZ::EditContext* ec = sc->GetEditContext();
  if (!ec) return;
  using namespace AZ::Edit::Attributes;
  ec->Class<MyEventComponent>("My Event Component Example", "[Communicates using AZ::Event]")
    ->ClassElement(AZ::Edit::ClassElements::EditorData, "")
    ->Attribute(AppearsInAddComponentMenu, AZ_CRC("Game"))
    ->Attribute(Category, "My Project");
}
```
