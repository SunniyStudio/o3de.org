---
description: ' 了解 Open 3D Engine 中组件与 EBus 之间的关系。 '
title: 组件和EBus
weight: 700
---

EBus 并非组件所必需，也不以任何方式与组件直接绑定。但由于 EBus 构成了所有 O3DE 组件之间的通信骨干，因此可以带来很多好处。我们强烈建议您学习如何在游戏、系统和组件中使用它们。更多信息，请参阅[使用事件总线（EBus）系统](/docs/user-guide/programming/messaging/ebus/)。

大多数组件提供两种 EBus 来促进通信：请求总线和通知总线。这两种 EBus 都使用 `EBusAddressPolicy::ById` 地址策略和实体 ID 进行标识。

## 请求总线

组件的请求总线允许其他组件或外部系统向组件提出请求。通常，组件的运行时版本会实现请求总线。不过，在特殊情况下，编辑器组件也可以为总线提供服务。

下面将介绍请求总线的各个部分。

### Transform请求事件组 

下面的示例定义了一组由 `TransformComponent` 处理的事件。

```
class TransformComponentRequests
      : public AZ::ComponentBus // EBus traits for component buses: identification is based on an entity ID.
{
      public:

      // EBusTraits overrides - Only a single handler is allowed for a given entity ID.
      // Only one component on a entity can implement the events.
      static const EBusHandlerPolicy HandlerPolicy = EBusHandlerPolicy::Single;

      // Returns the local transform (parent transform excluded).
      virtual const Transform& GetLocalTM() = 0;

      // Sets the local transform and notifies all interested parties.
      virtual void SetLocalTM(const Transform& /*tm*/) {}

      // Returns the world transform (including parent transform).
      virtual const Transform& GetWorldTM() = 0;

      // Sets the world transform and notifies all interested parties.
      virtual void SetWorldTM(const Transform& /*tm*/) {}

      // Returns both local and world transforms.
      virtual void GetLocalAndWorld(Transform& /*localTM*/, Transform& /*worldTM*/) {}

...
 };
```

### 基类和Trait规范

大多数 `AZ::Component` 请求总线的基类是 `AZ::ComponentBus`。该类便于设置组件 EBus 的典型 EBus 特性。您也可以通过继承默认的`AZ::EbusTraits`来设置 EBus 特征。然后，您可以选择覆盖以下任何或所有特征。更多信息，请参阅 [EBus 配置选项](/docs/user-guide/programming/messaging/ebus-design/#ebus-in-depth-configuration)。
+ 地址策略
+ 总线 ID 类型
+ 连接策略
+ 处理程序策略
+ 锁定类型
+ 优先排序

以下示例展示了这两种方法。

```
// Example using AZ::ComponentBus.
class TransformComponentRequests
      : public AZ::ComponentBus
{...}
```

```
// Example using AZ::EBusTraits
class TransformComponentRequests
      : public AZ::EBusTraits
{
...
      // EBusTraits overrides.
      static const EBusAddressPolicy AddressPolicy = EBusAddressPolicy::ById; // OR YOUR CHOSEN POLICY
      static const AZ::EBusHandlerPolicy HandlerPolicy = AZ::EBusHandlerPolicy::Multiple; // OR YOUR CHOSEN POLICY
      using BusIdType = EntityId;
...
}
```

### EBus请求总线事件 

EBus 事件定义是总线规范的主要部分。该接口定义了组件的功能。在下面的示例中，`TransformComponent`允许检索和修改本地和世界变换。它还创建了用于设置父子关系的接口。

```
...

// Returns the local transform (parent transform excluded).
virtual const Transform& GetLocalTM() = 0;

// Sets the local transform and notifies all interested parties.
virtual void SetLocalTM(const Transform& /*tm*/) {}

// Returns the world transform (including parent transform).
virtual const Transform& GetWorldTM() = 0;

// Sets the world transform and notifies all interested parties.
virtual void SetWorldTM(const Transform& /*tm*/) {}

// Returns both local and world transforms.
virtual void GetLocalAndWorld(Transform& /*localTM*/, Transform& /*worldTM*/) {}

...
```

### EBus请求总线定义 

声明事件组后，必须定义 EBus。虽然您可以使用 `AZ::EBus<TransformComponentRequests>` 来定义 EBus，但我们建议您使用 `typedef` 来代替，如下例所示。这样可以提高总线调用站点的可读性。

```
typedef AZ::EBus<TransformComponentRequests> TransformComponentRequestBus;
```

另一个最佳实践是在 EBus 中使用描述性名称，避免重载函数。明确和描述性的函数名称可以防止将来在类继承（可能是许多继承）EBus接口时发生API名称冲突。避免重载函数可以改善在脚本环境中使用 EBus 的体验。在 Lua 和可视化脚本中，额外的表现力可以提高可读性和清晰度。

## 通知总线 

组件使用其通知总线向其他组件和引擎的其他部分通报相关变更。为此，它以 EBus 事件的形式向任何监控总线的类发送通知。为了监控总线，类需要实现通知总线处理程序接口\(对于 `TransformComponent`，这就是 `AZ::TransformNotificationBus::Handler`。\)

{{< note >}}
请求总线**向**组件发送信息；通知总线**从**组件发送信息。
{{< /note >}}

### Transform通知事件组 

下面的示例定义了一组由 `TransformComponent` 发送的通知事件。

```
class TransformNotifications
      : public AZ::ComponentBus
{
    public:
    ...
      // Called when the local transform of the entity has changed. Local transform update always implies world transform change too.
      virtual void OnTransformChanged(const Transform& /*local*/, const Transform& /*world*/) {}
    ...
};

typedef AZ::EBus<TransformNotifications>    TransformNotificationBus;
```

如果需要，通知总线还可以更改其 `EBusTrait` 规范。

## 作为 EBus 处理程序的组件

创建 EBus 事件组并定义 EBus 后，您的组件就可以通过派生 EBus 处理程序来实现 EBus 接口。下面的示例来自`TransformComponent`。

```
class TransformComponent
      : public AZ::Component
    , private AZ::TransformComponentRequestBus::Handler
{
      ...

      // TransformBus.

      /// Returns true if the tm was set to the local transform.
      const AZ::Transform& GetLocalTM() override { return m_localTM; }

      /// Sets the local transform and notifies all interested parties.
      void SetLocalTM(const AZ::Transform& tm) override;

      /// Returns true if the transform was set to the world transform.
      const AZ::Transform& GetWorldTM() override { return m_worldTM; }

      /// Sets the world transform and notifies all interested parties.
      void SetWorldTM(const AZ::Transform& tm) override;

      /// Returns both local and world transforms.
      void GetLocalAndWorld(AZ::Transform& localTM, AZ::Transform& worldTM) override { localTM = m_localTM; worldTM = m_worldTM; }

      ...
}
```

此时，您可以在 `TransformComponent` 中实现所定义的方法。当 `TransformComponent` 为其实体 ID 连接到 EBus 后，每当该总线或 ID 上有事件发生时，就会调用其事件处理程序。
