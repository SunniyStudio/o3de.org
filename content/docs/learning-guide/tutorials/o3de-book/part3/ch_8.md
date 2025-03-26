---
linkTitle: 第8章 AZ::EBus是什么
title: 第8章 AZ::EBus是什么
description: 第8章 AZ::EBus是什么
---
# 第8章 AZ::EBus是什么
## 介绍
解释复杂事物的一个好方法是将其与相似但略有不同的事物进行对比。

游戏引擎中的对象或组件相互通信的常用方式有哪些？大多数情况下，方法是通过直接调用。也就是说，你获得一个指针或对组件实例的引用，然后对其调用一个方法。有时它可能通过直接类型指针完成，有时它可能通过基类指针完成。

```c++
// pseudo-code ahead
{
  MyComponent* a;
  a->DoSomething();
  // OR
  MyComponent* a;
  MyBase* b = a;
  b->DoSomething();
}
```
在编程中，任何在小规模上看起来简单的东西在大规模上往往是完全破碎的和可怕的。

上述设计当然有效。然而，在大型系统中，它通常会导致所有内容交织在一起并变成意大利面条式代码。这是我的理论，关于在具有直接组件交互的大规模产品代码的上下文中导致这种情况的原因。解耦和抽象通常只考虑成员变量和基类纯接口。但是，解耦不仅仅是解耦组件的接口。

抽象出组件的调用与抽象出组件实现的细节一样重要。面向对象编程的基本原则是众所周知的。其中一项原则是抽象。一个抽象出类的细节，以便可以提供各种运行时行为，而无需将调用者绑定到特定接口。这样，A 可以以相同的方式调用 B 或 C，只要它们都继承自同一个基类。但是，这里还有另一个元素可以抽象出来。

考虑一下，无论何时在任何实例上调用方法，都必须获取指向该实例的指针或引用。这是调用方和被调用方之间的一种耦合形式。毕竟，无论你如何获得该指针或引用，都必须调用该对象。

现在想象一下，您抽象了如何调用对象的细节。现在，您将分离在对象上调用的方法和调用方法的方式。这意味着您根本不需要担心接收方。也许它在那里，也许它不存在。由于您不再保留指针，因此您不必将自己提交到您正在调用的特定实例。前一刻您可能正在与一个实例通信，而下一刻您可能正在与另一个实例通信。或者，您可能在不同时间对不同数量的对象调用方法。

这就是 EBus 背后的设计的真正力量。现在让我们看看这个理论在实践中意味着什么。

## 事件总线的基本概念
事件总线 （EBus） 是 O3DE 用于调度通知和接收请求的通用通信系统。事件总线是可配置的，并支持许多不同的使用案例。

以下是通过虚拟接口的常见直接指针方法：
```C++
// pseudo-code ahead
Caller A;
Callee B;
// A somehow gets a pointer or a reference to B
BaseCallee* bb = &B;
A::Do()
{
  bb->DoSomething();
}
```

现在考虑一下你需要做什么来替换 bb 所指向的东西。例如，假设您正在编写一个单元测试，并希望测试 A 对 B 的行为。如果 A 在内部保存了指针 bb（这很常见），则必须确保有办法修改该指针并确保更改它是安全的。

你以前有没有在公共类中看到过 DEBUG_SetVariable() 的模式？也许，它甚至在发布版本中被定义出来，使用预处理器技术与 #ifdef？它通常是为此目的而创建的。必须有人编写自定义代码来插入对象，使其与测试对象（有时称为模拟对象）通信。

```
// pseudo-code ahead
class A{
public:
  void DoSomething();
#ifndef _RELEASE
  void DEBUG_ChangePointerToB(BaseCallee*); // YUCK!
#endif
};
```

或者，这可能是通过在构造时传递对象来解决的。但是，这会在构造时耦合这两个对象，这有其自身的问题，因为您必须在构造时传递正确的对象。长话短说，虽然所有这些方法都在某种程度上有效，但它们都是由于没有抽象出对象之间的通信造成的。

现在，想象一个不耦合调用方法的伪代码：
```
// pseudo-code ahead
Caller A;
Callee B;
B.connect_on(42);
A::CallAnother()
{
  call_on(42, &DoSomething); // where DoSomething is a method
}
```

42 只是一个任意标识符。它的目的是弄清楚谁将获得 DoSomething()。
将这个逻辑更进一步，你可以想象 B 随时在 42 上连接和断开连接。
```
// pseudo-code ahead
B.connect_on(42);
// somebody calls
call_on(42, &DoSomething);
D.connect_on(42);
// now this method will be called on two objects
call_on(42, &DoSomething);
// or both disconnect
B.disconnect(42);
D.disconnect(42);
// Now nobody gets the call
call_on(42, &DoSomething);
```

