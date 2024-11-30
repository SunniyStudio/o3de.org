---
linktitle: 添加和连接节点
title: 在 Script Canvas 中添加和连接节点
description: 了解如何在 Open 3D Engine 中添加和连接 Script Canvas 节点。
weight: 100
---

您可以通过将节点从 **Node Palette** 拖放到画布上来将节点添加到画布上。使用 Node Palette 的搜索功能按名称搜索节点。

您还可以在不使用 **Node Palette** 的情况下快速添加和连接节点，如以下部分所述。

## 创建链接节点链

您可以使用菊花链功能快速连续地将节点添加到图形中。Script Canvas 会自动将每个新节点上的引脚链接到上一个节点。

**创建链接节点链**

1. 在图表右侧，按 **Shift** + **右键单击**。

1. 在上下文菜单搜索框中，输入要添加的节点的名称或部分名称。

1. 按 Enter** 接受搜索结果，或单击结果列表中另一个节点的名称。

    新节点显示在图表上，并带有一条从 output 引脚延伸的线。上下文菜单将自动打开，供您添加另一个节点并将其链接到前一个节点。

1. 继续添加所需数量的已连接节点。以前创建的节点会自动向左滚动。

    {{< note >}}
如果您首先在图表左侧添加节点，则您创建的节点会快速滚动出视图。要查看您之前创建的节点，请先按图表右侧的 **Shift+右键单击**。
    {{< /note >}}

    ![Quickly adding nodes in succession in the Script Canvas Editor.](/images/user-guide/scripting/script-canvas/nodes-adding-and-connecting-1.gif)

1. 要退出菊花链模式，请按**ESC**.

## 从现有节点的输出引脚创建节点

**从现有节点的输出引脚创建连接的节点**

1. 从现有节点上的输出引脚，将一条线拖动到画布上。

1. 在上下文菜单搜索框中，输入要添加的节点的名称或部分名称。

1. 按 Enter** 接受搜索结果，或单击结果列表中另一个节点的名称。新节点上的引脚会自动连接到现有节点上的引脚。

    ![Creating a node from the output pin of an existing node in the Script Canvas Editor.](/images/user-guide/scripting/script-canvas/nodes-adding-and-connecting-2.gif)

1. 要从现有节点的输出引脚创建链接节点链，请按住 Shift，然后从输出引脚拖动一条线到画布上。

## 在两个连接的节点之间插入一个节点

要在两个连接的节点之间插入节点并自动连接新节点，可以使用以下方法：

* 在节点之间插入现有节点。
* 在节点之间创建新节点。

**在两个连接的节点之间插入现有节点**

1. 将节点拖动到连接两个节点的线上，然后将新节点保持在原位。

1. 当出现展开的盒形动画时，表示节点引脚已连接。Script Canvas 将周围的节点推到一边以容纳新节点。

    ![Inserting an existing node between two connected nodes in the Script Canvas Editor.](/images/user-guide/scripting/script-canvas/nodes-adding-and-connecting-3.gif)

1. 默认情况下，节点微移功能处于启用状态。要更改它，请选择**Edit**, **Settings**, **Global Preferences**并选择或清除 **Allow Node Nudging On Splice** 选项。

    ![Configuring the node nudging preferences in the Script Canvas Editor.](/images/user-guide/scripting/script-canvas/nodes-adding-and-connecting-4.png)

**在两个连接的节点之间创建节点**

1. 右键单击连接两个节点的线。

1. 在上下文菜单搜索框中，执行以下操作之一：

    * 输入所需节点的名称，然后按 Enter。
    * 从列表中选择一个节点。

    新节点上的引脚会自动连接到左侧和右侧的节点。

    ![Creating a node between two nodes and connecting it automatically in the Script Canvas Editor.](/images/user-guide/scripting/script-canvas/nodes-adding-and-connecting-5.gif)

## 自动连接两个现有节点

您可以通过将一个节点拖动到另一个节点上来连接两个节点。Script Canvas 为您连接相应的引脚。

**自动连接两个节点**

1. 将节点拖动到要连接的节点一侧，同时短暂地将新节点保持在原位。例如，将一个节点的左侧拖动到第二个节点的右侧。

1. 当重叠节点上出现展开的框形动画时，表示节点引脚已连接。

1. 调整新节点在图表上的位置。

    ![Connecting two existing nodes by superimposing them in the Script Canvas Editor.](/images/user-guide/scripting/script-canvas/nodes-adding-and-connecting-6.gif)
