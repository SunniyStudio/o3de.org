---
linkTitle: BT Editor
title: Behavior Tree Editor
description: Kythera AI Inspector 的行为树编辑器工具概述
weight: 800
toc: true
---

您可以在 Kythera 基于节点的行为树编辑器 **BT Editor** 中为 Kythera 代理创建行为树 （BT）。

在 Web 浏览器中打开 Kythera **Inspector**，然后选择 BT 编辑器选项卡。将显示以下对话框：

![BT editor](/images/user-guide/gems/kythera-ai/bt-editor-choose-bt.png)

{{< note >}}
BT 编辑器仅在 Chrome 浏览器中受支持。如果未显示现有树的列表，请刷新浏览器。
{{< /note >}}

选择现有树之一，例如 Idle，您将看到 BT 编辑器视图。

![BT editor view](/images/user-guide/gems/kythera-ai/bt-editor-show-bt.png)

在此视图中，您可以移动、添加和删除节点，或者选择节点并编辑其属性。

* 可以通过拖动节点来移动节点。但是，无法移动根节点，该节点由交叉圆圈图标指示，并包含行为树的名称。

* 通过在左侧菜单中找到要添加的节点，然后将该节点拖动到树视图中来添加节点。将节点添加到页面后，可以通过从父节点的底部拖动到新节点的顶部，将其作为子节点连接到 **FlowControl** 或 **Conditional** 节点。

* 只需选择节点并按 **Delete** 键，即可从树中删除节点。无法删除树的根节点。如果要删除整个树，可以使用 **File** 下拉菜单。

* 可以通过选择节点，然后在屏幕右下角显示的属性对话框中进行更改来编辑节点的属性。

## 导航树

导航 BT Editor 树状视图非常直观。使用鼠标滚轮缩放树视图。单击并拖动背景上的 以平移树视图。

除了可用树列表外，还有三个按钮可用于导航树。自上而下工作：

* ![bt editor home button](/images/user-guide/gems/kythera-ai/bt-editor-button-home.png) **Home** 按钮将视图返回到根节点。

* ![bt editor zoom button](/images/user-guide/gems/kythera-ai/bt-editor-button-zoom.png) **Zoom**按钮框住树，以便可以看到整个树。

* ![bt editor view button](/images/user-guide/gems/kythera-ai/bt-editor-button-view.png) 使用 **View** 按钮，可以在要聚焦的区域周围 **拖动** 选择框，从而创建包含所选区域的视图。

建议使用 **Zoom** 按钮查找您感兴趣的树区域，然后使用 **View** 按钮放大该区域。

## 节点类型

行为树编辑器中有四种主要类型的节点可用：

* **FlowControl**节点用于控制整个树中的操作流。这些节点以及 **Conditional** 节点可以附加子节点。子节点以某种方式执行，具体取决于您使用的 **FlowControl**。

* **Conditional** 节点用于装饰（成为其父节点）另一个节点，并控制是否根据条件语句执行该节点

* **Decorator** 节点是一类通用的装饰节点，用于更改节点的返回值或游戏状态的其他方面。它们还可用于重复执行其下方的节点，直到满足特定条件。

* **Leaf** 节点不能有子节点。它们终止分支的执行并在返回之前执行某种功能。

## 行为树验证

每当对树的任何部分进行更改（包括移动节点）时，**Validator** 都会重新计算整个树的正确性。这是因为行为树是从上到下（通过连接）执行的，然后从左到右执行。也就是说，每个节点的子节点将按照它们从左到右出现的顺序执行。如果节点未正确配置，它将显示为红色，单击该节点将显示有关如何修复该节点的消息。

![bt editor validation](/images/user-guide/gems/kythera-ai/bt-editor-validation.png)

## 保存行为

行为会自动导入到游戏中，并在您每次更改时保存。无需显式保存它们。了解这一点很重要，因为编辑器中当前没有撤消/还原功能（除了撤消会话中的最后一个操作），因此如果要返回到以前的版本，则需要使用源代码控制。

如果您计划对树进行重大更改，但不想丢失当前版本，则可以使用 **File > Duplicate** 功能复制树。
