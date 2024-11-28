---
linkTitle: 嵌套布局组件
description: ' 在 Open 3D Engine UI Editor 中将布局组件嵌套在其他布局组件中。 '
title: 嵌套布局组件
weight: 600
---

您可以将布局组件嵌套在其他布局组件中。

要查看具有嵌套布局的完整画布的游戏内示例，请打开项目 SamplesProject 中的关卡 UiFeatures。按 **Ctrl+G** 玩游戏，然后选择 **Components**, **Layout Components**, **Nested Layout**。按 **Esc** 退出游戏。

要在 UI 编辑器 中查看同一画布，请导航到`\Gems\LyShineExamples\Assets\UI\Canvases\LyShineExamples\Comp\Layout` 目录并打开 `\NestedLayout.uicanvas`.

以下示例显示了一个大型布局行。

![Hierarchy pane with LayoutRow and children](/images/user-guide/interactivity/user-interface/components/layout/ui-editor-components-nesting-row.png)

布局行中有四列。

![LayoutColumns within LayoutRow](/images/user-guide/interactivity/user-interface/components/layout/ui-editor-components-nesting-column.png)

在 A 列中，有 3 个布局行。列 B 有两个布局网格。列 C 有 3 张图像。列 D 有一个由颜色组成的大图像。

![Child elements of the LayoutColumns](/images/user-guide/interactivity/user-interface/components/layout/ui-editor-components-nesting-nested.png)

C 列中的第一个图像具有最小高度设置为 120 的布局单元格组件。这为它提供了比它下面的两个兄弟更大的空间，这两个兄弟姐妹没有 **LayoutCell**l 组件。布局列 D 还有一个 **LayoutCell** 组件，最小宽度为 110，这比其他三列（没有 **LayoutCell** 组件）提供了更多的空间。

![Child elements with LayoutCells](/images/user-guide/interactivity/user-interface/components/layout/ui-editor-components-nesting-cell.png)
