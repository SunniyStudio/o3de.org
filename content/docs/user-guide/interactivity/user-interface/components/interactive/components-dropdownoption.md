---
linkTitle: UI Dropdown Option
description: ' 使用 Dropdown Option 组件，将元素设为 Open 3D Engine UI Editor 下拉菜单中的选项。 '
title: UI Dropdown Option 组件
weight: 215
---

您可以使用 **DropdownOption** 组件将元素设置为下拉菜单中的选项。使用 **DropdownOption** 组件时，请注意以下事项：
+ **DropdownOption** 与元素上的 **Dropdown** 组件一起使用。
+ **DropdownOption** 需要一个交互式组件，通常是 **RadioButton** 组件，以便任何时候在下拉菜单中都只能选择一个选项。
+ 其子元素通常包含 **Text** 组件，该组件在选择该选项时显示。

要查看带有 **DropdownOption** 组件的完整画布的游戏内示例，请在项目 SamplesProject 中打开关卡 UiFeatures。按 **Ctrl+G** 玩游戏，然后选择 **Components**, **Interactable Components**, **Dropdown**, **Using UI Components and Dropdowns**。按 **Esc** 退出游戏。

要在 UI 编辑器中查看此同一画布，请导航到`\Gems\LyShineExamples\Assets\UI\Canvases\LyShineExamples\Comp\Dropdown`目录，打开`UsingUi.uicanvas`文件。此画布具有一个下拉菜单，其中包含滚动框、图像、复选框、滑块和单选按钮。

**编辑 DropdownOption 组件**

在[**UI Editor**](/docs/user-guide/interactivity/user-interface/editor)的 **Properties**面板中，展开 DropdownOption，并根据需要执行以下操作：

**Interactable**

见 [Properties](properties) 以编辑 Common Interactive Component 设置。

**Elements, Owning Dropdown**

从列表中选择具有 **Dropdown** 组件的元素。这是选择此选项时要修改的元素。

**Elements, Text Element**

从列表中选择一个元素，该元素显示与此选项对应的文本。选择此选项后，此文本将显示在拥有的下拉菜单中（只要下拉菜单配置了 **Text Element**）。

**Elements, Icon Element**

从列表中选择一个元素，该元素显示与此选项对应的图标。选择此选项后，此图标会显示在拥有下拉菜单上（只要下拉菜单配置了 **Icon Element**）。
