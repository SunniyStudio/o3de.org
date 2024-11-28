---
linkTitle: UI Scroll Box
description: ' 使用滚动框组件在 O3DE 的 UI 编辑器中的可滚动区域内显示内容，例如图像或文本。 '
title: UI Scroll Box 组件
weight: 180
---

您可以使用 **ScrollBox** 组件在可滚动区域内显示内容，例如图像或文本。

此组件通常与蒙版组件一起使用，该组件将内容隐藏在蒙版区域之外。

![Animation of Scrollboxes used with a mask](/images/user-guide/interactivity/user-interface/components/interactive/ui-editor-components-scrollbox.gif)

要查看具有 **ScrollBox** 组件的完整画布的游戏内示例，请打开项目 SamplesProject 中的关卡 UiFeatures。按 Ctrl+G** 玩游戏，然后选择**Components**, **Interactable Components**, **ScrollBox**。您可以查看不同滚动选项、对齐选项、滚动框和其他组件之间的交互以及嵌套滚动框的示例。按 **Esc** 退出游戏。

要在 UI 编辑器 中查看这些相同的画布，请导航到 `\Gems\LyShineExamples\Assets\UI\Canvases\LyShineExamples\Comp\ScrollBox`目录。您可以打开以下画布：
+ `Interactions.uicanvas` - 滚动框和其他交互式组件之间的交互示例
+ `Nested.uicanvas` - 嵌套滚动框的示例
+ `Scrolling.uicanvas` - 不同滚动选项的示例，例如水平、垂直、2D 和不受约束
+ `Snapping.uicanvas` - 不同对齐选项的示例

您可以从切片库添加预构建的 **ScrollBox** 元素。执行此操作时，将自动创建掩码、内容和图像元素并将其嵌套在 **Hierarchy** 窗格中。

**从切片库添加 ScrollBox 元素**
+ 在 [**UI Editor**](/docs/user-guide/interactivity/user-interface/editor) 中，选择 **New**, **Element from Slice Library**, **Scrollbox**.

名为 **ScrollBox** （1） 的元素应用了 **ScrollBox** 组件 （2）。可以将图像添加到 **ScrollBox** 元素的 **Image** 组件 （3） 中，该组件充当滚动框的可视框架。由于 mask 元素及其子元素绘制在滚动框元素的前面，因此您只能在 **ScrollBox** 组件上看到图像的边缘。要增加或减少此图像的可视区域，请调整蒙版元素的 [**Transform2D**](/docs/user-guide/interactivity/user-interface/components/transform2d)  组件中的偏移量。

![ScrollBox component properties](/images/user-guide/interactivity/user-interface/components/interactive/ui-editor-components-scrollbox.jpg)

名为 **Mask** 的元素具有 [**Mask**](../components-mask)  组件，该组件充当视区，您可以通过该视区查看内容。要指定自定义蒙版，可以将图像添加到 **Mask** 元素的 **Image** 组件中。内容将绘制到蒙版的可见区域;蒙版的透明区域会隐藏内容。

**编辑滚动框组件**

在 [**UI 编辑器**](/docs/user-guide/interactivity/user-interface/editor) 的 **Properties** 窗格中，展开 **ScrollBox** 并根据需要执行以下操作：

**Interactable**

见 [Properties](properties) 以编辑 Common Interactive Component 设置。

**Content**, **Content element**

从列表中选择一个元素，以提供要在滚动框中显示的内容。

**Content**, **Initial scroll offset**

输入内容元素的枢轴点与父元素的枢轴点之间的初始偏移值。

**Content**, **Constrain scrolling**

选中该复选框可防止内容滚动到其边缘之外。

**Content**, **Snap**

选择对齐模式：
+ **None** - 无对齐。
+ **To children** - 释放拖动动作时，内容元素的移动方式是，最近的子元素的枢轴点与父元素的枢轴点对齐。例如，当拖动停止时，可以使用此选项将子元素在滚动框中居中。
+ **To grid** - 释放拖动动作时，内容元素的枢轴点将对齐到父元素枢轴点的网格间距的倍数。

**Horizontal scrolling**, **Enabled**

选中该复选框可使内容水平滚动。如果元素或其父元素旋转，则滚动轴也会旋转。您可以同时启用水平滚动和垂直滚动，以在两个方向上滚动。

**Horizontal scrolling**, **Scrollbar element**

从列表中选择一个元素，以提供与滚动框关联的水平滚动条。

**Horizontal scrolling**, **Scrollbar visibility**

选择水平滚动条的可见性行为：
+ **Always visible** - 滚动条始终可见。
+ **Auto hide** - 滚动条在不需要时自动隐藏。滚动条的大小根据垂直滚动条的可见性进行调整。
+ **Auto hide and resize view area** - 与 **Auto hide** 相同，但当滚动条可见时，视图区域的大小也会调整得更小，当滚动条隐藏时，视图区域的大小也会调整得更大。
   
**Vertical scrolling**, **Enabled**

选中该复选框可使内容垂直滚动。如果元素或其父元素旋转，则滚动轴也会旋转。您可以同时启用垂直滚动和水平滚动，以在两个方向上滚动。

**Vertical scrolling**, **Scrollbar element**

从列表中选择一个元素，以提供与滚动框关联的垂直滚动条。

**Vertical scrolling**, **Scrollbar visibility**

选择垂直滚动条的可见性行为：
+ **Always visible** - 滚动条始终可见。
+ **Auto hide** - 滚动条在不需要时自动隐藏。滚动条的大小根据垂直滚动条的可见性进行调整。
+ **Auto hide and resize view area** - 与自动隐藏相同，但当滚动条可见时，视图区域的大小也会调整得更小，当滚动条隐藏时，视图区域的大小也会调整得更大。
   
**Actions**, **Change**

设置每次位置更改时拖动时触发的操作。

**Actions**, **End change**

设置拖动运动完成时触发的操作。
