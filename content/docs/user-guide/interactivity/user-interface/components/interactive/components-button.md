---
linkTitle: UI Button
description: ' 使用按钮组件使元素的行为类似于 O3DE UI 编辑器中的按钮。 '
title: UI Button 组件
weight: 100
---

您可以使用 **Button** 组件使元素的行为类似于按钮。

![UI Editor Button component](/images/user-guide/interactivity/user-interface/components/interactive/ui-editor-components-button.png)

要查看带有 **Button** 组件的完整画布的游戏内示例，请在项目 SamplesProject 中打开关卡 UiFeatures。按 Ctrl+G 玩游戏，然后选择 **Components**、**Interactable Components**、**Button**。您可以查看可以创建的不同类型的按钮。按 **Esc** 退出游戏。

要在 UI 编辑器中查看此同一画布，请打开`\Gems\LyShineExamples\Assets\UI\Canvases\LyShineExamples\Comp\Button\Styles.uicanvas`.

请注意以下事项：
+ 此组件通常应用于具有图像组件的元素;如果不存在 Visual 或 Image 组件，则 Button 的许多属性不起作用。

+ 如果要向按钮添加文本标签，请添加带有文本组件的子元素。

+ 要定义切片图像类型的边框，请打开 **Sprite Editor**。为此，请单击箭头 （open-in） ![Open In Icon](/images/user-guide/interactivity/user-interface/editor/sprite-editor/ui-editor-components-button-1.png) 的 **Sprite Path**。

您可以从切片库添加预构建的 **Button** 元素。执行此操作时，将在 **Hierarchy** 窗格中自动创建一个带有文本字符串 “Button” 的基本按钮。

**从切片库添加 Button 元素**
+ 在 [**UI 编辑器**](/docs/user-guide/interactivity/user-interface/editor)中，选择 **New**, **Element from Slice Library**, **Button**.

**编辑按钮组件**

在 [**UI Editor**](/docs/user-guide/interactivity/user-interface/editor)的 **Properties** 面板中，展开 **Button**并根据需要执行以下操作：

****Interactable****

见 [Properties](properties)以编辑 Common Interactive Component 设置。

****Actions**, **Click****

输入文本字符串。单击按钮时，此字符串将作为 UI 画布上的操作发送。
