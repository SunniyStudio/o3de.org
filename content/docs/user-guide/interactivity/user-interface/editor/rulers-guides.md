---
linkTitle: 标尺和参考线
description: ' 使用 Open 3D Engine 的 UI 编辑器中的标尺和参考线，直观地指导游戏 UI 元素在 UI 画布上的放置。'
title: 标尺和参考线
weight: 200
---

使用 **标尺** 和 **参考线** 直观地指导 UI 元素在 UI 画布上的位置。

**显示或隐藏标尺**
+ 执行以下操作之一：
  + 按下 **Ctrl+R**
  + 选择 **View**，然后选择**Rulers**

标尺的单位在画布空间中以像素为单位进行测量。标尺上的洋红色线条标记光标的当前位置。

![Magenta lines on the Rulers mark the current location of the cursor.](/images/user-guide/interactivity/user-interface/editor/ui-editor-rulers-guides-magenta.png)

**参考线** 显示为绿线，并充当定位 UI 元素的视觉辅助工具。

**显示或隐藏 **指南****
+ 执行以下操作之一：
  + 按下 **Ctrl+;** (分号)
  + 选择 **View**，然后选择 **Guides**

将参考线放置在画布上的特定像素偏移处。**UI 编辑器**将参考线显示为绿线，您可以围绕绿线或沿绿线放置 UI 元素。

**创建参考线**

1. 确保**标尺**已出现。

1. 单击顶部或侧标尺，然后拖动到画布中。

   如果从顶部标尺开始，则会创建一条水平线。从侧标尺开始创建一条垂直线。

1. 在要放置导板的位置松开。

![To create guides, ensure the Ruler is displayed and then drag down or across from the side or top ruler.](/images/user-guide/interactivity/user-interface/editor/ui-editor-rulers-guides-creating-gif.gif)

1. 要调整参考线的位置，请单击参考线并将其拖动到新位置。

    ![Adjust the guide position by placing the cursor on the guide and dragging it.](/images/user-guide/interactivity/user-interface/editor/ui-editor-rulers-guides-adjust.png)

    {{< note >}}
您必须处于 **移动** 或 **锚点** 模式才能调整参考线的位置。
{{< /note >}}

您可以锁定参考线以防止意外移动它们。锁定参考线还可以更轻松地在参考线之间和周围移动 UI 元素。

**锁定参考线**
+ 选择 **View**，然后选择 **Lock Guides**.

**删除一个参考线**
+ 单击参考线并将其拖出画布。

**删除所有**
+ 选择 **View**，然后选择 **Clear Guides**.

您可以更改所有参考线的颜色。

**更改参考线颜色**

1. 单击视区或层次结构窗格上的空白区域，以便未选择任何 UI 元素，您将在 **Properties** 窗格中看到 **UI 画布**组件。

1. 在 **UI Canvas** 组件中，在**Editor settings**下，点击 **Guide color**并选择一种新颜色。
