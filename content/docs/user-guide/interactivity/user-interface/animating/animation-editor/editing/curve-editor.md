---
linkTitle: Curve Editor
description: ' 使用 Open 3D Engine 的 UI Animation 编辑器中的 Curve Editor 来更改浮点值随时间变化的方式。 '
title: 在 UI Animation Editor 中使用 Curve Editor。
weight: 300
---

**曲线编辑器** 将动画显示为函数曲线。每个轨迹的曲线都表示一个属性值（例如锚点、偏移、颜色或 UI 元素的任何属性）的动画。

**曲线的元素**

1. 曲线或样条曲线

1. 样条键

1. 切线控制柄

![Example elements of a curve in the Curve Editor.](/images/user-guide/interactivity/user-interface/animating/animation-editor/editing/ui-animation-curve-editor.png)

曲线的路径表示关键帧之间值的过渡。如果每个关键帧之间的值沿直线变化（线性），则关键帧之间的过渡将不会平滑。默认曲线使值平滑地缓入和缓出。每个键都有一个 in tangent 和一个 out tangent。根据首选效果，您可以使用工具栏图标将切线切换到 auto、zero、step 或 linear。您也可以手动拖动切线控制柄。

默认情况下，动画轨道以平滑过渡录制。您可以使用 ****Curve Editor**** 顶部工具栏中的按钮来更改曲线在所选关键帧任一侧的行为方式。您还可以将样条曲线关键帧拖动到时间轴中的其他点。

显示 **曲线编辑器****
+ 在 **UI 动画** 编辑器中，选择 **视图**、******曲线编辑器**** 或 **视图**、**两者**。

**放大或缩小**
+ 滚动鼠标滚轮

**平移视图**
+ 在曲线编辑器****中，使用鼠标中键拖动

**调整样条键**

1. 在 **Node Pane**（节点窗格）中，选择一个轨道。该轨迹的曲线将显示在 ****Curve Editor**** 中。

1. 在 ****Curve Editor**** 中，选择一个样条键。

1. 执行以下一项或多项操作：
   + 将样条键拖动到时间轴上的其他点。
   + 使用工具栏按钮选择预设：自动、零、步进或线性。

您可以一次选择要修改的多个样条曲线键。选择后，可以将它们一起移动，设置它们的入切线和出切线，等等。

**选择多个样条键**
+ 在 ****Curve Editor**** 中，将选择框拖到要选择的所有样条键上。

**一次编辑多个元素**

1. 在 **Animation Editor** 的节点窗格中，选择包含要编辑的子轨道的轨道。

1. 将选择框拖动到要选择的样条键上。

1. 拖动样条键以编辑其位置。
