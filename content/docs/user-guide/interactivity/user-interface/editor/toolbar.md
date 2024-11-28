---
linkTitle: UI 编辑器工具栏
description: ' 使用 Open 3D Engine 的 UI 编辑器中的工具栏来操作您的游戏 UI 元素。 '
title: UI 编辑器工具栏
weight: 100
---

## 交互模式

**UI 编辑器**工具栏具有交互模式。所选交互模式决定了您如何与画布中的 UI 元素交互。

**交互模式**

**选择**
在画布中选择元素。在此模式下，选择是您可以在 UI 元素上使用的唯一操作。

**移动**
选择并移动 UI 元素。移动 UI 元素会修改其偏移量。在 **移动** 模式下，您可以查看锚点构件，但不能与之交互，该构件已禁用。
您可以通过以下方式移动 UI 元素：
+ 选择一个元素并将其拖动到新位置。
+ 在轴 Gizmo 上选择特定轴（X、Y 或 Z），然后一次仅拖动一个轴。
+ 使用键盘箭头微移所选元素。
+ 选择多个元素并使用 [对齐工具](#alignment-tool).

**旋转**
选择并旋转 UI 元素。

**调整**
选择 UI 元素并调整其大小。调整大小会修改 UI 元素的偏移量。

**Anchor**
按 UI 元素的锚点选择和移动 UI 元素。如果您选择一个元素，则锚点 Widget 将显示为蓝色，您可以直接与该元素交互。如果选择多个元素，则会禁用锚点 Widget。
在 **Anchor** 模式下，您可以采用与 **Move** 模式中相同的方式与 UI 元素交互。

## 对齐工具

对齐工具是 **UI 编辑器 **工具栏上的一组按钮。

![The alignment tools are on the UI Editor's toolbar.](/images/user-guide/interactivity/user-interface/editor/ui-editor-toolbar-alignment-tool-buttons.png)

当您的交互模式为 **移动** 或 **锚点** 并且您选择了两个或多个可移动元素时，将启用对齐工具。

{{< note >}}
一种您无法移动的元素类型是由父布局元素控制的元素。
{{< /note >}}

使用这些工具，您可以通过排列元素来对齐元素：
+ 顶部边缘
+ 元素的水平中心
+ 下边缘
+ 左边缘
+ 元素的垂直中心
+ 右边缘

**对齐对象**

1. 选择以下交互模式之一：
   + **Move**
   + **Anchor**

1. 在画布上，选择两个或多个可移动元素。

   工具栏上的对齐工具现已启用。

1. 选择对齐按钮。

对齐工具用于移动元素的方法取决于您使用的交互模式。如果模式为 **Move**，则使用元素的偏移量移动元素。如果 mode 为 **Anchor****，则元素将按其锚点移动。

边界矩形是一个有用的图形，用于了解对齐工具如何确定它如何对齐元素。

想象一个灰色的边界矩形，它包含其中的元素。

![The selected elements are contained by an imaginary bounding rectangle, which determines how the elements are moved.](/images/user-guide/interactivity/user-interface/editor/ui-editor-toolbar-alignment-tool-bounding1.png)

如果按元素的中心垂直对齐，则元素将以边界矩形的中心为中心，该中心将保持其原始位置。

![Align elements vertically by their center.](/images/user-guide/interactivity/user-interface/editor/ui-editor-toolbar-alignment-tool-bounding2.png)

如果按元素的右边缘垂直对齐，则元素的右边缘将与边界矩形的右边缘对齐。

![Align elements vertically by their right edges.](/images/user-guide/interactivity/user-interface/editor/ui-editor-toolbar-alignment-tool-bounding3.png)
