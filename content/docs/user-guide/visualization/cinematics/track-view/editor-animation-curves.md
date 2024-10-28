---
description: ' 使用 <guilabel>Track View</guilabel> 编辑器中的曲线编辑器管理 Open 3D Engine中的动画。 '
title: Using Animation Curves
---

**Curve Editor**将动画显示为函数曲线。每个轨迹的曲线代表一个属性值的动画，如锚点、偏移、颜色或用户界面元素的任何属性。

打开**Curve Editor**

1. 完成以下之一：
   + 在Track View中，选择 **View**, **Curve Editor**。
   + 在 **View** 工具栏中，选择 **Curve Editor** 图标 ![Curve editor button](/images/user-guide/cinematics/cinematics-curve-icon-track-view-editor.png)。

1. 在时间轴中选择一个关键帧，在**Curve Editor**中查看。

    {{< note >}}
如果您希望同时使用两个工具，可以同时打开Track View 和 **Curve Editor** 。
{{< /note >}}

曲线由以下三个部分组成

1. 曲线或样条线

1. 曲线或样条曲线上的关键键

1. 键的切线手柄

![Elements of a curve in Track View for the Curve Editor](/images/user-guide/cinematics/cinematics-track-view-editor-curves.png)

**编辑曲线中的元素**

1. 选择一个键，查看相关的切线手柄，然后拖动键上的方框或切线手柄的两端（包括统一切线和自动切线）进行操作。

1. 移动关键帧时，按住**Shift**键可将移动限制在时间范围内。

1. 按住**Alt**可在播放头位置周围缩放选定的关键帧。

曲线的路径表示关键帧之间数值的过渡。如果数值在每个关键帧之间呈直线变化（线性），关键帧之间的过渡就不平滑。默认曲线会使数值平滑地缓入缓出。每个关键帧都有一个输入切线和一个输出切线。根据喜欢的效果，可以使用工具栏图标将切线切换为自动、零、阶跃或线性。您还可以手动拖动切线手柄。

默认情况下，动画轨迹以平滑过渡的方式录制。你可以使用**Curve Editor**顶部工具栏中的按钮来更改选定键两侧的曲线行为。您还可以将样条曲线键拖动到时间轴上的不同点。

请参阅以下在**Curve Editor**中工作的提示：
+ 要放大或缩小，请滚动鼠标滚轮
+ 要平移视图，单击并拖动鼠标中间的滚轮
+ 要选择多个样条曲线键，请单击并拖动选择键

**调整样条线关键帧**

1. 在**Node Pane**中，选择一条轨道。该轨道的曲线将显示在**Curve Editor**中。

1. 在**Curve Editor**中，选择一个样条键。

1. 执行以下操作之一：
  + 将样条线键拖动到时间线上的不同点。
  + 使用工具栏按钮选择预设：自动、零、阶跃或线性。

**一次编辑多个元素** 

1. 在**Node**浏览器中，选择父轨道或子轨道。

1. 将样条键拖动到时间轴上的不同点。

1. 使用工具栏按钮选择预设：自动、零、阶跃或线性。