以下是使用 EBus 设计的这种通信形式的图形形式。

图 8.1.通过事件总线后面的接口调用

![](/images/learning-guide/tutorials/o3de-book/Part3/o3de_book_3_8.PNG)

现在，您可以将 DoSomething 从 Caller 重新路由到另一个目的地，而无需更改 Caller。相反，它将取决于 MyComponent 或其他行为类似的对象，例如 MyMock Component。

{{<note>}}
顺便说一句，这使得对 O3DE 组件进行单元测试变得轻而易举。我们将在第 15 章 为组件编写单元测试中了解如何为组件编写单元测试。
{{</note>}}

## 事件总线示例：TransformBus
好吧，现在理论已经足够了。让我们看一下 EBus 的真实示例以及如何调用它。O3DE 中最常见的事件总线之一是 TransformBus，它由 TransformComponent 实现。每当您希望移动 AZ::Entity 时，都必须使用 TransformBus。其定义可在 C:\O3DE\21.11.2\Code\Framework\AzCore\AzCore\Component\TransformBus.h 中找到：

例 8.1.TransformBus 定义
```
/**
* 用于定位实体和父实体的请求的 EBus。
* 事件在 AZ::TransformInterface 类中定义。
  */
  typedef AZ::EBus<TransformInterface>    TransformBus;
```

这是事件总线的声明。它声明了一种新型的 AZ::EBus TransformBus，其接口由纯虚拟类 TransformInterface 提供。您可以在同一个文件中找到它：

例 8.2.TransformInterface 代码段

```c++
 //! Interface for AZ::TransformBus, which is an EBus that 
//! receives requests to translate (position), rotate, and 
//! scale an entity in 3D space.
 class TransformInterface
        : public ComponentBus
{
 public:
 ...
 //! Sets the entity's world space translation, which 
//! represents how to move the entity to a new position 
//! within the world.
 virtual void SetWorldTranslation(
            [[maybe_unused]] const AZ::Vector3& newPosition) {}
 //! Gets the entity's world space translation.
 //! @return A three-dimensional translation vector.
 virtual AZ::Vector3 GetWorldTranslation() { 
  return AZ::Vector3(FLT_MAX); 
}
```

可以在 C:\O3DE\21.11.2\Code\Framework\AzFrame 中找到 TransformComponent
work\AzFramework\Components\TransformComponent.h 中：

```c++
class TransformComponent
: public AZ::Component
  , public AZ::TransformBus::Handler
```

如您所见，它实现了 EBus 的 Handler 子类。换句话说，这声明 TransformComponent 将处理对 TransformBus 的调用。最后，TransformComponent 必须连接到 TransformBus。你可以看到，在相应的源文件中，C:\git\o3de\Code\Framework\AzFramework\AzFramework\Components\TransformComponent.cpp：

```c++
 void TransformComponent::Activate()
    {
        AZ::TransformBus::Handler::BusConnect(m_entity->GetId());
 ...
```

{{<note>}}
C:\git\o3de 是我克隆 O3DE 的 GitHub 存储库的位置：
https://github.com/o3de/o3de
{{</note>}}

我们稍后将回到 Activate()和 m_entity->GetId()的含义。现在，让我们看一下整个情况。

图 8.2.TransformBus 和 TransformComponent

![](/images/learning-guide/tutorials/o3de-book/Part3/o3de_book_3_9.PNG)

以下是通过 TransformBus 调用 SetWorldTranslation 的方法：

例 8.3.事件总线调用示例

```c++
AZ::TransformBus::Event(
  // where is it going?
  entity.GetId(),
  // what method will be invoked?
  &AZ::TransformBus::Events::SetWorldTranslation,
  // input parameter(s)?
  position);
```

这意味着调用方正在通过 TransformBus 呼叫标识符为 entity 的某人。GetId() 方法的 SetWorldTranslation 具有单个输入参数 position。

谁将接听此电话？使用相同实体 ID 连接到 TransformBus 的用户。

```c++
AZ::TransformBus::Handler::BusConnect(m_entity->GetId());
```

{{<note>}}
这不是调用 EBus 的唯一方法。有许多不同的方法可以满足您的需求。后面的章节将介绍常见情况。
{{</note>}}

