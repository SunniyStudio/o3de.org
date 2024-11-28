---
linkTitle: UI Layout Cell
description: ' 在 Open 3D Engine UI Editor 中，将布局单元格组件添加到布局行或列的子项，以指定子项的最小大小、目标大小或额外比率大小。 '
title: UI Layout Cell 组件
weight: 400
---

在处理单元格内容时，了解布局单元格和 **LayoutCell** 组件之间的区别非常重要。布局单元格表示一组值，这些值确定在布局行或列中分配给子项的空间或区域。另一方面，**LayoutCell** 组件控制布局单元格的大小。布局单元格存在于布局行或布局列的子项上，无论它是否具有 **LayoutCell** 组件。**LayoutCell** 组件只提供了一种操作和覆盖布局单元格的默认计算的方法。

布局单元格的属性包括最小大小、目标大小和额外大小比率。这些属性不能在 UI Editor 中直接修改，但可以通过多种方式确定：

+ 组件 - 以下组件可能会影响布局单元格大小：

  + 图像或文本 - 图像的默认大小是布局单元格的目标大小。文本组件中字符串的长度和大小是布局单元格的目标大小。
  + 布局行或布局列（添加或嵌套为子项） - 作为子项添加的布局行或布局列的默认值决定了布局单元格的最小大小和目标大小。默认值的计算方法是其自己的 children 之和加上 padding 和 spacing。

    {{< note >}}
**LayoutColumn** 和 **LayoutRow** 组件包含一个名为 **Ignore Default Cells** 的属性。选择此属性会导致忽略上述计算，并简单地为所有子项分配相等的空间，而不考虑内容。清除此属性可按组件计算布局单元格值。有关详细信息，请参阅 [LayoutColumn](/docs/user-guide/interactivity/user-interface/components/layout/components-layout-column).
{{< /note >}}

+ 固定默认布局单元格值 - 如果子项没有任何计算自己的布局单元格值的组件，则会为布局单元格分配最小和目标大小 0 以及额外大小比率 1。这通常意味着没有影响布局单元格大小的组件的子项的间距相等。每个布局单元格都以相同的速率增长以填充可用空间（因此额外大小比率为 1）。
+ **LayoutCell** component - 添加 **LayoutCell** 组件以指定最小大小和目标大小的值，以及额外大小比率。您在此处指定的任何值都会覆盖所有其他方法计算的值。

计算布局单元格值后，布局单元格空间将按以下方式分配：

1. 首先，每个子项都会收到其最小大小 (**Min Height** 或 **Min Width**).

1. 如果有可用空间，则每个子项都会收到其目标大小 (**Target Height** 或 **Target Width**).

1. 如果在此之后仍有可用空间，则使用 **Extra Size Ratio** 值来确定如何分配剩余空间。此比率是相对于孩子的兄弟姐妹的比率。例如，如果一个子项的额外大小比率为 1，另一个子项的 Extra Size Ratio 为 2，则第二个子项获得的额外空间是第一个子项的两倍。额外大小比率 0 意味着在达到目标大小后不再分配更多空间。

## 使用 LayoutCell 组件

您可以将 **LayoutCell** 组件应用于布局行或列的子项，以覆盖布局单元格的默认计算。

在以下示例中，layout 列将三个 images 作为其子项。每个图像在列中占据相等的空间。

![Hierarchy pane and canvas](/images/user-guide/interactivity/user-interface/components/layout/ui-editor-components-layout-cell.png)

如果将 **LayoutCell** 组件添加到第一个图像，然后选择 **Min Height** 并分配值 100，则 UI 系统将覆盖该子级的默认计算值，并为顶部图像提供比其同级图像更高的高度，后者的值将重新计算以调整到剩余的列空间。

![Setting Min Height in LayoutCell](/images/user-guide/interactivity/user-interface/components/layout/ui-editor-components-layout-cell-2.png)

在下一个示例中，将布局网格添加为子项。它计算出的大小与它上面的两个同级相同。

![LayoutGrid added to canvas](/images/user-guide/interactivity/user-interface/components/layout/ui-editor-components-layout-cell-3.png)

但是，当您将 **LayoutCell** 组件添加到网格中，然后将 **Min Height** 指定为 100 时，将向整个网格授予该空间量。但是，如果将 **LayoutCell** 组件添加到布局网格的子项中，则它不会产生任何效果。这是因为各个网格空间始终是统一的，并且由网格父级控制。

![LayoutCell added to LayoutGrid](/images/user-guide/interactivity/user-interface/components/layout/ui-editor-components-layout-cell-4.png)

**To edit a LayoutCell component编辑 LayoutCell 组件**

在[**UI Editor**](/docs/user-guide/interactivity/user-interface/editor)的 **Properties** 面板中，展开 **LayoutCell** 并根据需要执行以下操作：

**Min Width**

选择以定义布局单元格的最小宽度。在显示的框中键入一个值。

**Min Height**

选择以定义布局单元格的最小高度。在显示的框中键入一个值。

**Target Width**

选择以定义布局单元格的目标宽度。在显示的框中键入一个值。如果有可用空间，则此目标宽度将分配给布局单元格。

**Target Height**

选择以定义目标高度。在显示的框中键入一个值。如果有可用空间，则此目标高度将分配给布局单元格。

**Extra Width Ratio**

选择以定义布局单元格的额外宽度比例。在显示的框中键入一个值。此值是相对于其他元素的比率。如果在布局单元格达到其目标大小后仍有空间，则 **Extra Size Ratio** 值用于分配其余空间。

由于此 **Extra Size Ratio** 值是相对于其他子项的，因此，如果一个子项的 Extra Size Ratio 为 1，而另一个子项的 Extra Size Ratio 为 2，则第二个子项获得的额外空间是第一个子项的两倍。额外大小比率 0 意味着在达到目标大小后不再分配更多空间。

**Extra Height Ratio**
选择以定义布局单元格的额外高度比率。在显示的框中键入一个值。
