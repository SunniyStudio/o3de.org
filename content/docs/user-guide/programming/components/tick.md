---
description: ' 了解如何在 Open 3D 引擎中使用Tick总线。'
title: Tick总线和组件
weight: 775
---

Tick 总线是组件订阅每个模拟帧所发生事件的主要机制。与其将组件连接到 Tick 总线，不如努力使组件完全由事件驱动。如果您的组件需要基于 Tick 的功能，它可以实现 Tick 总线接口的 `OnTick` 方法，并在需要的时间段内连接到 Tick 总线。为避免基于轮询的更新结构扩展性差，组件应限制与 Tick 总线连接的时间。

传统上，组件可能会使用 `OnTick` 方法连续检查状态，但只在连接时间的一小部分主动进行处理。相反，组件应该只在状态发生变化时才连接到 Tick 总线，并在处理完成后断开连接。这种方法更接近于基于事件的编程，即只在短时间内进行轮询或根本不进行轮询。

举例来说，一个组件要在实体进入触发器后监控实体的状态。在实体进入触发器之前，该组件应避免订阅 Tick 总线。一旦实体离开触发器，该组件就应立即断开与 Tick 总线的连接。

在下面的示例中，`NavigationComponent`实现了 `OnTick` 方法。

```
class NavigationComponent
      : public AZ::Component
    , public NavigationComponentRequestBus::Handler
    , public AZ::TickBus::Handler
{
      ...

    // TickBus
    virtual void OnTick(float deltaTime, AZ::ScriptTimePoint time);

      ...
}
```

要连接或断开 Tick 总线，组件需要使用如下代码。

```
AZ::TickBus::Handler::BusConnect();
```

```
AZ::TickBus::Handler::BusDisconnect();
```

## 自定义Tick顺序

默认情况下，处理程序根据组件初始化的顺序接收事件。要控制组件接收 `OnTick`事件的顺序，可以覆盖 `GetTickOrder()`函数，返回一个自定义整数值。该整数值决定了相对于 Tick 总线上其他组件，您的组件被Tick的顺序。较低的值会在较高的值之前打勾。允许使用任何值。为方便起见`AZ::ComponentTickBus`枚举\(`TickBus.h`\)提供了一些预设值。这些值如下表所示。

**Tick顺序预设值**

| 名称 (C++) | 名称 (Lua/Script Canvas) | 值 | 说明 |
| --- | --- | --- | --- |
| TICK\_FIRST | TickOrder.First | 0 | Tick处理程序顺序中的第一个位置。 |
| TICK\_PLACEMENT | TickOrder.Placement | 50 | 建议的Tick处理程序位置，适用于需要在Tick顺序中提前Tick的组件。 |
| TICK\_INPUT | TickOrder.Input | 75 | 建议的输入组件Tick处理程序位置。|
| TICK\_GAME | TickOrder.Game | 80 | 游戏相关组件的建议Tick处理程序位置。 |
| TICK\_ANIMATION | TickOrder.Animation | 100 | 动画组件的建议Tick处理程序位置。 |
| TICK\_PHYSICS | TickOrder.Physics | 200 | 物理组件的建议Tick处理程序位置。 |
| TICK\_ATTACHMENT | TickOrder.Attachment | 500 | 附件组件的建议Tick处理位置。 |
| TICK\_PRE\_RENDER | TickOrder.PreRender | 750 | 建议的Tick处理程序位置，用于更新与呈现相关的数据。 |
  | TICK\_DEFAULT | TickOrder.Default | 1000 | 处理程序构建时的默认Tick处理程序位置。 |
| TICK\_UI | TickOrder.UI | 2000 | 用户界面组件的建议Tick处理程序位置。 |
| TICK\_LAST | TickOrder.Last | 100000 | Tick处理程序顺序中的最后一个位置。|

下面的代码示例展示了如何在 Lua 和 C++ 中覆盖 `GetTickOrder()` 函数。

```
-- Lua example
function MyLuaUIComponent:GetTickOrder()
    return TickOrder.UI
end
```

```
// C++ example
int MyCppUIComponent::GetTickOrder()
{
    return TICK_UI;
}
```

## 基于事件的编程和基于事件的轮询： 最佳实践

重要的是要知道何时使用 tick 总线，何时使用事件驱动编程模式。

### 基于事件的轮询

通常情况下，我们可以方便地在每一帧对某个组件进行Tick操作，并监控其他实体的状态。例如，`LookAt`相机组件通常会在每一帧打勾，获取目标实体的变换，并相应地更新自己的变换。

### 基于事件的编程

在 O3DE 中，更多的事件驱动方法是使用`TransformBus`，以纯事件驱动的方式监控目标实体的变换变化。如果目标实体没有移动，则不做任何工作，也不需要轮询。当目标实体移动时，`LookAt`组件会相应地调整自己实体的变换。

### 使用通知使组件易于使用

在创建组件时，尽量预测可能依赖于你的组件的组件的需求。使用通知总线为你的组件提供适当的通知。这种方法能让其他人以更快、更可扩展的方式编写使用你的组件服务的代码。

有关更多最佳实践，请参阅[组件和 EBus： 最佳实践](/docs/user-guide/programming/components/entity-system-pg-components-ebuses-best-practices/)。
