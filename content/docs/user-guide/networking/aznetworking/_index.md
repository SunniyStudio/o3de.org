---
linktitle: AzNetworking
title: AzNetworking 框架
description: 了解 Open 3D Engine (O3DE) 的底层网络协议栈。
weight: 100
---

`AzNetworking`是**Open 3D Engine (O3DE)**中的一个底层网络接口。`AzNetworking`提供简单、快速和高效的网络连接，重点是减少代码大小和复杂性、降低数据包发送和接收操作的延迟以及降低消息处理开销。

## 章节主题

| 主题                           | 说明 |
|------------------------------|---|
| [数据包结构](./packets)           | 有关 `AzNetworking` 用于 TCP 和 UDP 数据包的数据包结构，以及如何管理分片的 UDP 数据包的信息。 |
| [自动数据包](./autopackets)      | 有关如何通过`AzNetworking`发送和接收数据包的信息可通过 XML 生成。 |
| [使用DTLS的UDP加密](./encryption) | 如何使用 O3DE 支持通过数据报传输层安全（DTLS）的 UDP 安全连接。 |
| [序列化](./serializers)         | 有关序列化和可用序列化器类型的信息。 |

## 相关主题

| 主题 | 说明 |
|---|---|
| [AzNetworking API 参考](/docs/api/frameworks/aznetworking/annotated.html) | `AzNetworking` 框架的完整 C++ API 参考资料。 |
| [网络和多人游戏设置](./settings) | 在`AzNetworking` 和 Multiplayer Gem 中查找控制客户端和服务器行为的设置。 |
