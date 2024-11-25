---
linkTitle: Character Movement APIs
title: Kythera AI Character Movement APIs
description: 在 Open 3D Engine （O3DE） 中使用 Kythera AI Gem 进行角色移动的 API 的详细信息。
weight: 400
toc: true
---

在二维导航网格上实现角色移动有两个主要事件总线：`Kythera::MovementRequestBus` 和 `Kythera::MovementNotificationBus`.

## MovementRequestBus

`MovementRequestBus` 由 Kythera AI 用于向游戏发出移动实体的请求。为了使实体具有由 Kythera AI 驱动的移动，它需要具有实现此事件总线的组件。

Kythera AI Gem 包含此 EBus 的非常基本的实现，其形式为 **Simple Movement Controller** 组件。

{{< note >}}
Simple Movement Controller组件实现尚不支持动画。
{{< /note >}}

`MovementRequestBus` 使用结构 `MovementRequest` 封装 Kythera AI 请求的移动，由三种方法组成：`SetMovementRequest`, `ClearMovementRequest` 和`IsAnimationSupported`。

* `SetMovementRequest` 是 Kythera AI 调用以请求从实体移动的方法。实现此请求的组件应接受此请求，保留它，然后尝试从组件的下一个时钟周期开始实施请求的移动。

* `ClearMovementRequest` 是 Kythera AI 在当前没有实体要执行的移动时调用的方法。实现此请求的组件应使角色在其当前位置保持大致静止，直到收到新的移动请求。

* `IsAnimationSupported`是 Kythera AI 调用以查询角色当前是否支持命名动画的方法。例如，在寻路时，这可用于确定实体是否能够使用 meshlink。如果网格链接指定角色需要名为 `JetPack` 的动画才能使用它，Kythera AI 将使用 `animationName JetPack` 调用此方法。如果返回 false，则 Kythera AI 不会尝试使用此 meshlink 生成导航路径。如果返回 true，则 Kythera AI 将 meshlink 用作导航路径的一部分。

## MovementRequest

`MovementRequest` 有三种可能的模式： `GoTo`, `ExactGoTo`, 和 `Animation`。`MovementRequest`的当前模式由 '`mode`' 参数指示。

### GoTo

`GoTo`请求表示标准移动命令。当实体遵循标准导航路径时，通常使用此方法，没有特殊要求。对于 `GoTo`请求，角色应以大约请求的速度向世界坐标`goal`移动，注视`lookTarget`，并将实体的主体朝向`bodyTarget`。`allowStrafing`参数指示角色是否应该能够扫射。当角色距离其目标足够距离（或超过目标）时，Kythera AI 会检测到移动请求已完成并更新移动请求以指向下一个目标位置。

### ExactGoTo

`ExactGoTo`当实体在完成移动时必须精确定位和定向时，将使用模式。例如，当即将触发特殊动画或实体即将遍历导航网格时，可以使用此方法，并且角色的确切位置对于动画看起来正确至关重要。

`ExactGoTo` 功能类似于`GoTo`模式，但有一个重要区别;Kythera 不会自动检测到移动请求已完成。运行时代码必须确定角色的位置和方向是否正确，然后使用`MovementNotificationBus`上的`ExactGotoEnded`方法通知 Kythera 移动已结束。然后，Kythera 使用下一个目标更新移动请求。

方向和定位所需的精度可能因不同的动画系统而异，并且可以指定。

### Animation

`Animation`模式触发特定动画以在两个位置之间移动角色。这通常在角色遍历导航网格时触发。例如，用于在楼层之间移动的“爬梯”动画，或用于越过低障碍物的“拱顶墙”动画。

`animation`结构体中的参数定义了要使用的动画的`name`、动画的 '`start` 位置以及动画所需的`end`位置。动画系统的选择留给集成商。动画的名称取自提供的输入，例如，与导航网格关联的动画的名称。动画完成后，游戏代码必须调用`MovementNotificationBus`上的 `AnimationEnded` 方法，以通知 Kythera 移动已结束。然后，Kythera 使用下一个目标更新移动请求。

## MovementNotificationBus

游戏代码调用`MovementNotificationBus`，以便在某些类型的运动完成时通知 Kythera AI。它有两种方法：`AnimationEnded`和`ExactGotoEnded`。`AnimationEnded` 通知 Kythera AI “Animation”移动请求已完成。`ExactGotoEnded`通知 Kythera AI `ExactGoTo`请求已完成。
