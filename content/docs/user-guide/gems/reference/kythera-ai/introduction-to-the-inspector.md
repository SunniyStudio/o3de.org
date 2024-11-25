---
linkTitle: The Inspector
title: Introduction the Kythera AI Inspector
description: Overview of the browser-based debugger and behavior tree authoring tool, the Kythera AI Inspector
weight: 700
toc: true
---

**Inspector** 是配置和调试 Kythera AI 的主要工具。它允许您配置 Kythera 的各个方面，并在游戏运行时监控 AI 代理的状态。

要使用 Inspector：

*   启动编辑器、启动器或服务器。
*   打开浏览器（建议使用 Chrome）打开 [http://localhost:8081/](http://localhost:8081/).

## Live View

**Live**选项卡允许您查看当前正在运行的 AI 的状态。它有两个子选项卡，一个用于检查 AI 黑板的内容，另一个用于查看正在执行的各个 AI 行为树。

## Blackboards

在 **Blackboards** 子选项卡中，您可以检查任何活动 AI 的实体状态树。如果打开特定 AI 的树（例如 KytheraTest1），则会显示该 AI 的 **Entity State Tree**，即表示其当前状态的黑板树。一些感兴趣的特定子黑板：

*   调试黑板允许专门为此实体打开各种调试选项
*   进入 BehaviorState，查看自此行为开始运行以来已写入此行为的行为黑板的任何值
  
![introduction to inspector live view](/images/user-guide/gems/kythera-ai/introduction-to-inspector-live-view.png)

## Behavior Tree Debugger

在 **Behavior Trees** 子选项卡中，你可以查看任何 AI 的活动行为树，并查看其实时更新的当前状态。从下拉列表中选择所需的 AI。如果您按下 Live 按钮，您将看到树实时更新。再按一次可停止。

![introduction to inspector debugger](/images/user-guide/gems/kythera-ai/introduction-to-inspector-debugger.png)

## BT Editor

作为 Inspector 一部分的另一个主要工具是 [Behavior Tree editor](./behavior-tree-editor)。使用此工具，您可以创建新的 BT 并快速轻松地编辑现有 BT。
