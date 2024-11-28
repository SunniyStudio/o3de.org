---
linkTitle: 搜索 UI 元素
description: ' 在 Open 3D Engine 的 UI 编辑器中搜索 UI 元素或其组件。 '
title: 搜索 UI 元素
weight: 300
---

您可以通过按名称或其附加组件进行筛选来查找 UI 元素。**查找元素** 工具按层次结构显示活动 UI 画布的 UI 元素。

![The UI Editor's Find Element tool.](/images/user-guide/interactivity/user-interface/elements/ui-editor-search-element-tool.png)

**打开 **Find Element** 工具**
+ 在 **UI 编辑器**中，执行以下操作之一：
  + 从菜单中，选择 **Edit** ，然后选择 **Find Elements**.
  + 按下 **Ctrl+F**.

  最初，当前活动的 UI 画布中的所有 UI 元素都显示在树视图中。

![To find UI elements by name, enter the name in the search box.](/images/user-guide/interactivity/user-interface/elements/ui-editor-search-element-name.png)

**按 UI 元素名称筛选**
+ 在搜索筛选条件框中输入名称。

  包含输入文本的 UI 元素名称仍然可见。所有其他 UI 元素都处于隐藏状态。

![To find UI elements by components, click the filter button and select a component.](/images/user-guide/interactivity/user-interface/elements/ui-editor-search-element-component.png)

**按组件筛选**

1. 选择搜索筛选条件框旁边的筛选条件按钮。

1. 在显示的菜单中，选择一个或多个元件。

    包含所选组件的 UI 元素可见。所有其他 UI 元素都处于隐藏状态。
    
    {{< note >}}
**Find Element**工具仅显示活动 UI 画布中的 UI 元素使用的组件以供选择。
{{< /note >}}

与搜索条件不匹配的 UI 元素通常会被隐藏。但是，如果与搜索条件不匹配的父元素之一满足搜索条件，则该父元素仍然可见。这将保留用于显示 UI 元素的树结构。显示的满足搜索条件的 UI 元素显示为白色文本。显示的 UI 元素不满足条件，但可见以保持树结构，以灰色文本显示。

![The parent of a child element that meets search criteria appears in gray text.](/images/user-guide/interactivity/user-interface/elements/ui-editor-search-element-gray.png)

选择一个或多个 UI 元素后，您可以选择在 **UI 编辑器** 层次结构中选择它们。

**将选区传输到 **Hiearchy** 面板**

1. 在**Find Elements**工具中，选择一个或更多UI元素。按住 **Ctrl** 选择多个元素。

1. 选择 **Select in Hiearchy**.

   **Hierarchy** 面板，并选中相同的 UI 元素。

   ![Choose Select in Hierarchy to transfer your selections to the Hierarchy panel.](/images/user-guide/interactivity/user-interface/elements/ui-editor-search-element-hierarchy.png)
