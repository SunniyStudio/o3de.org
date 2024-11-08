---
linkTitle: FlexMatch 集成
title: FlexMatch 集成
description: 了解如何使用 Open 3D Engine （O3DE） 中的 AWS GameLift Gem 将 FlexMatch 集成到您的游戏和专用服务器中。
toc: true
weight: 200
---

## 先决条件

- 完成 [游戏会话管理集成](../session-management/integration) 用于游戏客户端和专用服务器。
  
## 集成游戏客户端

要支持包括回填在内的可选 FlexMatch 功能，您的客户端应用程序需要实施以下使用案例：
- `StartMatchmaking`
- `StopMatchmaking`
- `StartPolling`
- `StopPolling`
- `AcceptMatch`

AWS GameLift Gem 提供了 [C++ APIs](cpp-api/) 和 [scripting](scripting/)。您可以使用任一方法实现这些使用案例。


## 集成专用服务器

要支持可选的手动回填，您的服务器应实施以下使用案例：
- `StartMatchBackfill`
- `StopMatchBackfill`
- `OnUpdateSessionBegin`
- `OnUpdateSessionEnd`

## 相关信息

- [为 FlexMatch 准备游戏](https://docs.aws.amazon.com/gamelift/latest/flexmatchguide/match-integration-intro.html)
