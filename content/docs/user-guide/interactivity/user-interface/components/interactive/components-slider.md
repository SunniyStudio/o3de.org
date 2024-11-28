---
linkTitle: UI Slider
description: ' 使用滑块组件使元素的行为类似于 O3DE 的 UI 编辑器中的滑块。 '
title: UI Slider 组件
weight: 150
---

您可以使用 **Slider** 组件使元素的行为类似于滑块。此组件通常应用于具有三个可视子元素的元素：一个直接子元素，称为 **Track**，以及轨道的两个子元素，称为 **Fill** 和 **Handle**。

![Element with Slider component on canvas](/images/user-guide/interactivity/user-interface/components/interactive/ui-editor-slider-components.png)

要查看使用 **Slider** 组件完成的画布的游戏内示例，请在项目 SamplesProject 中打开关卡 UiFeatures。按 **Ctrl+G** 玩游戏，然后选择 **Components**, **Interactable Components**, **Slider**。您可以查看不同滑块行为和位置的示例。按 **Esc** 退出游戏。

要在 UI 编辑器 中查看这些相同的画布，请导航到`\Gems\LyShineExamples\Assets\UI\Canvases\LyShineExamples\Comp\Slider`目录。您可以打开以下画布：
+ `Behavior.uicanvas` - 滑块行为示例，例如填充类型、手柄和用于控制滑块的按钮
+ `Rotation.uicanvas` - 滑块定位示例

您可以从切片库添加预构建的 **Slider** 元素。执行此操作时，滑块的 track、fill 和 handle 将在 **Hierarchy** 窗格中自动创建。

**从切片库添加 Slider 元素**
+ 在 [**UI Editor**](/docs/user-guide/interactivity/user-interface/editor)，选择 **New**, **Element from Slice Library**, **Slider**.

**编辑滑块组件**

在[**UI Editor**](/docs/user-guide/interactivity/user-interface/editor)的**Properties**面板中，展开 **Slider** 并根据需要执行以下操作：

**Interactable**

见 [Properties](properties) 以编辑 Common Interactive Component 设置。

**Elements**, **Track**

从列表中选择一个元素，以提供滑块的背景并限制操纵器的移动。

**Elements**, **Fill**

从列表中选择一个元素以提供滑块的背景，从下限到操纵器位置的中心。

**Elements**, **Manipulator**

从列表中选择一个元素以提供滑块的可移动旋钮。

**Value**, **Value**

输入滑块的初始值。

**Value**, **Min**

输入滑块的下限。

**Value**, **Max**

输入滑块的上限。

**Value**, **Stepping**

输入 step 值。例如，使用 `1` 只允许整数值。

**Actions**, **Change**

输入文本字符串。当滑块完成更改值时，此字符串将作为 UI 画布上的操作发送。

**Actions**, **End Change**

输入文本字符串。当滑块的值发生变化时，此字符串将作为 UI 画布上的操作发送。
