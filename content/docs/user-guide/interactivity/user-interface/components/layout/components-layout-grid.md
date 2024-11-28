---
linkTitle: UI Layout Grid
description: ' 使用 Open 3D Engine UI Editor 中的布局网格组件将子元素组织到一个统一的网格中。'
title: UI Layout Grid 组件
weight: 300
---

您可以使用布局网格组件将子元素组织到一个统一的网格中。要使用此功能，您需要将布局网格组件添加到元素中，然后添加子元素。UI 系统将子元素定位在网格模式中。您可以选择子元素是按从左到右还是从右到左、从下到上还是从上到下放置。子元素可以包含纹理或图像、一段文本、一个按钮、一个复选框、更多列、行、网格等。每个子项的大小由 **Cell Size** 属性确定，并且独立于每个子项的内容。

![Example canvas with LayoutGrid](/images/user-guide/interactivity/user-interface/components/layout/ui-editor-components-layout-grid.png)



要查看具有 **LayoutGrid** 组件的完整画布的游戏内示例，请在项目 SamplesProject 中打开关卡 UiFeatures。按 **Ctrl+G** 玩游戏，然后选择 **Components**, **Layout Components**, **Layout Grid**。 您可以查看不同填充图案的示例。按 **Esc** 退出游戏。

要在 UI 编辑器中查看此同一画布，请导航到`\Gems\LyShineExamples\Assets\UI\Canvases\LyShineExamples\Comp\Layout `目录并打开`SimpleGrid.uicanvas`文件。

您可以从切片库添加预构建的 **Layout Grid** 元素。执行此操作时，将自动创建一个简单的布局网格并将其嵌套在 **Hierarchy** 窗格中。

**从切片库添加 Layout Grid 元素**
+ 在[**UI Editor**](/docs/user-guide/interactivity/user-interface/editor)中，选择**New**, **Element from Slice Library**, **LayoutGrid**.

**编辑布局网格组件**

在[**UI Editor**](/docs/user-guide/interactivity/user-interface/editor)的**Properties**面板中，展开 **LayoutGrid** 并根据需要执行以下操作：

**Padding**

键入相对于元素边框的像素值。

**Spacing**

以像素为单位键入值以调整元素之间的间距。

**Cell size**

键入以像素为单位的值以指定子元素的大小。

**Order**

根据需要执行以下操作：
+  对于 **Horizontal**, 选择 **Left-to-Right** 或 **Right-to-Left** 确定元素水平显示的顺序。
+ 对于 **Vertical**, 选择 **Top-to-Bottom** 或 **Bottom-to-Top** 确定元素垂直显示的顺序。
+ 对于 **Starting With**, 选择 **Horizontal** 或 **Vertical** 来确定元素是先水平显示还是垂直显示。

**Child Alignment**
如果布局的子项不占用所有可用的布局空间，则此设置将确定子项的对齐方式。

对于 **Horizontal**, 选择 **Left**, **Center**, 或 **Right** 来确定子项的水平对齐方式。

对于 **Vertical**, 选择 **Top**, **Center**, 或 **Bottom** 来确定子项如何垂直对齐。
