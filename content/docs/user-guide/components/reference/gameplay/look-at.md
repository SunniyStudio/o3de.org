---
linkTitle: Look-At
title: Look-At 组件
description: 在Open 3D Engine (O3DE)关卡中，使用 Look-At 组件可强制一个实体面向另一个实体。
---

使用**Look-At**组件可在Open 3D Engine (O3DE)关卡中强制一个实体面向另一个实体或位置。

## 提供方

[O3DE Core (LmbrCentral) Gem](/docs/user-guide/gems/reference/o3de-core)

## Look-At 属性

![Look-At component properties](/images/user-guide/components/reference/gameplay/look-at-component.png)

| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **Target** | 设置目标实体后，带有Look-At 组件的实体的**Forward Axis**将始终指向**Target**实体。 | EntityId | None |
| **Forward Axis** | 设置始终指向**Target**实体的轴。 | `Y+`, `Y-`, `X+`, `X-`, `Z+`, `Z-` | None |

## LookAt

| 请求名称 | 说明 | 参数 | 返回值 | 可脚本化 |
|-|-|-|-|-|
| `SetAxis` | 设置Look-At组件的**Forward Axis**。 | Axis: 0 - 5; 0 = `X+`, 1 = `X-`, 2 = `Y+`, 3 = `Y-`, 4 = `Z+`, 5 = `Z-`| None | Yes |
| `SetTarget` | 设置 Look-At 组件的 **Target** 实体。 | Target Entity: EntityId | None | Yes |
| `SetTargetPosition` | 设置 Look-At 组件始终指向特定位置。 | Target Position: Vector3 | None | Yes |

## LookAtNotification

| 通知名称 | 说明 | 参数 | 返回值 | 可脚本化 |
|-|-|-|-|-|
| `OnTargetChanged` | 当Look-At组件的 **Target** 实体发生变化时通知侦听器。 | None | Target Entity: EntityId | Yes |

更多信息，请参阅 [使用事件总线 (EBus) 系统](/docs/user-guide/programming/messaging/ebus/)。
