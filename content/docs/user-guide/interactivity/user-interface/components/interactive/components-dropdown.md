---
linkTitle: UI Dropdown
description: ' 使用 Dropdown 组件使元素的行为类似于 Open 3D Engine UI Editor 中的下拉菜单。 '
title: UI Dropdown 组件
weight: 210
---

您可以使用 **Dropdown** 组件使元素的行为类似于下拉菜单。将此组件与包含内容的子元素一起使用。子元素提供下拉菜单的内容。

您可以将 **Dropdown** 组件与 **DropdownOption** 组件一起使用。使用 **DropdownOption** 组件，您可以配置选项，以便在选择菜单文本后更改菜单文本及其图标。

要查看带有 **Dropdown** 组件的完整画布的游戏内示例，请打开项目 SamplesProject 中的关卡 UiFeatures。按 **Ctrl+G** 玩游戏，然后选择 **Components**、**Interactable Components**、**Dropdown**。您可以查看**Simple Dropdowns**，**Nested Dropdowns**，**Multiple Select & Functionalit**，以及**Using UI Components and Dropdowns**。按 **Esc** 退出游戏。

要在 UI 编辑器 中查看这些相同的画布，请导航到`\Gems\LyShineExamples\Assets\UI\Canvases\LyShineExamples\Comp\Dropdown`目录。您可以打开以下画布：
+ `MultipleFunc.uicanvas` - Multiple selection 下拉菜单和功能下拉菜单（对球执行操作，例如创建、销毁、移动和更改颜色）
+ `Nested.uicanvas` - 两级和多级（带同级）子菜单
+ `Simple.uicanvas` - 简单选择下拉菜单、带图标的选择下拉菜单、带图标的选区下拉菜单、带图标的暂停时展开
+ `UsingUi.uicanvas` - 带有滚动框、图像、复选框、滑块和单选按钮的下拉菜单

您可以从切片库添加预构建的 **Dropdown** 元素。执行此操作时，将在 **Hierarchy** 窗格中自动创建一个下拉菜单、三个选项及其图像元素。

![Dropdown slice on canvas](/images/user-guide/interactivity/user-interface/components/interactive/ui-editor-components-interactive-dropdown-slice.png)

**从切片库添加 Dropdown 元素**
+ 在 [**UI Editor**](/docs/user-guide/interactivity/user-interface/editor)中，选择 **New**, **Element from Slice Library**, **Dropdown**.

**编辑 Dropdown 组件**

在[**UI Editor**](/docs/user-guide/interactivity/user-interface/editor)的 **Properties** 面板中，展开 **Dropdown** 并根据需要执行以下操作：

**Interactable**

见 [Properties](properties)以编辑 Common Interactive Component 设置。

**Elements, Content**

从列表中选择一个元素。这指定了展开下拉菜单时显示的实体。

**Elements, Expanded Parent**

将元素拖到 **Expanded Parent** 框上。这指定了在下拉菜单展开时用作父级的实体。这用于图层下拉菜单。

**Elements, Text Element**

从列表中选择一个元素，以指定实体以显示与所选选项对应的文本。

**Elements, Icon Element**

从列表中选择一个元素，以指定要显示与所选选项相对应的图标的实体。

**Options, Expand on Hover**

选择以在暂停时启用下拉行为，并在退出时折叠。

**Options, Wait Time**

输入下拉菜单在暂停时展开或退出时折叠之前等待的秒数。

**Options, Collapse on Outside Click**

选择以启用在菜单外部单击时折叠下拉菜单。

**Dropdown States, Expanded**

点击 加 在展开时将状态添加到下拉菜单中。

**Actions, Expanded**

输入一个文本字符串，当下拉菜单展开时，该字符串将作为操作在 UI 画布上发送。

**Actions, Collapsed**

输入一个文本字符串，当下拉菜单折叠时，要作为操作在 UI 画布上发送。

**Actions, Option Selected**

输入要在选择下拉选项时作为操作在 UI 画布上发送的文本字符串。
