---
linkTitle: UI Radio Button
description: ' Use a RadioButton component to make an element behave like a radio button in O3DE''s UI Editor. '
title: UI Radio Button Component
weight: 130
---

您可以使用 **RadioButton** 组件使元素的行为类似于单选按钮。此组件通常用于具有两个可视子元素的元素 - 一个在选择单选按钮时显示，另一个在清除单选按钮时显示。

将此组件与 **RadioButtonGroup** 组件结合使用。**RadioButtonGroup** 组件负责选择和清除组中的单选按钮，并确保只选择一个单选按钮。

![RadioButton element on canvas](/images/user-guide/interactivity/user-interface/components/interactive/ui-editor-components-radiobutton.png)

要查看带有 **RadioButton** 组件的完整画布的游戏内示例，请在项目 SamplesProject 中打开关卡 UiFeatures。按 **Ctrl+G** 玩游戏，然后选择 **Components**, **Interactable Components**, **RadioButton**。您可以查看单选按钮的不同行为、默认设置和组的示例按 **Esc** 退出游戏。

要在 UI 编辑器 中查看这些相同的画布，请导航到`\Gems\LyShineExamples\Assets\UI\Canvases\LyShineExamples\Comp\RadioButton` 目录。您可以打开以下画布：
+ `Groups.uicanvas` - 不同单选按钮分组的示例
+ `RadioButton.uicanvas` - 不同行为和默认设置的示例

**To edit a radio button component**

在[**UI Editor**](/docs/user-guide/interactivity/user-interface/editor)的 **Properties** 面板中，展开 **RadioButton** 并根据需要执行以下操作：

**Interactable**

见 [Properties](properties) 以编辑 Common Interactive Component 设置。

**Elements**, **On**

从列表中选择一个元素，以指定在单选按钮状态为 on （selected） 时显示的实体。

**Elements**, **Off**

从列表中选择一个元素，以指定在单选按钮状态为 off （cleared） 时显示的实体。

**Elements**, **Group**

从列表中选择一个元素以指定单选按钮所属的组。

**Value**, **Checked**

选中该框以更改单选按钮的初始状态。

**Actions**, **Change**

输入文本字符串。当单选按钮有任何状态更改时，此字符串将作为 UI 画布上的操作发送。

**Actions**, **On**

输入文本字符串。当单选按钮状态更改为 on （selected） 时，此字符串将作为 UI 画布上的操作发送。

**Actions**, **Off**

输入文本字符串。当单选按钮状态更改为 off （cleared） 时，此字符串将作为 UI 画布上的操作发送。
