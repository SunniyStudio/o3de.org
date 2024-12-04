---
linktitle: 最佳实践
title: 脚本事件最佳实践
description: 了解在 Open 3D Engine （O3DE） 中使用脚本事件时的最佳实践。
weight: 400
---

以下是在 Open 3D Engine （O3DE） 中使用脚本事件的一些最佳实践。

## 确保在发送事件之前激活实体

在实体激活期间发送事件可能会产生意外结果。由于无法保证实体的激活顺序，因此在激活期间发送事件时，某些需要处理该事件的实体可能无法收到该事件。特别是，**在图形开始时** 和 **在实体激活时** 事件会受到激活顺序问题的影响。

要确保所有需要监听和处理给定脚本事件的实体都已准备好接收该事件，请在 [tick bus](/docs/user-guide/programming/components/tick)上将消息排队。在 Script Canvas 中实施此策略的一种方法是将 **On Graph Start** 节点连接到 **Tick Delay** 节点。延迟有助于确保在发送脚本事件消息时，可能连接到该脚本事件的所有实体都会收到该事件。

![Using the Tick Delay node in Script Canvas to ensure that entities are activated before events are sent.](/images/user-guide/scripting/script-events/best-practices-tick-delay.png)

## 注意脚本事件资源版本控制

由于脚本事件是用户创建的资源，因此当现有脚本或 Script Canvas 图表中引用的资源发生更改时，可能会出现问题。

{{< note >}}
Script Canvas 提供脚本事件版本验证。修改脚本事件资产时，Script Canvas 会更新引用它的脚本事件节点。如果打开具有已修改脚本事件的图形，则该图形将标记为已修改。要将脚本事件节点更新到最新版本，请保存图表。
{{< /note >}}

## 使用脚本事件而不是 Gameplay Notification Bus 系统

与 Gameplay 总线系统相比，脚本事件具有以下优势。脚本事件：

* 由数据驱动。
* 支持更多数据类型。
* 需要较少的维护。
* 可以在 Script Canvas 和 Lua 中使用。

由于这些原因，脚本事件取代了游戏通知总线系统和以下与`GameplayNotificationBus`相关的类：

* `GameplayNotificationBus`
* `GameplayNotificationId`
* `BehaviorGameplayNotificationBusHandler`
* `GameplayEventHandlerNode` (Legacy)

### 迁移到脚本事件

如果您的项目使用`GameplayNotificationBus`，您可以对其进行修改以使用脚本事件。

**从 GameplayNotificationBus 迁移到脚本事件**

1. 创建一个新的脚本事件，用于执行所需的事件消息收发。

1. 确定使用`GameplayNotificationBus`的 Script Canvas 图形或 Lua 脚本。

1. 将使用`GameplayNotificationBus`的节点或代码替换为脚本事件 **Send** 或脚本事件 **Receive**。
