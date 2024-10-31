---
linkTitle: Fly Camera Input
title: Fly Camera Input 组件
description: 使用 Fly Camera Input组件可为您的Open 3D Engine  (O3DE) 关卡快速添加一个可控摄像机。
---

使用**Fly Camera Input**组件可为您的关卡快速添加一个可控摄像机。Fly Camera Input组件可接收来自鼠标和键盘、游戏手柄或触摸设备的输入。 您可以控制与Fly Camera Input组件连接到同一实体上的相机组件的向前、向后和横向移动以及外观方向。 鼠标 X 轴和 Y 轴移动可控制外观方向。键盘上的`W`, `A`, `S`, 和 `D`键可分别控制向前、向左、向后和向右运动。

## 提供方

[Atom Bridge Gem](/docs/user-guide/gems/reference/rendering/atom/atom-o3de-integration/)

## 依赖 ##

[Camera component](/docs/user-guide/components/reference/camera/camera)

## Fly Camera Input 属性

![Fly Camera Input component properties](/images/user-guide/components/reference/gameplay/fly-camera-input-component.png)

| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **Move Speed** | 设置摄像机的移动速度。 | 1.0 - 100.0 | `20.0` |
| **Rotation Speed** | 设置摄像机的旋转速度。 | 1.0 - 100.0 | `5.0` |
| **Mouse Sensitivity** | 设置鼠标的输入灵敏度。 | 0.0 - 1.0 | `0.025` |
| **Invert Rotation Input X** | 反转 X 轴旋转输入值（偏航、左右观察方向）。| Boolean | `Disabled` |
| **Invert Rotation Input Y** | 更改 Y 轴旋转输入值（俯仰、上下方向）。 | Boolean | `Disabled` |
| **Is Initially Enabled** | 如果设置为启用，则当父实体被激活时，飞行摄像机将处于活动状态并可被控制。 | Boolean | `Enabled` |

## FlyCameraInputBus

| 请求名称 | 说明 | 参数 | 返回值 | 可脚本化 |
|-|-|-|-|-|
| `GetIsEnabled` | 如果飞行摄像头处于活动状态，则返回 `True`。 | None | Boolean | Yes |
| `SetIsEnabled` | 如果为 `True`，则将飞行摄像头设置为激活状态。 如果为`False`，则停用飞行摄像头。 | Boolean | None | Yes |

更多信息，请参阅 [使用事件总线 (EBus) 系统](/docs/user-guide/programming/messaging/ebus/)。
