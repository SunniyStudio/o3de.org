---
linkTitle: UI Layout Column
description: ' 使用 Open 3D Engine UI Editor 中的布局列组件将子元素组织到一个列中。 '
title: UI Layout Column 组件
weight: 100
---

您可以使用 **LayoutColumn** 组件将子元素组织到一个列中。要使用此功能，请将 **LayoutColumn** 组件添加到元素，然后添加子元素。UI 系统根据您在组件属性中选择的顺序，将子元素从上到下或从下到上定位在列中。子元素可以包含纹理或图像、按钮、复选框、文本、列、行、网格等。

要查看具有 **Layout Column** 组件的完整画布的游戏内示例，请打开项目 SamplesProject 中的关卡 UiFeatures。按 **Ctrl+G** 玩游戏，然后选择 **Components**, **Layout Components**, **Layout Column***。您可以在列中查看不同子项大小的示例。按 **Esc** 退出游戏。

要在 UI 编辑器中查看此同一画布，请导航到`\Gems\LyShineExamples\Assets\UI\Canvases\LyShineExamples\Comp\Layout` 目录并打开`SimpleColumn.uicanvas` 文件。

您可以从切片库添加预构建的 **Layout Column** 元素。执行此操作时，将自动创建一个简单的布局列并将其嵌套在 **Hierarchy** 窗格中。

**从切片库添加 Layout Column 元素**
+ 在[**UI Editor**](/docs/user-guide/interactivity/user-interface/editor)中，选择 **New**, **Element from Slice Library**, **LayoutColumn**.

默认情况下，layout 列为每个子项提供相同的空间量，而不管其内容如何。但是，您可以通过将 [LayoutCell](./components-layout-cell) 组件添加到每个子项或特定子项来操纵每个子项的大小。

layout 列还可以根据其内容为每个子项提供不同的空间。要使布局列能够执行此操作，请清除 **LayoutColumn** 组件属性中的 **Ignore Default Cell** 选项。

在第一个图像中，选择了 **Ignore Default Cells**。layout 列为每个子项提供相同的空间量，而不管其内容如何。

![Layout column with equally sized children](/images/user-guide/interactivity/user-interface/components/layout/ui-editor-components-layout-column-ignore.png)

在第二个图像中，清除了 **Ignore Default Cells**。layout 列根据其子项的内容计算其子项的空间。

![Layout column with children sized to content](/images/user-guide/interactivity/user-interface/components/layout/ui-editor-components-layout-column-clear.png)

要查看带有布局列组件的完整画布的示例，请打开 `SimpleColumn.uicanvas`，位于LyShineExamples Gem \(`\dev\Gems\LyShineExamples\Assets\UI\Canvases\LyShineExamples\Comp\`\)中。

**编辑布局列组件**

在[**UI Editor**](/docs/user-guide/interactivity/user-interface/editor)的**Properties**中，展开 **LayoutColumn**并根据需要执行以下操作：

**Padding**

键入相对于元素边框的像素值。

**Spacing**

以像素为单位键入值以调整元素之间的间距。

**Order**

选择 **Top-to-Bottom** 或 **Bottom-to-Top** 以指定子元素在列中的显示顺序。

**Child Alignment**

如果 layout 的 children 不占用所有可用的 layout 空间，请使用此设置来确定 children 的对齐方式。

对于 **Horizontal**， 选择 **Left**, **Center**, 或 **Right** 来确定子项的水平对齐方式。

对于 **Vertical**， 选择 **Top**, **Center**, 或 **Bottom** 来确定子项如何垂直对齐。

**Ignore Default Cell**

默认情况下，此属性处于选中状态，它会导致布局列为每个子项提供相等的空间量，而不管其内容如何（除非该子项具有 [**LayoutCell**](./components-layout-cell) 组件）。布局列会忽略布局单元格中基于内容的默认计算。

清除此选项时，布局列将使用子项的布局单元格计算值来确定根据其内容为每个子项提供多少空间。有关详细信息，请参阅 [LayoutCell](./components-layout-cell).
