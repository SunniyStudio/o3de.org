---
linktitle: 生成玩家
title: 生成玩家实体
description: 在 Open 3D Engine （O3DE） 中生成和注册网络玩家实体的参考。
weight: 700
---

在大多数联网多人游戏中，当玩家加入会话时，主机必须为玩家生成一个 [*autonomous*](overview#multiplayer-entity-roles)  联网实体。同样，当玩家离开时，主机必须删除实体并进行清理。在 **Open 3D Engine （O3DE）** 中，您可以以这种方式设置生成逻辑，或者使用 **多人游戏 Gem** 的`IMultiplayerSpawner`接口提供的`OnPlayerJoin` 和 `OnPlayerLeave`事件以其他方式处理 join 和 leave 事件。或者，为了更快地开始，您可以绕过这些事件的编程，并使用 [**Simple Network Player Spawner**](/docs/user-guide/components/reference/multiplayer/simple-player-spawner)组件，该组件为这个常见用例设置了`OnPlayerJoin` 金额 `OnPlayerLeave`。

## `IMultiplayerSpawner` 接口

*IMultiplayerSpawner* 是一个[`AZ::Interface<T>`](/docs/user-guide/programming/messaging/az-interface)，它提供了一种机制，当玩家加入会话时，告诉多人游戏 Gem 要生成什么以及在哪里生成它。`IMultiplayerSpawner` 还提供了一个钩子，用于在玩家离开时进行清理。所有多人游戏都应提供一个实现来处理为玩家加入和玩家离开事件交付的事件。

### 玩家加入事件

玩家在以下情况下被定义为已*加入*会话：
  * 客户端应用程序开始托管会话，在这种情况下，主机可能需要生成新的玩家实体。
  * 客户端连接到正在进行的托管会话。

`IMultiplayerSpawner`的`OnPlayerJoin`方法提供了一个钩子，用于服务器何时可能希望代表用户生成自主玩家预制件。它采用用户提供的标识符和自定义数据，并且需要实现来确定要生成的预制件及其世界位置。例如，随连接提供的数据可用于选择特定角色或团队。

`OnPlayerJoin`应返回`NetworkEntityHandle`，从而使调用方负责为用户实例化联网预制件。然后，多人游戏 Gem 获取返回的`NetworkEntityHandle`，并将其标记为自主，并将其与玩家的连接相关联。

### 玩家离开事件

玩家在客户端与服务器断开连接时定义为 * 离开* 会话。与玩家连接不同，当客户端停止托管自己的会话时，没有特殊情况 - 其中一个步骤应该始终在关闭服务器之前断开 *所有* 客户端的连接。

`IMultiplayerSpawner` 的`OnPlayerLeave`方法提供了一个钩子，这样当客户端断开连接时，服务器可以清理为客户端生成的任何实体。`OnPlayerLeave`将实体句柄用于`OnPlayerJoin` 生成的预制件，以便可以删除玩家实体。它还需要连接的复制集，这也允许服务器删除关联的实体（例如，由离开的玩家部署的对象）。

`OnPlayerLeave` 还采用 disconnect reason，它允许响应不同类型的 disconnect。例如，如果玩家可以尝试重新连接到会话，则如果玩家超时，则清理对象可能是不可取的。


## MultiplayerSample Project 中的示例

生成器实现的一个实际示例是 [MultiplayerSample Project](https://github.com/o3de/o3de-multiplayersample/) 中的 ['MultiplayerSampleSystemComponent'](https://github.com/o3de/o3de-multiplayersample/blob/2c84827ffb20082b8c16fc0edc65cd49226f3cd2/Gem/Code/Source/MultiplayerSampleSystemComponent.cpp)。MultiplayerSample Project 实现了一个“round robin（循环）”样式的生成系统，该系统使用`NetworkPlayerSpawnerComponents`收集实体。然后，`MultiplayerSampleSystemComponent`在`OnPlayerJoin`事件期间查询该系统。`OnPlayerLeave` 事件只是标记传入的要删除的实体。

有关 MultiplayerSample 项目的更多信息，请参阅 [MultiplayerSample README](https://github.com/o3de/o3de-multiplayersample/blob/development/README.md).

## Simple Player Spawning 组件

Simple Network Player Spawner 关卡组件提供了一种简单的方法来设置 `OnPlayerJoin` 和 `OnPlayerLeave`事件。它实现了处理这些事件的常见用例，具体而言：当玩家加入时，为该玩家创建一个网络实体，当玩家离开时，删除该实体。

有关更多信息，请参阅 [Simple Network Player Spawner 组件](/docs/user-guide/components/reference/multiplayer/simple-player-spawner).
