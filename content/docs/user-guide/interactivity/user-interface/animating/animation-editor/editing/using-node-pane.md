---
linkTitle: Node Pane
description: ' 使用 O3DE 的 UI 动画编辑器的 Node Pane 在动画序列中添加或删除 UI 元素。 '
title: Using the Node Pane使用 Node 窗格
weight: 100
---

The **Node Pane** in the [Animation Editor](../) 显示所选动画序列中的所有节点。**Node Pane** 中列出的每个项目都被视为一个节点，尽管它们代表序列的不同部分。您可以使用 **Node Pane** 添加或删除 UI 元素节点。录制轨迹时，轨迹节点显示在其 UI 元素下方。

动画序列节点位于顶层，包含其 UI 元素节点的列表。每个 UI 元素节点都包含其 track 节点的列表。

1. **Animation Sequence** 节点

1. **UI Element** 节点

1. **Track** 节点

![Node Pane with nodes](/images/user-guide/interactivity/user-interface/animating/animation-editor/editing/ui-animation-node-pane.png)

**添加新的 UI 元素节点**

1. 在[**UI Editor**](/docs/user-guide/interactivity/user-interface/editor)中，选择一个或多个元素。

1. 在 **动画编辑器**中，右键单击节点窗格中的任意位置，然后选择 **添加选定的 UI 元素**。

删除 UI 元素节点
+ 在 **Animation Editor** 的节点窗格中，右键单击元素节点，然后单击 **Delete**。

**编辑轨道**

1. 在 **Animation Editor** 的节点窗格中，选择一个 track 节点。

1. 右键单击轨迹节点，然后选择以下任一选项：
   + **复制密钥**
   + **复制所选键**
   + **粘贴键**
   + **禁用轨道**

您还可以使用 **Edit Sequence ** 工具直接编辑序列的属性。您可以设置各种属性，例如开始和结束时间、序列是否循环等。

**打开 Edit Sequence 工具**
+ 在 **动画编辑器** 中，单击 **编辑序列** 图标。

![Edit Sequence Icon](/images/user-guide/interactivity/user-interface/animating/animation-editor/editing/ui-animation-edit-sequence.png)
