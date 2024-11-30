---
linktitle: 创建新节点
title: 在 Script Canvas 中创建新节点
description: 了解在 Open 3D Engine （O3DE） 中创建新 Script Canvas 节点的不同方法。
weight: 300
---

Script Canvas 是一个 _extensible_ 可视化脚本系统。您可以通过以下四种方式在 Script Canvas 中创建新节点，按复杂程度升序列出：

* Script Canvas 函数
* 脚本事件
* 行为上下文绑定
* 自定义节点

## 函数

创建新节点的最简单方法之一是将多个现有节点的功能组合成一个可重用的函数节点。使用 **Script Canvas 编辑器** 创建函数节点---无需编写任何C++代码。

![Script Canvas function node example](/images/user-guide/scripting/script-canvas/function-node-example.png)

{{< note >}}
Script Canvas 函数在 Script Canvas 脚本系统之外不可用。
{{< /note >}}

有关更多信息，请参阅 [Script Canvas 函数](editor-reference/functions).

## 脚本事件

您可以使用 Asset Editor 在 Script Canvas 中创建新的发送方和接收方事件节点。通过提供事件名称、参数和返回值在 Asset Editor 中配置脚本事件---类似于在任何编程语言中定义函数，但不需要C++代码。

![Script event node example](/images/user-guide/scripting/script-canvas/script-event-node-example.png)

{{< note >}}
脚本事件可用于所有 O3DE 脚本系统，包括 Lua。
{{< /note >}}

有关更多信息，请参阅 [脚本事件](/docs/user-guide/scripting/script-events/).

## 行为上下文绑定

要向 Script Canvas 公开新的 C++ 类、方法、常量、数据类型和事件，您可以创建到行为上下文的脚本绑定。这是用于为 O3DE Gem 和组件创建新节点的最常用方法。

![Example node created from behavior context binding](/images/user-guide/scripting/script-canvas/behavior-context-node-example.png)

{{< note >}}
您可以将脚本绑定与任何使用行为上下文的 O3DE 脚本系统（如 Lua）一起使用。
{{< /note >}}

{{< note >}}
您无需指定对 Script Canvas Gem 的依赖项，即可创建反映到行为上下文的脚本绑定。
{{< /note >}}

有关更多信息，请参阅《Script Canvas 程序员指南》中的 [从行为上下文创建 Script Canvas 节点](programmer-guide/behavior-context)。

## 自定义节点

为了在创建新节点时获得最大的控制和灵活性，包括添加新功能的能力，您可以使用
* _nodeables_，一种高度可定制的节点实现机制
* 免费函数节点，体现 C++ 免费函数的可自定义节点

为了创建自定义 Script Canvas 节点，我们使用 XML 配置、Jinja 模板和自动 C++ 代码生成工具。
程序员可以使用 Nodeables/free 函数节点通过 SDK 在 C++ 中编写类/函数，该 SDK 保证生成一个类接口，该接口在 Script Canvas Editor 中提供可用节点，并在运行时提供可用对象。

![Example node created from nodeables](/images/user-guide/scripting/script-canvas/nodeable-node-example.png)

{{< note >}}
Nodeables 不可用于其他 O3DE 脚本系统，例如 Lua。
{{< /note >}}

{{< note >}}
使用可节点提供自定义 Script Canvas 节点的 Gem 必须指定对 Script Canvas Gem 的依赖关系。
{{< /note >}}

{{< note >}}
使用免费函数节点提供自定义 Script Canvas 节点的 Gem 必须指定对 ScriptCanvas.Extensions 目标的依赖项。
{{< /note >}}

有关更多信息，请参阅《Script Canvas 程序员指南》中的 [在 Script Canvas 中创建自定义节点](programmer-guide/custom-nodes/) 。
