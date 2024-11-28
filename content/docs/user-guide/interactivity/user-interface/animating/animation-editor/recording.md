---
linkTitle: 录制动画数据
description: ' 创建新序列后，使用 O3DE 的 Animation Editor 录制新动画。'
title: 录制动画数据
weight: 200
---

录制动画通常包括三个步骤：

1. 创建新的动画序列。

1. 向该序列添加 UI 元素。

1. 打开动画录制以捕获元素属性中的更改。

添加 UI 元素还会向序列中添加节点。此后，只要您进入录制模式，对于您对此 UI 元素所做的任何更改，都会自动将轨迹添加到您的动画中。您无需手动添加轨道。有关详细信息，请参阅 [使用节点窗格](editing/using-node-pane).

您可以从 **Animation Editor** 菜单或工具栏创建动画序列。

**创建新的动画序列**
在 [**Animation Editor**](./) 中，执行以下操作之一：
+ 从 **序列** 菜单中，选择 **新建序列**。
+ 单击工具栏上的 **添加序列** 图标。

**将 UI 元素添加到序列中**

1. 在 [UI 编辑器**](/docs/user-guide/interactivity/user-interface/editor) 中，选择要添加动画效果的 UI 元素。

1. 在 **Animation Editor** 中，右键单击您创建的序列，然后单击 **Add Selected UI Element（s）**。

**录制动画序列**

1. 在 **Animation Editor** 工具栏中，单击 **Record** 图标。

1. 在 **UI 编辑器**中，使用 **属性** 窗格或视区窗格对所选 UI 元素进行更改。

1. 完成所有更改后，单击 **Animation Editor** 工具栏中的 **Stop** 图标。

{{< note >}}
在当前版本中，并非所有组件属性都可以记录。例如，枚举值（如图像组件的图像类型）无法进行动画处理。
{{< /note >}}

录制轨道后，该轨道将显示在其 UI 元素下方。节点窗格列出了您当前的动画序列。有关 **Node Pane** 的更多信息，请参阅 [使用节点窗格](editing/using-node-pane)