## 什么是 AZ::EntityId？
在一个由实体和附加到实体的组件组成的世界中，您期望对世界中特定组件进行寻址的常见方法是什么？O3DE 中最常见的通信形式是：“嘿，你，附加到实体 Y 的 X 类型的组件，做这个！例如，如果要移动实体，则需要指示其附加的 Transform 组件移动。因此，我们必须知道如何唯一地标识实体。

{{<note>}}
在 Editor 中，可以设置 Entity names（实体名称），但是，这些名称不是唯一的，仅用于调试目的。
{{</note>}}

实体由 AZ::EntityId 唯一标识。其定义可以在 C:\O3DE\21.11.2\Code\Framework\AzCore\AzCore\Component\EntityId.h 中找到。
```c++
/**
* Entity ID type.
* Entity IDs are used to uniquely identify entities. Each
* component that is attached to an entity is tagged with
* the entity's ID, and component buses are typically
* addressed by entity ID.
*/
class EntityId
{
  ...
  /**
  * Entity ID.
  */
  u64 m_id;
```

在后台，EntityId 是一个 64 位无符号整数，它是 O3DE 中的 AZ::u64 类型。

使用 AZ：：EntityId 与另一个组件通信的方法有很多种：
1. 与同一实体上的组件通信。
2. 与父实体上的组件通信。
3. 与子实体上的组件通信。
4. 与不相关实体上的组件通信。

### 同一实体上的事件总线
这是组件最常见的通信方式之一。

通常，附加到实体的组件会分配其实体 ID
```c++
class Component
{
  ...
  /**
  * Returns the entity ID if the component is attached to
  * an entity. If the component is not attached to any
  * entity, this function asserts. As a safeguard, make
  * sure that GetEntity()!=nullptr.
  * @return The ID of the entity that contains the component.
  */
  EntityId GetEntityId() const;
```

因此，如果在同一实体中调用 SetWorldTranslation，我们可以重写前面的调用 SetWorldTranslation 示例：
```c++
AZ::TransformBus::Event(
  // to the same entity
  GetEntityId(),
  &AZ::TransformBus::Events::SetWorldTranslation,
  position);
```

###  父实体的事件总线
通常，人们会将实体附加到其他实体，以便组合复杂的对象，例如第 3 章 实体和组件简介。O3DE 中的父子关系以 TransformComponent 表示，可以通过其事件总线 TransformBus 进行访问。因此，如果要移动父实体：

```c++
AZ::EntityId parentId;
// Get parent entity id of the current entity
AZ::TransformBus::EventResult(parentId, GetEntityId(),
&AZ::TransformBus::Events::GetParentId);
```

键是 AZ::TransformBus 中的 GetParentId 方法：

```c++
class TransformInterface {
  //! Returns the entityId of the entity's parent.
  //! @return The entityId of the parent. The entityId is
  //! invalid if the entity does not have a parent.
  virtual EntityId GetParentId() { return EntityId(); 
}
```

之后，它将对 SetWorldTranslation 进行 EBus 调用，例如：
```c++
AZ::TransformBus::Event(
  // to the parent entity
  parentId,
  &AZ::TransformBus::Events::SetWorldTranslation,
  position);
```

### 指向子实体的 EBus
TransformInterface 还有一种方法可以获取其所有子实体：GetChildren。
```c++
class TransformInterface {
//! Returns the entityIds of the entity's immediate children.
 virtual AZStd::vector<AZ::EntityId> GetChildren()
```

以下是它的使用方法：
```c++
    AZStd::vector<AZ::EntityId> children;
    AZ::TransformBus::EventResult(children, GetEntityId(),
        &AZ::TransformBus::Events::GetChildren);
 // iterate over all or just call one of children
 for (AZ::EntityId id: children)
    {
        AZ::TransformBus::Event(
 // to a child entity
            id,
            &AZ::TransformBus::Events::SetWorldTranslation,
            position);
    }

```

### 事件总线事件与 EventResult
您可能已经注意到，在上面的示例中，我们使用了 AZ::TransformBus::Event 和 AZ：：TransformBus::EventResult。唯一的区别是 EventResult 返回一个结果。EventResult 中的第一个参数是返回值。一般来说，这两种调用都是：

```c++
// EBus without a return value
{BusType}::Event({BusId}, &{Method}, {any input parameters});
// EBus with a return value
{BusType}::EventResult({return_value},
{BusId}, &{Method}, {any input parameters});
```

{{<note>}}
上面示例中的 BusId 是 AZ::EntityId，但它也可以是任何其他类型。
{{</note>}}
