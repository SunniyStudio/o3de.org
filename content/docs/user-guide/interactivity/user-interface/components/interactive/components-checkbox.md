---
linkTitle: UI Checkbox
description: ' 使用复选框组件使元素的行为类似于 O3DE 的 UI 编辑器中的复选框。 '
title: UI Checkbox 组件
weight: 120
---

您可以使用 **Checkbox** 组件使元素的行为类似于复选框。此组件通常应用于具有两个可视子元素的元素：一个元素在选中复选框时显示，另一个元素在清除复选框时显示。

![UI Editor Checkbox component](/images/user-guide/interactivity/user-interface/components/interactive/ui-editor-checkbox-components.png)

要查看带有 **Checkbox** 组件的完整画布的游戏内示例，请在项目 SamplesProject 中打开关卡 UiFeatures。按 **Ctrl+G** 玩游戏，然后选择 **Components**, **Interactable Components**, **CheckBox**。您可以查看颜色目标、复选框交互区域以及打开和关闭元素的示例。按 **Esc** 退出游戏。

要在 **UI 编辑器**中查看这些相同的画布，请导航到 `\Gems\LyShineExamples\Assets\UI\Canvases\LyShineExamples\Comp\CheckBox` 目录。您可以打开以下画布：
+ `Area.uicanvas` - 不同复选框交互区域的示例
+ `ColorTargets.uicanvas` - 与复选框交互时不同颜色目标的示例
+ `OnOff.uicanvas` - 不同复选框 on 和 off 元素的示例

您可以从切片库添加预构建的 **Checkbox** 元素。执行此操作时，将在 **Hierarchy** 窗格中自动创建一个带有文本字符串 “Checkbox” 的基本复选框和该框的复选图像。

**从切片库添加 Checkbox 元素**
+ 在[**UI Editor**](/docs/user-guide/interactivity/user-interface/editor)中，选择 **New**, **Element from Slice Library**, **Checkbox**.

**编辑复选框组件**

在[**UI Editor**](/docs/user-guide/interactivity/user-interface/editor)的**Properties** 面板中，展开 **Checkbox** 并根据需要执行以下操作：

**Interactable**

见 [Properties](properties) 以编辑 Common Interactive Component 设置。

**Elements**, **On**

从列表中选择一个元素，以指定当复选框状态为`on`（已选中）时显示的实体。

**Elements**, **Off**

从列表中选择一个元素，以指定在复选框状态为`off`（清除）时显示的实体。

**Value, Checked**

选中该框以更改复选框的初始状态。

**Actions**, **Change**

输入文本字符串。当复选框有任何状态更改时，此字符串将作为 UI 画布上的操作发送。

**Actions**, **On**

输入文本字符串。当复选框状态更改为 `on` （选中） 时，此字符串将作为 UI 画布上的操作发送。

**Actions**, **Off**

输入文本字符串。当复选框状态更改为`off`（清除）时，此字符串将作为 UI 画布上的操作发送。
