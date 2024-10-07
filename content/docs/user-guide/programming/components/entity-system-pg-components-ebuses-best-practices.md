---
linkTitle: '组件和EBus: 最佳实践'
title: '组件和EBus: 最佳实践'
description: 了解 Open 3D Engine 中组件和 EBus 的最佳实践。
toc: true
weight: 825
---

请遵循以下创建和使用组件和 EBus 的最佳实践。

## EBus 名称 

以下的 EBus 命名约定消除了歧义并提供了一致性。

+ 使用名称格式 `MyComponentRequestBus` 来表示其他人用来在 `MyComponent` 上调用函数的总线，如下面的示例。

  ```
  class CheeseburgerComponentRequests : public AZ::ComponentBus
  {
        bool ICanHasCheeseburger() const = 0;
  };
  using CheeseburgerComponentRequestBus = AZ::EBus<CheeseburgerComponentRequests>;
  ```
+ 对于从 `MyComponent` 广播的事件，请使用名称格式 `MyComponentNotificationBus`，如下例所示。

  ```
  class CheeseburgerComponentNotifications : public AZ::ComponentBus
  {
        void OnCheeseburgerEaten(AZ::u8 yelpRating) {};
  };
  using CheeseburgerComponentNotificationBus = AZ::EBus<CheeseburgerComponentNotifications>;
  ```

## 提供方法的默认实现 

通知总线通常会提供接口内方法的默认实现。许多其他组件可以监控您组件的事件，但并非所有组件都对您组件发送的每个事件感兴趣。如果您为所有方法提供默认实现，那么订阅您的事件的其他组件就只能实现与它们相关的事件。

## EBus事件命名

好的 EBus 事件名称是冗长的。类可以监控多个总线，因此描述性的事件名称可以清楚地说明函数对应的总线。这种做法还能防止不同总线的事件接口之间可能出现的名称冲突。

下面的示例就是一个清晰命名的 `PhysicsComponentNotificationBus` 事件。

```
virtual void OnPhysicsEnabled() = 0;
```

下面的示例是一个名称含糊的`PhysicsComponentNotificationBus`事件。

```
virtual void OnEnabled() = 0;
```

## 避免为序列化数据使用类型定义 

来自 O3DE 的一个具有启发性的示例说明了在序列化数据中使用类而不是类型定义的重要性。以前，`EntityId` 使用类型定义 `uint32_t`。当决定将其改为 64 位时，必须为每个包含 `EntityId` 的类编写升级函数。如果 `EntityId` 是一个类，只需为该类编写一个升级函数，就无需再做其他工作了。显然，这一原则不适用于原始类型，如 `bool`、`float`、`int` 和 `string`。但是，如果您有一个特定的序列化类型，并且将来可能会改变，那么就将其作为一个反射类来实现。这就提供了一个单一的上下文，您可以轻松地对类或类型进行转换。

## EBus 结果 

在调用覆盖变量的 EBus 事件之前，一定要先初始化变量。即使你确定某个特定的类或组件正在监听总线，也值得处理特殊情况。这一点在分布式环境中尤为重要，因为在这种环境中，实体会作为兴趣区或其他动态模式的一部分出现和消失。

下面的示例在调用产生结果的 EBus 事件前初始化了一个结果变量。

```
AZ::Transform targetEntityTransform = AZ::Transform::Identity(); // initialize result variable...
EBUS_EVENT_ID_RESULT(targetEntityTransform, targetEntityId, AZ::TransformBus, GetWorldTM); // ...in case of no response
```

## EBus 时间 

以下是一些关于 EBus 动作时间安排的最佳做法。

+ 在[Activate()](/docs/api/frameworks/azcore/class_a_z_1_1_component.html#a3e3e21c0cb8c2a8cb6dc42c83c94a8fe)函数中，请确保连接到总线是最后一步。
+ 在 [Deactivate()](/docs/api/frameworks/azcore/class_a_z_1_1_component.html#a7b78b890df9b149722d8703da05c79ac) 函数中，请确保断开与总线的连接是第一步。
+ 在多线程环境中，从连接总线到断开连接，都有可能接收到总线事件。因此，请确保以下几点：
  + 在开始对事件做出反应之前，组件已完全激活。
  + 组件在开始停用前停止接收事件。

这种做法可以防止组件在开始对事件做出反应时处于半激活状态，或者在接收事件时处于半关闭状态。

* 在通知式总线上发送事件时，函数的最后一步应确保数据已完全填充。

下面是一个需要避免的例子。

```
EBUS_EVENT_ID(GetEntityId(), OnTransformChanged, newTransform);
m_transform = newTransform;
```

如果某个组件正在监控 `OnTransformChanged` 事件，并在响应该事件时设置了变换，该组件的操作将被`m_transform = newTransform;`赋值撤销。

## 使函数公开或受保护 

在决定将函数设为公共或私有时，请考虑以下几点。
* 如果总线函数构成了类的公共接口，则将其设置为`public`。虽然不鼓励这样做，但 O3DE 并不阻止用户获取组件的直接指针和直接调用函数。为了避免这种情况，请确保您的有用函数是公共的。例如，`MyComponent`可能应该公开实现 `MyComponentRequestBus` 的函数。
* 如果您的总线函数包含了您的类的私有工作机制，请将它们设置为`protected`。例如，您的组件对 `TransformNotificationBus::OnTransformChanged`事件的反射很可能是一个私有的实现细节。
