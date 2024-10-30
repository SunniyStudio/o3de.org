---
linkTitle: 网络
title: 网络
description: 了解 Open 3D Engine (O3DE) 中的底层网络堆栈，以及使用它在游戏和模拟中提供多人游戏功能的Multiplayer Gem。
weight: 950
---

**Open 3D Engine (O3DE)** 包括以下联网和多人游戏功能：

* `AzNetworking`框架，提供底层网络 API。
* **Multiplayer Gem** 框架建立在`AzNetworking`之上，用于为游戏和模拟提供多人联网功能。
* **Remote Tools Gem**，为调试场景提供 O3DE 应用程序之间的连接解决方案。请参阅 [Remote Tools Gem](/docs/user-guide/gems/reference/debug/remote-tools/)文档，了解有关此 Gem 的更多信息。

## 网络框架

### AzNetworking

`AzNetworking` 是一种低级网络传输接口。它提供简单、快速和高效的网络，重点是减少代码大小和复杂性、降低数据包发送和接收操作的延迟以及降低消息处理开销。`AzNetworking` 的特点包括：

* 网络压缩
* 使用 TLS/DTLS 加密
* 序列化
* 高性能信息处理
* 可靠/不可靠的 UDP 数据包
* TCP 数据包
* 管理 UDP 和 TCP 套接字的封装类

如果需要在较低级别的网络类中使用网络类，请参阅 [AzNetworking 部分](aznetworking/)  主题并使用 [AzNetworking API 参考](/docs/api/frameworks/aznetworking/annotated.html)。

### Multiplayer Gem

Multiplayer Gem 支持 O3DE 中基于实体的异步联网，使用事件驱动的网络属性和远程过程调用来同步状态。Gem 在设计时考虑到了多人游戏和其他模拟，并提供了以下功能： 

* 服务器权威网络模型
* 玩家生成器
* 实体复制
* 基于推送的同步
* 事件驱动网络属性
* 可靠和不可靠的远程过程调用RPC
* 本地预测
* 网络输入处理程序

有关如何使用Multiplayer Gem 提供的多人游戏框架的信息，请参阅 [多人游戏部分](multiplayer/)主题。为帮助您尝试使用其功能，请务必下载[多人游戏示例](https://github.com/o3de/o3de-multiplayersample#readme) 并尝试[多人游戏教程](/docs/learning-guide/tutorials/multiplayer/)。

要快速了解 O3DE 网络层和Multiplayer Gem，请观看以下视频：

{{< youtube-width id="FfrkHJJt_X0" title="O3DE - Networking Overview" >}}

## 章节主题

| 主题 | 说明 |
|---|---|
| [AzNetworking](aznetworking/) | 了解 O3DE 中的底层网络协议栈。 |
| [多人游戏](multiplayer/) | 了解多人游戏框架以及Multiplayer Gem所提供的功能。 |
| [网络和多人游戏设置](./settings) | 在`AzNetworking` 和 Multiplayer Gem 中查找控制客户端和服务器行为的设置。 |

## 相关主题

| 主题 | 说明 |
|---|---|
| [AzNetworking API 参考](/docs/api/frameworks/aznetworking/annotated.html) | `AzNetworking` 框架的完整 C++ API 参考资料。 |
| [Multiplayer Gem](/docs/user-guide/gems/reference/multiplayer/multiplayer-gem) | Multiplayer Gem 提供代码扩展和组件，以便在网络上同步 O3DE 组件和实体，为您提供制作多人游戏的工具。 |
| [Multiplayer Compression Gem](/docs/user-guide/gems/reference/multiplayer/multiplayer-compression) | 示例 Gem 展示了如何实现网络压缩。 |
| [教程: 第1个网络组件](/docs/learning-guide/tutorials/multiplayer/first-multiplayer-component/) | 创建网络组件的教程。 |
