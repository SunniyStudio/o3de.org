---
linkTitle: UI Layout Row
description: ' 使用 Open 3D Engine UI Editor 中的布局行组件将子元素组织到一行中。'
title: UI Layout Row 组件
weight: 200
---

您可以使用 **LayoutRow** 组件将子元素组织成一行。要使用此功能，请将 **LayoutRow** 组件添加到元素中，然后添加子元素。UI 系统根据您选择的顺序，将子元素从左到右或从右到左放置在行中。子元素可以包含纹理或图像、一段文本、一个按钮、一个复选框、更多列、行、网格等。要控制特定或所有子项的大小，请将 [LayoutCell](./components-layout-cell)  组件添加到这些子项中。

与 **LayoutColumn** 组件类似，**LayoutRow** 组件具有 **Ignore Default Cell** 属性。有关更多信息，请参阅[LayoutColumn](./components-layout-column).

![LayoutRow with children](/images/user-guide/interactivity/user-interface/components/layout/ui-editor-components-layout-row.png)

要查看具有 **Layout Row** 组件的完整画布的游戏内示例，请打开项目 SamplesProject 中的关卡 UiFeatures。按 **Ctrl+G** 玩游戏，然后选择**Components**, **Layout Components**, **Layout Row**。您可以在一行中查看不同子项大小的示例。按 **Esc** 退出游戏。

要在 UI 编辑器中查看此同一画布，请导航到`\Gems\LyShineExamples\Assets\UI\Canvases\LyShineExamples\Comp\Layout` 目录并打开 `SimpleRow.uicanvas` 文件。

您可以从切片库添加预构建的 **Layout Row** 元素。执行此操作时，将自动创建一个简单的布局行并将其嵌套在 **Hierarchy** 窗格中。

**从切片库添加 Layout Row 元素**
+ 在 [**UI Editor**](/docs/user-guide/interactivity/user-interface/editor)中，选择 **New**, **Element from Slice Library**, **LayoutRow**.

**编辑布局行组件**

在[**UI Editor**](/docs/user-guide/interactivity/user-interface/editor)的**Properties**面板中，展开 **LayoutRow** 并根据需要执行以下操作：

**Padding**

键入相对于元素边框的像素值。

**Spacing**

以像素为单位键入值以调整元素之间的间距。

**Order**

选择 **Left-to-Right** 或 **Right-to-Left** 以指定子元素在行中的显示顺序。

**Child Alignment**

如果 layout 的 children 不占用所有可用的 layout 空间，请使用此设置来确定 children 的对齐方式。

对于 **Horizontal**, 选择 **Left**, **Center**, 或 **Right** 来确定子项的水平对齐方式。

对于 **Vertical**, 选择 **Top**, **Center**, 或 **Bottom** 来确定子项如何垂直对齐。

**Ignore Default Cell**

默认情况下，此属性会使布局行为每个子项提供相等的空间量，而不管其内容如何（除非子项具有 [**LayoutCell**](./components-layout-cell) 组件）。布局行将忽略布局单元格的基于内容的默认计算。

清除此选项时，布局行将使用子项的布局单元格计算值来确定根据其内容为每个子项提供多少空间。有关详细信息，请参阅 [LayoutCell](./components-layout-cell).
