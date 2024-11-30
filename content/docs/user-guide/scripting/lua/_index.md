---
linktitle: Lua
title: 在 Open 3D Engine 中编写 Lua 脚本
description: 在 Open 3D Engine 中使用 Lua 来自动化您的游戏项目。
toc: true
weight: 100
---

您可以在 Open 3D Engine （O3DE） 中使用 Lua 来促进游戏项目的快速迭代。Lua 是一种功能强大、快速、轻量级、可嵌入的脚本语言。在构建新的游戏和游戏系统时，您可以立即运行更改，而无需编译源代码。

O3DE 使用 Lua 版本 {{< versions/lua >}}.

## Lua 编辑器和调试

O3DE 中的 Lua 开发环境包括 **Lua 编辑器**。Lua 编辑器附带的调试器使用由 AzFramework 的 TargetManagement 管理的 AzNetworking 连接。

## 学习 Lua 

对于学习 Lua 语言本身，[lua.org](http://www.lua.org)网站是一个很好的起点。
+ [Lua 官方文档](http://www.lua.org/docs.html) - 为 Lua 的信息提供一个中心位置，包括 [入门](http://www.lua.org/start.html) 页面。
+ [Programming in Lua](http://www.lua.org/pil/) - 本文是 Lua 编程入门的资源。
+ [Lua {{< versions/lua >}} 参考手册 Manual](http://www.lua.org/manual/{{< versions/lua >}}/) - 提供 Lua 中默认可用的所有函数的参考。

## 在 O3DE 中学习 Lua

在阅读完有关为组件实体系统编写 Lua 脚本的教程后，通过查阅以下资源，了解有关在 O3DE 中使用 Lua 的更多信息。

  + 有关 O3DE 内置 Lua 编辑器的信息，请参考 [Lua 编辑器](./lua-editor).
  + 有关 O3DE 事件总线的信息，请参阅 [使用事件总线 （EBus） 系统](/docs/user-guide/programming/messaging/ebus).
  
## 本部分主题

|主题 |描述 |
| --- | --- |
| [将 Lua 脚本添加到组件实体](add-lua-script) | 使用 Lua Script （Lua 脚本） 组件将脚本功能添加到您的游戏实体中。 |
| [Lua 脚本结构](basic-lua-script) | Lua 脚本的基本结构。 |
| [Lua 编辑器](lua-editor) | 了解 Lua 编辑器。 |
| [属性表](properties) | 指定 O3DE Editor 中显示的 Lua 脚本组件的属性。 |
| [在Lua中使用EBus](ebus) | 编写使用 EBus 在组件之间进行通信的 Lua 脚本。 |
| [Lua 环境 (高级)](environment) | 了解如何在 O3DE Lua 环境中添加 ScriptContext 实例和使用通用代码。|
| [调试 Lua 脚本](debugging-scripts) | 了解如何在 O3DE 中调试 Lua 脚本。 |
| [使用 Lua 编辑器进行调试](debugging-tutorial) | 使用 Lua 编辑器调试 Lua 脚本。 |

## 相关主题

|主题 |描述 |
| --- | --- |
| [Camera 组件](/docs/user-guide/components/reference/camera/camera) | 使用 Camera （摄像机） 组件允许将实体用作摄像机。 |
| [使用用户输入](/docs/user-guide/interactivity/input/using-player-input) |  在 O3DE 中使用 Input （输入） 组件。 |
| [Input 组件 Event Bus 接坑](/docs/user-guide/components/reference/gameplay/input-event-bus-interface) | 使用 Input 组件 EBus （event bus）。 |
| [AWS Client Auth Scripting](/docs/user-guide/gems/reference/aws/aws-client-auth/scripting) | 将 AWS 客户端身份验证 Gem 与 Script Canvas 和 Lua 结合使用的示例。 |
| [AWS Metrics Scripting](/docs/user-guide/gems/reference/aws/aws-metrics/scripting) | 将 Script Canvas 或 Lua 与 AWS Metrics Gem 结合使用以生成和提交指标的示例。 |
| [Gestures Gem](/docs/user-guide/gems/reference/input/gestures) | Gestures Gem 提供对基于手势的常见输入操作的检测。 |
| [Virtual Gamepad Gem](/docs/user-guide/gems/reference/input/virtual-gamepad) | Virtual Gamepad Gem 提供了在 O3DE 项目的触摸屏设备上模拟游戏手柄的控件。 |
| [Tweener Lua Script](/docs/user-guide/interactivity/user-interface/animating/tweener-system/tweener-lua-code) | 了解如何使用 Lua 脚本通过 Scripted Entity Tweener 系统为实体制作动画。 |
| [Tweener Timeline](/docs/user-guide/interactivity/user-interface/animating/tweener-system/tweener-timeline) | 使用 Scripted Entity Tweener 的时间轴功能将动画链接在一起，并对它们进行精细控制。 |
| [Synchronizing Animation Graphs](/docs/user-guide/visualization/animation/character-editor/sync-graph) | 使用同步动画图形在角色之间同步动画。 |
