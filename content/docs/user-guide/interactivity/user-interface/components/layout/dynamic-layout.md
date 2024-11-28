---
linkTitle: UI Dynamic Layout
description: 将动态布局组件与布局组件一起使用，可以在 Open 3D Engine （O3DE） 中的游戏 UI 中显示动态内容。
title: UI Dynamic Layout 组件
weight: 550
---

使用 **DynamicLayout** 组件，您可以在运行时更改布局元素的子元素数。要使用 **DynamicLayout** 组件，请将其放置在一个元素上，该元素也具有[**LayoutColumn**](components-layout-column), [**LayoutRow**](components-layout-row), 或 [**LayoutGrid**](components-layout-grid) 组件。

布局元素 （1） 动态调整大小以适应其子元素。布局元素的第一个子元素 （2） 充当 prototype 元素。在运行时，UI 系统会克隆 prototype 元素，以在布局中实现指定数量的子元素。

![Layout element and child in Hierarcy pane](/images/user-guide/interactivity/user-interface/components/ui-editor-components-dynamic-child.png)

布局元素的自动大小调整取决于布局类型。

对于 [**LayoutColumn**](components-layout-column) 和 [**LayoutRow**](components-layout-row) 元素中，布局元素会调整大小，以使所有子元素的大小与 prototype 元素相同。

对于 [**LayoutGrid**](components-layout-grid) 元素，**LayoutGrid** 组件的单元格大小决定了子元素的大小。**LayoutGrid** 元素的初始大小决定了每行或每列可以容纳的子项的数量，具体取决于填充方向或 **Order** 设置。如果 **Starting with** 填充方向为 **Horizontal**，则 UI 系统使用 **LayoutGrid** 元素的初始宽度来确定每行可容纳多少个子项。如果设置为 **vertical**，则初始高度用于确定每列可容纳多少个子项。

![Order setting 'Starting with' set to 'Horizontal'](/images/user-guide/interactivity/user-interface/components/ui-editor-components-dynamic-fillorder.png)

**使用动态布局组件**

1. 在 [**UI Editor**](/docs/user-guide/interactivity/user-interface/editor)中，添加一个 **LayoutRow**, **LayoutColumn**, 或 **LayoutGrid** prefab。为此，选择 **New**, **Element from Prefab**。然后选择其中一个布局元素。

   这用作保存动态内容的结构或框架。

1. 添加一个 **DynamicLayout** 组件到布局组件。为此，在 **Properties** 面板中，选择 **Add Component**, **DynamicLayout**.

   在 **Num Cloned Elements**中，输入要创建的子项的初始数量。

1. 创建具有 **Image** 组件的子实体。为此，请右键单击 **Hierarchy** 窗格中的布局组件，然后选择 **New、**Element from Prefab**、**Image**。

   此图像用作原型元素，该元素将被克隆并填充动态内容。
