---
linkTitle: Session Management 集成
title: Session Management 集成
description: 了解如何使用 Open 3D Engine （O3DE） 中的 AWS GameLift Gem 将多人游戏会话管理集成到您的游戏和专用服务器中。
toc: true
weight: 200
---

## 集成游戏客户端

对于客户端应用程序，您的游戏必须实现以下用例来管理会话：
- `CreateSession`
- `SearchSessions`
- `JoinSession`
- `LeaveSession`

AWS GameLift Gem 同时提供 [C++ API](cpp-api/) 和 [scripting](scripting/)。您可以使用任一方法实现这些使用案例。

## 集成专用服务器

要在服务器和 GameLift 之间建立通信，您必须通知 GameLift 您的服务器已准备就绪，然后让您的服务器响应 GameLift 通知。

有关 GameLift 服务器初始化的更多详细信息，请参阅AWS GameLift Gem C++ API的 [服务器初始化](cpp-api/#server-initialization)。

有关 GameLift 服务器通知的更多详细信息，请参阅AWS GameLift Gem C++ API的 [服务器通知](cpp-api/#server-notifications)。


## 相关信息

- [将游戏与自定义游戏服务器集成](https://docs.aws.amazon.com/gamelift/latest/developerguide/integration-custom-intro.html)
