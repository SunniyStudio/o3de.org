---
linktitle: Script Canvas
title: 使用 Script Canvas 创建 Gameplay 和其他行为
description: Open 3D Engine （O3DE） Script Canvas 入门，这是一个可视化脚本环境，无需编写代码即可实现运行时逻辑和脚本化行为。
weight: 50
---

**Script Canvas** 是 **Open 3D Engine （O3DE）** 的通用用途可视化脚本环境，可用于为任何实体创建运行时逻辑和脚本化行为。Script Canvas 提供了一个易于使用但功能强大的环境，可以使用与 Lua 和 C++ 相同的框架来编写行为。您可以使用 Script Canvas 为游戏逻辑创建 [脚本](/docs/user-guide/appendix/glossary#scripts)，而无需知道如何编码，并利用 O3DE 消息传递系统生成不需要复杂逻辑来维护状态的小型事件驱动脚本。

Script Canvas 是一个可扩展的系统。您可以从 **Script Canvas 编辑器**中构建自己的可重用 Script Canvas 函数，并使用根据通过 [行为上下文](/docs/user-guide/appendix/glossary#behavior-context) 公开的功能自动创建的 Script Canvas 节点。行为上下文反映运行时代码，通过提供对 C++ 类、方法、属性、常量和枚举的绑定，使其可供脚本访问。行为上下文还为 O3DE 的[AZ::Event](/docs/user-guide/programming/messaging/az-event) 和 [EBus](/docs/user-guide/appendix/glossary#ebus) 消息传递系统提供绑定，使您能够使用 Script Canvas 节点来分派和处理消息和事件。

要进一步扩展 Script Canvas，您可以使用 [AzAutoGen](/docs/user-guide/programming/autogen) 系统强大的自动生成功能创建自己的自定义节点（“nodeables”）。

与更传统的 Lua 模型相比，这种可视化脚本环境的易用性_不会_以牺牲性能为代价。为了保持类似的性能，Asset Processor 将 Script Canvas 图形转换为优化的 Lua 脚本。

要在 O3DE 项目中使用 Script Canvas，必须启用[Script Canvas](/docs/user-guide/gems/reference/script/script-canvas) Gem 及其依赖项。

|主题 |描述 |
| --- | --- |
| [入门指南](get-started/) | 了解 Script Canvas 的基本概念和 Script Canvas 编辑器的界面。 |
| [编辑器参考](editor-reference/) | Script Canvas 编辑器功能、菜单选项和工具的参考。 |
| [创建新节点](creating-new-nodes) | 了解创建新 Script Canvas 节点的不同方法。 |
| [程序员指南](programmer-guide/) | 了解如何使用行为上下文在 Script Canvas 中公开运行时代码，以及如何使用 AzAutoGen 创建自定义节点（“节点”）。 |
| [最佳实践](best-practices) | 获取有关使用 Script Canvas 时的最佳实践的提示。 |
| [调试](debugging) | 了解如何使用 Script Canvas 调试器。 |
