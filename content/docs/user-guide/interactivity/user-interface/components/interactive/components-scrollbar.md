---
linkTitle: UI Scroll Bar
description: ' 使用滚动条组件在 Open 3D Engine UI Editor 中添加滚动条。 '
title: UI Scrollbar 组件
weight: 170
---

您可以使用 **ScrollBar** 组件添加可滚动条或手柄，用于操作设置或在滚动框中滚动。

要查看使用 **ScrollBar** 组件完成画布的游戏内示例，请打开项目 SamplesProject 中的关卡 UiFeatures。按 **Ctrl+G** 玩游戏，然后选择 **Components**, **Interactable Components**, **ScrollBar**。您可以查看不同类型的滚动条位置和手柄、与滚动框配对的滚动条以及可见性选项的示例。按 **Esc** 退出游戏。

要在 UI 编辑器 中查看这些相同的画布，请导航到`\Gems\LyShineExamples\Assets\UI\Canvases\LyShineExamples\Comp\ScrollBar` 目录。您可以打开以下画布：
+ `ScrollBoxes.uicanvas` - 与滚动框配对的滚动条示例
+ `Simple.uicanvas` - 具有简单逻辑的滚动条示例
+ `Visibility.uicanvas` - 与滚动框配对时滚动条可见性选项的示例

这是一个水平滚动条：

![Animation of horizontal scroll bar](/images/user-guide/interactivity/user-interface/components/interactive/horizontal-scrollbar.gif)

这是滚动框中的图像，同时具有水平滚动条和垂直滚动条：

![Animation of image scrolling with scroll bars](/images/user-guide/interactivity/user-interface/components/interactive/scrollbar-scrollbox.gif)

滚动条在不使用时也可以自动淡化：

![Example of auto fade scroll bar.](/images/user-guide/interactivity/user-interface/components/interactive/ui-scrollbar-autofade.gif)

您可以添加预制的水平或垂直滚动条元素。执行此操作时，将自动创建一个手柄并将其嵌套在 **Hierarchy** 窗格中。

您可以从切片库添加预构建的 **ScrollBarHorizontal** 或 **ScrollBarVertical** 元素。执行此操作时，滚动条及其手柄将在 **Hierarchy** 窗格中自动创建。

**从切片库添加 ScrollBar 元素**
+ 在 [**UI Editor**](/docs/user-guide/interactivity/user-interface/editor)中，选择 **New**, **Element from Slice Library**, **ScrollBarHorizontal** or **ScrollBarVertical**.

**编辑滚动条组件**

在[**UI Editor**](/docs/user-guide/interactivity/user-interface/editor)的**Properties**面板中，展开 **ScrollBar** 并根据需要执行以下操作：

**Interactable**

见 [Properties](properties)以编辑 Common Interactive Component 设置。

**Elements**, **Handle**

从列表中选择一个元素以提供滚动条的可移动手柄。

**Values**, **Orientation**

选择滚动条的方向：
    + **Horizontal** - Scrollbar 的手柄可向左和向右移动。
    + **Vertical** - Scrollbar 的手柄上下移动。
   
**Values**, **Value**

输入滚动条的初始值 (**0.0** 到 **1.0**).

**Values**, **Handle size**

输入手柄相对于滚动条的大小 (**0.0** 到 **1.0**).

**Values**, **Min handle size**

输入手柄的最小大小（以像素为单位）。

**Actions**, **Change**

输入文本字符串。当滚动条更改值时，此字符串将作为 UI 画布上的操作发送。

**Actions**, **End Change**

输入文本字符串。当滚动条完成更改值时，此字符串将作为 UI 画布上的操作发送。

**Fade**, **Auto Fade When Not In Use**

选中该复选框可使滚动条在一定时间内未使用后淡出为透明。在 **Fade Delay** 中指定延迟时间。

**Fade**, **Fade Delay**

输入滚动条开始淡化为透明度之前的延迟（以秒为单位）。需要选中 **Auto Fade When Not In Use**。

**Fade**, **Fade Speed**

输入滚动条完全淡化为透明所需的时间（以秒为单位）。需要选中 **Auto Fade When Not In Use**。
