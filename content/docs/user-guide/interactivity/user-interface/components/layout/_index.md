---
linkTitle: 布局组件
description: ' 使用 O3DE 的 UI 编辑器中的布局组件将子元素组织成列、行或网格。 '
title: 布局组件
weight: 500
---

布局组件定义游戏界面的排列方式。UI 系统具有四个布局组件来组织您的元素：**LayoutColumn**、**LayoutRow**、**LayoutGrid** 和 **LayoutCell**。您还可以嵌套布局组件。

## 章节主题

| 主题 | 说明 |
|---|---|
| [UI Layout Column](components-layout-column) | 将 **LayoutColumn** 组件添加到元素中，使其成为布局列。当您将子元素添加到布局列时，布局列会为每个子元素分配一个布局单元格。布局列根据您添加的子元素数量以及子元素的布局单元格提供的值来调整布局单元格的大小。 |
| [UI Layout Row](components-layout-row) | 将 **LayoutRow** 组件添加到元素中，使其成为布局行。与布局列一样，布局行为每个子元素分配一个布局单元格。布局行根据您添加的子元素数量以及子元素的布局单元格提供的值来调整布局单元格的大小。 |
| [UI Layout Grid](components-layout-grid) | 将 **LayoutGrid** 组件添加到元素中，使其成为布局网格。布局网格将子元素放入网格中。但是，与布局行和布局列不同，布局网格不使用布局单元格。**LayoutGrid** 组件的属性决定了其子组件的大小。 |
| [UI Layout Cell](components-layout-cell) | 将 **LayoutCell** 组件添加到布局行或布局列的子项中，以自定义布局单元格大小的确定方式。布局单元是一个编程概念，其属性定义子元素的区域。每当将子元素添加到布局行或布局列时，该子元素都会接收布局单元格属性（在 **UI 编辑器**中不可见），这些属性将决定子元素的空间大小。您可以通过将 **LayoutCell** 组件添加到子组件来覆盖布局单元格的计算属性。 |
| [UI Layout Fitter](components-layout-fitter) |将 **LayoutFitter** 组件添加到元素中，使元素调整自身大小以适应其内容。将布局拟合器组件与提供单元格大小信息的其他组件结合使用，例如文本、图像（ImageType 设置为 Fixed）或布局组件（单元格、行、列、网格）。 |
| [UI Dynamic Layout](dynamic-layout) | 使用 **Dynamic Layout** 组件，您可以在运行时更改布局元素的子元素数。 |
| [嵌套布局组件](components-layout-nesting) | 了解如何嵌套布局组件。 |
