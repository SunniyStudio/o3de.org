---
linktitle: 编写脚本
title: 在 O3DE 中编写游戏脚本
description: 了解 Open 3D Engine （O3DE） 中支持的脚本语言，以便在项目中添加游戏逻辑和行为。
weight: 900
---

**Open 3D Engine （O3DE）** 提供两种脚本技术，因此您可以在项目中添加游戏逻辑和行为：**Script Canvas** 和 **Lua**。

*Script Canvas* 是一个通用的可视化脚本环境。在 **Script Canvas Editor** 中，您可以布局和连接图形节点，以提供逻辑流的可视化表示。Script Canvas 提供了一个易于使用的环境，可以使用与 Lua 和 C++ 相同的框架来编写行为。您可以使用 Script Canvas 创建脚本，而无需编码经验。

*Lua* 是一种强大、快速、轻量级、可嵌入的脚本语言。Lua 有助于在项目中进行快速迭代，因为您可以立即运行更改，而无需重新编译源代码。


### 脚本在 O3DE 中的通信方式

O3DE 功能通过 _behavior context_ 向 Script Canvas 和 Lua 公开。行为上下文反映运行时代码，使脚本可以通过绑定到 C++ 类、方法、属性、常量和枚举来访问它。行为上下文还为 O3DE 的事件总线提供绑定，以便您可以通过 Script Canvas 和 Lua 分派和处理事件。

Script Canvas 和 Lua 的功能都通过组件添加到实体中。您可以拥有多个脚本组件，并在实体中混合和匹配 Lua 和 Script Canvas。这种方法使您能够创建可在整个项目中重复使用的小型、可管理的逻辑和行为模块。


## 章节主题

| 主题 | 说明 |
| --- | --- |
| [Script Canvas](script-canvas/) | 了解如何开始使用 Script Canvas，并查找有关最佳实践、故障排除以及如何使用 Script Canvas 调试器的提示。包括 Script Canvas 编辑器的参考和了解如何扩展 Script Canvas 的程序员指南。 |
| [Lua](lua/) | 了解如何开始将 Lua 脚本添加到您的实体以及使用 Lua 编辑器和调试器。 |
| [Script Events](script-events/) | 了解如何编写脚本事件，以便在 O3DE 中的实体、脚本和脚本系统之间发送和接收数据。 |
