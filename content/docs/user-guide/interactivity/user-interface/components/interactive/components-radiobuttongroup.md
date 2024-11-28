---
linkTitle: UI Radio Button Group
description: ' 使用 Radio Button Group 组件在 O3DE 的 UI 编辑器中管理单选按钮组。 '
title: UI Radio Button Group 组件
weight: 140
---

您可以使用 **RadioButtonGroup** 组件来管理单选按钮。此组件在适当时处理选择和清除组中的单选按钮。它还确保一次只选择一个单选按钮。通常在具有 children 单选按钮的元素上使用此组件，这些单选按钮是单选按钮组的一部分。

![RadioButtonGroup element on canvas](/images/user-guide/interactivity/user-interface/components/interactive/ui-editor-components-radiobuttongroup.png)

要查看具有 **RadioButtonGroup** 组件的完整画布的游戏内示例，请打开项目 SamplesProject 中的关卡 UiFeatures。按 **Ctrl+G** 玩游戏，然后选择 **Components**, **Interactable Components**, **RadioButton**。您可以查看单选按钮的不同行为、默认设置和组的示例按 **Esc** 退出游戏。

要在 UI 编辑器 中查看这些相同的画布，请导航到`\Gems\LyShineExamples\Assets\UI\Canvases\LyShineExamples\Comp\RadioButton`目录。您可以打开以下画布：
+ `Groups.uicanvas` - 不同单选按钮分组的示例
+ `RadioButton.uicanvas` - 不同行为和默认设置的示例

您可以从切片库添加预构建的 **RadioButtonGroup** 元素。执行此操作时，将在 **Hierarchy** 窗格中自动创建一组三个单选按钮。

**从切片库添加 RadioButtonGroup 元素**
+ 在[**UI Editor**](/docs/user-guide/interactivity/user-interface/editor)中，选择 **New**, **Element from Slice Library**, **RadioButtonGroup**.

**To edit a RadioButtonGroup component**

在[**UI Editor**](/docs/user-guide/interactivity/user-interface/editor)的 **Properties** 面板中，展开 **RadioButtonGroup** 并根据需要执行以下操作：

**Settings**, **Allow uncheck**

选择以启用清除或取消选中选定的单选按钮。

**Actions**, **Change**

输入文本字符串。当单选按钮组有任何状态更改时，此字符串将作为 UI 画布上的操作发送。
