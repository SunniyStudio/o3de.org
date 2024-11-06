---
linktitle: Simple Network Player Spawner
title: Simple Network Player Spawner 组件
description: Simple Network Player Spawner组件实现了在Open 3D Engine (O3DE)的网络多人会话中处理玩家加入和离开事件的基本设置。
---

在**Open 3D Engine (O3DE)**联网多人游戏项目中，**Simple Network Player Spawner**级组件实现了处理`OnPlayerJoin` 和 `OnPlayerLeave`事件的基本设置，**Multiplayer Gem**的 `IMultiplayerSpawner`接口提供了这些设置。在这个基本设置中，当有玩家加入时，为该玩家创建一个网络实体，当有玩家离开时，移除该实体。

目前，这个组件可以很好地用于原型开发。如果想开发更具体的行为，可以在此组件的基础上进行构建，或为`OnPlayerJoin` 和 `OnPlayerLeave`事件实现自己的处理程序。

有关网络玩家生成的更多信息，请参阅 [生成玩家](/docs/user-guide/networking/multiplayer/spawning.md)。


## 提供者

[**Multiplayer Gem**](/docs/user-guide/gems/reference/multiplayer/multiplayer-gem/)


## 属性

![Simple Network Player Spawner component](/images/user-guide/components/reference/multiplayer/simple-player-spawner-component.png)

| 属性 | 说明 | 值 | 默认值 |
| - | - | - | - |
| **Player Spawnable Asset**  | 为每个加入多人游戏会话的玩家生成的网络资产。 | Network Asset  |   |
|  **Spawn Points** | 一个或多个出生点的列表，当有玩家加入时，**Player Spawnable Asset**会在这些出生点生成。资产按列表顺序在产卵点位置生成。如果玩家数量多于出生点数量，则出生点位置会循环回到列表顶端。  | EntityID |   |
