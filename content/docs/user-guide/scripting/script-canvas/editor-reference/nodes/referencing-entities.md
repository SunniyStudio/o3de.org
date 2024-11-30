---
title: 引用实体
description: 了解如何从 Script Canvas 节点引用实体作为目标。
weight: 600
---

节点可以包含实体属性。这些属性告诉节点要影响哪个实体。默认情况下，许多节点引用 **Self**，该实体包含 [Script Canvas](/docs/user-guide/components/reference/scripting/script-canvas/) 组件，该组件指定包含该节点的脚本。或者，您可以引用除 Self 以外的其他实体。

## 从节点引用实体

1. 从 **Node Palette**（节点调色板）中，找到要添加到脚本中的节点，并将其拖到画布上。

1. 在节点中，将指针置于实体属性上，然后单击目标图标。

    ![Select and deselect entities for nodes in the Script Canvas Editor.](/images/user-guide/scripting/script-canvas/nodes-select-entity.png)

1. 在 O3DE Editor 视区或 **Entity Outliner** 中，选择要引用的实体。

1. 要清除实体，请将指针置于实体属性上，然后单击 **x** 图标。

1. 要将属性重置回自身，请右键单击实体属性，然后选择 **Reset Value**。
