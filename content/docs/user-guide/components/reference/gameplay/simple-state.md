---
linkTitle: Simple State
title: Simple State 组件
description: 使用 Open 3D Engine (O3DE) 中的 Simple State 组件提供一个简单的状态机。
---

**Simple State**组件提供了一个简单的状态机。每个状态由一个名称和一个包含零个或多个实体的数组表示。实体在进入状态时被激活，在退出状态时被停用。简单状态组件可能处于`NullState`，即没有激活状态。

## 提供方

[O3DE Core (LmbrCentral) Gem](/docs/user-guide/gems/reference/o3de-core)

## Simple State 属性

![Simple State component properties](/images/user-guide/components/reference/gameplay/simple-state-component.png)

| 属性 | 名称 | 值 | 默认值 |
|-|-|-|-|
| **Initial state** | Simple State组件首次激活时的活动状态。 | `<None>` or state **Name** | `<None>` |
| **Reset on activate** | 如果启用，Simple State组件会在激活时返回**Initial state**，而不是停用前的状态。 | Boolean | `Enabled` |
| **States** | 状态数组。 |  |  |
| **States** - **Name** | 定义状态的名称。 | String | `New State` |
| **States** - **Entities** | 与状态相关联的实体数组。 | EntityId | None |

## SimpleStateComponentRequestBus

| 请求名称 | 说明 | 参数 | 返回值 | 可脚本化 |
|-|-|-|-|-|
| `GetCurrentState` | 返回当前状态的**Name**。 | None | State: String | Yes |
| `GetNumStates` | 返回状态总数。 | None | Count: Integer | Yes |
| `SetState` | 根据**Name**将组件设置为特定状态。 | State: String | None | Yes |
| `SetStateByIndex` | 根据组件在 **States** 数组中的索引，将组件设置为特定状态。 | State Index: Integer | None | Yes |
| `SetToFirstState` | 将组件设置为 State[0]，即 **States** 数组中的第一个状态。 | None | None | Yes |
| `SetToLastState` | 将组件设置为 **States** 数组中的最后一个状态。 | None | None | Yes |
| `SetToNextState` | 将组件设置为 **States** 数组中的下一个状态。 | None | None | Yes |
| `SetToPreviousState` | 将组件设置为 **States** 数组中的前一个状态。 | None | None | Yes |


## SimpleStateComponentNotificationBus

| 通知名称 | 说明 | 参数 | 返回值 | 可脚本化 |
|-|-|-|-|-|
| `OnStateChanged` | 通知侦听器状态已更改。 | None | Old State: String, New State: String | Yes |

更多信息，请参阅 [使用事件总线 (EBus) 系统](/docs/user-guide/programming/messaging/ebus/)。
