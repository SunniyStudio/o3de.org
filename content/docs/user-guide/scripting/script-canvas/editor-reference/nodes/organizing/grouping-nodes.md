---
linktitle: 分组节点
title: 在 Script Canvas 中对节点进行分组
description: 对 Script Canvas 节点进行分组，以提高 Open 3D Engine （O3DE） 中图形的可管理性。
weight: 300
---

随着 Script Canvas 图形大小的增加，您可以对节点进行分组，以逻辑方式组织脚本的各个部分或降低其视觉复杂性。可以对组进行嵌套、命名和颜色编码。

## 创建和管理节点组

从现有节点和/或节点组创建节点组

1. 选择要分组的节点。

1. 右键单击图表，选择 **Group**（组），然后选择一个组类别。

1. 输入节点组的名称，然后按 Enter。

**要创建一个空组**
+ 右键单击图表的空白部分，选择 **Group**（组）**，然后选择一个组类别。您可以展开组的边界并向其添加节点。

折叠组
+ 执行以下操作之一：
  + 双击该组。
  + 右键单击组标题栏，然后选择 **Collapse**。
  + 在 **Node Inspector**中，打开 **Collapse Group** 设置。

折叠的组具有带有虚线的边框。

展开折叠的组
+ 执行以下操作之一：
  + 双击该组。
  + 右键单击组，然后选择 **Expend**。
  + 在 **Node Inspector**中，关闭 **Collapse Group** 设置。

![Creating a node group in the Script Canvas Editor.](/images/user-guide/scripting/script-canvas/nodes-grouping-expand.gif)

**编辑组的名称**
+ 执行以下操作之一：
  + 右键单击展开的组的标题栏，然后选择 **Edit group title**.
  + 左键单击组的标题栏。在 **Node Inspector**中，更改 **Group Name** 设置中的名称。

**更改组的颜色**
+ 执行以下操作之一：
  + 右键单击展开组的标题栏，然后从 **Apply Preset** 子菜单中选择一种新颜色。
  + 左键单击组的标题栏。在 **Node Inspector **中，更改 **Group Color** 设置中的 RGB 值，或使用该设置的 **Color Picker** 选择自定义颜色，然后选择 **OK**。

调整组的边框大小以适合组的内容

+ 要减少组内的空白水平空间，请双击组的左边框或右边框。

+ 要减少组内的空白垂直空间，请双击组的顶部或底部边框。

取消组分组
+ 右键单击组的标题栏，然后选择 **Ungroup**.

删除组及其所有节点
+ 执行以下操作之一：
  + 右键单击组的标题栏，然后选择 **Delete**。
  + 左键单击组的标题栏，然后按 **Delete**。

    {{< note >}}
删除组将删除组中的所有节点及其与组外其他节点的连接。如果要删除组容器但保留其节点和连接，请选择**Ungroup**.
    {{< /note >}}

### 启用组作为书签

要快速浏览图表中的组，您可以将组启用为书签。

**为群组添加书签**

1. 选择组。

1. 在 **Node Inspector**中，选择 **Enable as Bookmark**.

有关使用书签的更多信息，请参阅 [添加书签](./adding-bookmarks/).

## 自定义组

您可以使用 **Node Inspector** 自定义组的颜色并更改组标题的字体设置。

**更改节点组的颜色**

1. 选择节点组。

1. 在 **Node Inspector **中，执行以下操作之一：
   + 如果您知道要使用的 RGB 值，请在 **Group Color** 文本框中输入这些值。
   + 单击 **Group Color** 图标以使用 **Select Color** 对话框。

      ![Click the Group Color icon in the Script Canvas Node Inspector to customize the color of a node group.](/images/user-guide/scripting/script-canvas/nodes-grouping-color.png)

      + 在 **Select Color** 对话框中，指定要使用的颜色。您可以从基本颜色中进行选择，创建自定义颜色，或单击 **Pick Screen Color** 以使用指针在屏幕上选择颜色。

        ![Choose or create a color for a group node in the Script Canvas Editor.](/images/user-guide/scripting/script-canvas/nodes-grouping-color-select.png)

      + 点击 **OK**.

**更改节点组标题的字体设置**

1. 在 **Node Inspector** 中，展开 **Font Settings**。

1. 输入或选择要用于组标题字体的值。您所做的更改会立即可见。

    ![Expand a node group's Font Settings section in the Script Canvas Node Inspector.](/images/user-guide/scripting/script-canvas/nodes-grouping-font-settings.png)
