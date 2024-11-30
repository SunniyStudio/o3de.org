---
linktitle: 最佳实践
title: Script Canvas 最佳实践
description: 了解在 Open 3D Engine （O3DE） 中使用 Script Canvas 时的最佳实践。
weight: 500
---

Script Canvas 的最佳实践包括使用事件驱动型方法和使用自定义节点来简化图形。

## 使用事件驱动的方法

默认情况下，Script Canvas 节点是无状态的。但是，通过连接到 [tick 总线](/docs/user-guide/programming/components/tick)，可以将它们配置为具有状态。工程师必须管理具有状态的节点的生命周期和性能。

在核心 Script Canvas 库中，state 主要用于驱动图形中的逻辑（与 **Delay** 节点一样）。但是，建议使用事件驱动型范例，因为它有助于降低创作和运行图形的复杂性。

我们建议您通过行为上下文将功能反映到 Script Canvas 中。即使对于特定于 Script Canvas 的功能也是如此。使用行为上下文鼓励通过事件总线实现事件驱动的范例。这种方法产生了模块化、解耦的行为，可以降低图形复杂性并利用运行时优化。

## 使用自定义节点简化图表

识别经常使用但复杂的用户模式，并通过自定义节点和改进的行为上下文方法帮助简化它们。将自定义节点与事件总线结合使用可以降低图形的整体复杂性，并使图形创作更加直观。有关创建自定义节点的信息，请参阅 [在 Script Canvas 中创建自定义节点](/docs/user-guide/scripting/script-canvas/programmer-guide/custom-nodes).

## 小心实体激活顺序

在实体激活期间发送事件可能会产生意外结果。由于无法保证实体的激活顺序，因此在激活期间发送事件时，某些需要处理该事件的实体可能无法收到该事件。特别是，**On Graph Start** 和 **On Entity Activated**节点会受到激活顺序问题的影响。从它们发送事件时要小心。

为了确保需要侦听和处理给定脚本事件的所有实体都已准备好接收该事件，最好在 tick 总线上对消息进行排队。要实施此策略，请使用连接到 **On Tick** 消息的 **Once** 节点，如下图所示。这种做法保证了在发送消息时，可能连接到该脚本事件的所有实体都会收到它。

![The Once node connected to the On Tick message](/images/user-guide/scripting/script-canvas/best-practices-activation-order.png)
