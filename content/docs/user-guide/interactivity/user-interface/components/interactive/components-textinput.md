---
linkTitle: UI Text Input
description: ' 使用Text Input组件使元素在 O3DE 的 UI 编辑器中提供玩家输入。 '
title: UI Text Input 组件
weight: 160
---

您可以使用 **TextInput** 组件使元素提供玩家输入。此组件通常应用于具有一个图像组件和两个具有文本组件的子元素（一个用于占位符文本，一个用于输入文本）。

![Canvas with TextInput element](/images/user-guide/interactivity/user-interface/components/interactive/ui-editor-components-textinput.png)

要查看具有 **TextInput** 组件的完整画布的游戏内示例，请打开项目 SamplesProject 中的关卡 UiFeatures。按 Ctrl+G** 玩游戏，然后选择 **Components**, **Interactable Components**, **TextInput**。您可以在单行和多行上查看不同类型的文本输入行为的示例。按 **Esc** 退出游戏。

要在 UI 编辑器 中查看这些相同的画布，请导航到 `\Gems\LyShineExamples\Assets\UI\Canvases\LyShineExamples\Comp\TextInput` 目录。您可以打开以下画布：
+ `Multiline.uicanvas` - 编辑多行文本字符串的示例
+ `SingleLine.uicanvas` - 编辑单行文本字符串的示例

您可以从切片库添加预构建的 **TextInput** 元素。执行此操作时，将出现文本输入框、暂停状态和占位符文本“在此处键入...”在您的 **Hierarchy** 窗格中自动创建。

**从切片库添加 TextInput 元素**
+ 在 [**UI Editor**](/docs/user-guide/interactivity/user-interface/editor)中，选择 **New**, **Element from Slice Library**, **TextInput**.

**编辑文本输入组件**

在[**UI Editor**](/docs/user-guide/interactivity/user-interface/editor)的**Properties**中，展开 **TextInput** 并根据需要执行以下操作：

**Interactable**

见 [Properties](properties) 以编辑 Common Interactive Component 设置。

**Elements**, **Text**

从列表中选择一个元素，为输入文本提供文本组件。该列表显示具有文本组件的子元素。

**Elements**, **Placeholder text element**

从列表中选择一个元素，为占位符文本提供文本组件。该列表显示具有文本组件的子元素。

**Text editing**, **Selection color**

单击色板可为所选文本选择其他颜色。

**Text editing**, **Cursor color**

单击色样为光标选择其他颜色。

**Text editing**, **Max char count**

输入文本输入框中允许的最大字符数。键入 '`-1`' 表示无字符限制。

**Text editing**, **Cursor blink time**

输入一个值（以秒为单位）。使用“`0`”表示不闪烁，“`1`”表示每秒闪烁一次，“`2`”表示每两秒闪烁一次，以此类推。

**Text editing**, **Is password field**

选中该框并指定替换字符。

**Text editing**, **Clip input text**

在运行时将文本元素的 **Overflow mode** 设置为 **Clip text**。

**Actions**, **Change**

输入文本字符串。每当文本输入发生更改（例如键入或删除字符）时，此字符串都会作为 UI 画布上的操作发送。

**Actions**, **End edit**

输入文本字符串。每当玩家单击文本输入或按 **Enter** 时，此字符串将作为 UI 画布上的操作发送。

**Actions**, **Enter**

输入文本字符串。当玩家按 **Enter** 时，此字符串将作为 UI 画布上的操作发送。
