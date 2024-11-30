---
linktitle: 删除节点
title: 在 Script Canvas 中删除节点
description: 了解如何在 Open 3D Engine 中删除 Script Canvas 节点。
weight: 400
---

Script Canvas 提供了多种删除节点的方法。

删除一个或多个节点及其连接

1. 选择要删除的一个或多个节点。

1. 执行以下步骤之一。
   + 按 **Delete**。
   + 右键单击，然后选择 **Delete**。

**删除单个节点**

* 在要删除的节点上，按 **Alt+Left-click**。如果已删除的节点已连接到另一个节点，则会删除该连接。如果已删除的节点连接到其他两个节点，则其余节点将相互连接。

    ![Using Alt+Left-click to delete a node in Script Canvas Editor.](/images/user-guide/scripting/script-canvas/nodes-deleting-1.gif)

    {{< note >}}
您不能使用 **Alt+Left-click** 一次删除多个节点。
    {{< /note >}}

**删除图表中所有未使用的节点**

* 在 **Script Canvas Editor**中, 选择 **Edit**, **Remove Unused**, **Nodes**.

    ![Removing all unused nodes from a graph in the Script Canvas Editor.](/images/user-guide/scripting/script-canvas/nodes-deleting-2.png)
