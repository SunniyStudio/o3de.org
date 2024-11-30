---
linktitle: 断开节点
title: 断开 Script Canvas 中的节点连接
description: 了解如何在 Open 3D Engine 中断开 Script Canvas 节点的连接。
weight: 150
---

Script Canvas 提供了多种删除节点之间连接的方法。

**使用上下文菜单删除连接**

1. 将指针悬停在连接上。白线将变为一行移动的蓝色虚线。

1. 右键单击蓝色虚线。

1. 在上下文菜单中，选择 **Delete**。

**使用 **Delete** 键删除连接**

1. 将指针悬停在连接上。白线将变为一行移动的蓝色虚线。

1. 左键单击蓝色虚线。

1. 按 **删除**。

    {{< note >}}
您必须先左键单击蓝色虚线，然后才能按 **Delete**。
    {{< /note >}}

**使用 **Alt** 键删除连接**

1. 按住 **Alt** 键后，将指针悬停在行上。该线将变为红色。

1. 左键单击该行。

**通过“摇动”节点来删除节点的连接**

1. 使用指针选择一个连接的节点，然后用摇动手势移动它，以将其与其他节点分开。如果已删除的节点连接到其他两个节点，则其余节点将相互连接。
![Shaking nodes to disconnect them in the Script Canvas Editor.](/images/user-guide/scripting/script-canvas/nodes-disconnecting.gif)

1. 您可以在**Edit**, **Settings**, **Global Preferences**, **Shake to Desplice**中更改此选项。
