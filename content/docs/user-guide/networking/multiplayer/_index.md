---
linktitle: 多人游戏
title: 多人游戏框架
description: 了解 Open 3D Engine (O3DE) 中的多人游戏支持，以及如何使用Multiplayer Gem 提供的多人游戏框架。
weight: 200
---

**Multiplayer Gem**提供的多人游戏框架建立在 `AzNetworking`之上。Multiplayer Gem支持**Open 3D Engine (O3DE)**中基于实体的异步联网，使用事件驱动的网络属性和远程过程调用在网络上同步O3DE组件和实体，为您提供制作多人游戏的工具。

Multiplayer Gem 支持以下内容：

* 服务器权威网络模型
* 玩家生成器
* 实体复制
* 基于推送的同步
* 事件驱动网络属性
* 可靠和不可靠的远程过程调用
* 本地预测
* 网络输入处理程序

## 章节主题
1
| 主题 | 说明 |
|---|---|
| [概述](overview) | O3DE Multiplayer Gem概述。包括提供网络状态同步的多人游戏组件介绍。 |
| [配置项目](configuration) | 如何在项目中添加并启用 O3DE Multiplayer Gem。 |
| [运行多人游戏项目](running) | 如何运行使用 O3DE Multiplayer Gem 的项目。 |
| [自动创建多人游戏组件](autocomponents) | 如何使用 AzAutoGen 系统自动创建用于Multiplayer Gem 的组件。 |
| [分离客户端和服务器](code_separation) | 如何分离客户端和服务器逻辑并建立依赖关系。|
| [在编辑器中测试多人游戏项目](test-in-editor) | 在**O3DE 编辑器**中处理多人游戏项目时，如何自动启动本地服务器或连接到远程持久服务器。 |
| [网络实体层次结构](hierarchy) | 如何将网络实体划分为层次，共同处理其输入。 |
| [生成玩家实体](spawning) | 如何生成一个实体供连接玩家控制。 |
| [Debugging Multiplayer Desyncs](debug-desync) | 如何使用内置的 Desync Audit Trail 工具分析和调试多人不同步现象。 |

## Related topics

| Topic | Description |
|---|---|
| [Network and Multiplayer Settings](../settings) | Settings to control the client and server behavior in `AzNetworking` and the Multiplayer Gem. |
| [Multiplayer Components](/docs/user-guide/components/reference/#multiplayer) | Reference documentation for multiplayer components. |
| [Multiplayer Gem API Reference](/docs/api/gems/multiplayer/annotated.html) | The complete C++ API reference for the O3DE Multiplayer Gem. |
| [Multiplayer Compression Gem](/docs/user-guide/gems/reference/multiplayer/multiplayer-compression) | An example Gem showing how to implement network compression. |
| [Multiplayer Sample](https://github.com/o3de/o3de-multiplayersample#readme) | Download the multiplayer sample to help you experiment with the features in the Multiplayer Gem. |
| [Tutorial: Your First Network Component](/docs/learning-guide/tutorials/multiplayer/first-multiplayer-component/) | Tutorial for creating a network-enabled component. |
