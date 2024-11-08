---
linkTitle: 会话管理
title: 会话管理
description: 了解如何使用 Open 3D Engine （O3DE） 中的 AWS GameLift Gem 管理游戏中的多人游戏会话。
toc: true
weight: 300
---

游戏会话定义为您的服务器进程可供主机玩家使用的时间。通常，会话在满足游戏特定条件时结束，其中可能包括所有玩家都已离开会话、玩家达到获胜条件或时间段已过期。

对于 GameLift，会话具体是指在 Amazon GameLift 上运行的游戏实例，该实例已准备好托管玩家。当您的服务器通知 GameLift 结束会话时，会话结束。

## 主题
2
| 主题 | 说明 |
| - | - |
| [会话管理集成](integration/) | 了解将会话管理集成到游戏客户端和专用服务器的要求。 |
| [会话管理 C++ API](cpp-api/) | 了解如何使用 C++ API 向游戏添加会话管理。|
| [会话管理脚本编程](scripting/) | 了解如何使用 Script Canvas 向游戏添加会话管理。 |
