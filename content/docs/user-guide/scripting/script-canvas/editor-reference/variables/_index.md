---
linktitle: 变量
title: Script Canvas 变量和变量管理器
description: 使用 Open 3D Engine （O3DE） Script Canvas 编辑器中的变量管理器添加或管理变量，并在 Script Canvas 图形中创建获取、设置或更改值的事件节点。
weight: 300
---

**变量管理器** 显示 Script Canvas 图表中使用的变量。这些变量表示构建游戏逻辑所需的自定义数据。例如，您可以使用变量创建计数器、存储实体引用、指定方向或定义颜色。

## 添加和配置变量

您可以将变量添加到 Script Canvas 图形中，以声明和初始化它们。

**添加和配置变量**

1. 在 **Script Canvas 编辑器**中，打开您的 Script Canvas 图形或创建一个。

1. 在 **Variable Manager**中，选择 **Create Variable**，然后选择您的变量类型。您可以搜索以筛选变量类型列表。

    {{< note >}}
默认情况下，常见变量类型固定到列表顶部。您可以自定义固定列表以显示最常用的变量类型。为此，请选择变量类型左侧的框以固定或取消固定它。
    {{< /note >}}

    ![Choose from variable types in the Script Canvas Variable Manager.](/images/user-guide/scripting/script-canvas/variable-manager-create-variable-types.png)

1. 在 **Node Inspector **中，配置变量的属性。

    ![Configure variable properties in the Script Canvas Node Inspector.](/images/user-guide/scripting/script-canvas/node-inspector-properties-default.png)

    例如，如果添加 **Color** 变量，则可以执行以下操作：

    * 对于 **Name**, 输入名称以标识该颜色变量。您还可以双击 **Variable Manager** 中的名称来重命名变量。
    * 对于 **Color**, 输入 RGB 值或使用颜色选取器。
    * 对于 **Display Order**,输入您希望变量在 Script Canvas Editor 中显示的相对顺序，或将默认值保留为 -1。
    * 对于 **Scope**, 选择 **In** 以在 **Entity Inspector **中分配的(/docs/user-guide/components/reference/scripting/script-canvas/) 组件下显示变量属性和值，或将默认值保留为 **Local** 以保持变量对图形的私有。

        {{< note >}}
此设置允许您对多个实体使用相同的 Script Canvas 图形，但为特定实体自定义图形的一部分。当您更改组件上的变量值时，该值优先于图表中指定的默认值。
        {{< /note >}}

        ![Example of Color variable properties in the Script Canvas Node Inspector.](/images/user-guide/scripting/script-canvas/node-inspector-scope-in-example.png)

1. 在 Script Canvas Editor （Script Canvas 编辑器） 中，选择 **File**, **Save** 以保存更改。

## 设置变量的默认值

您可以在 **Node Inspector** 或 **Variable Manager** 中设置变量的默认值。

**设置变量值**

1. 在 **变量管理器** 中，选择要更新的变量。您可以搜索以筛选图表中的变量列表。

1. 执行以下操作之一以更新变量的属性：

    * 在 **Node Inspector **中，根据需要更新变量值。

        ![Set the variable default values in the Node Inspector.](/images/user-guide/scripting/script-canvas/node-inspector-modify-variable-values.png)

    * 在 **Variable Manager** 中，更新某些变量值。这些值显示在第三列中。您可以选择或双击它们。

        ![Set the variable default values in the Variable Manager.](/images/user-guide/scripting/script-canvas/variable-manager-modify-variable-values.png)

1. 在 Script Canvas Editor （Script Canvas 编辑器） 中，选择 **File**, **Save** 以保存对图表的更改。

## 创建 get 或 set 变量节点

您可以使用 get 和 set variable 节点来检索或设置变量的值。

**创建 get 或 set 变量节点**

执行以下操作之一：

* 将变量从 **Variable Manager** 拖动到画布上，然后选择 **Get `<variable name>`** 或 **Set `<variable name>`**.

* 在 **变量管理器** 中 **右键单击** 变量，然后选择**Get `<variable name>`** 或 **Set `<variable name>`**.

    ![Right-click to create a get or set variable in the Script Canvas Variable Manager.](/images/user-guide/scripting/script-canvas/variable-manager-create-get-set-variable.png)

* 使用以下键盘快捷键：
  * 按 **Shift** 并将变量从 **变量管理器** 拖动到画布上，以创建获取变量节点。
  * 按 **Alt** 并将变量从 **变量管理器** 拖动到画布上，以创建设置变量节点。

## 创建值更改的节点

您可以使用 **OnVariableValueChanged**（值已更改）事件节点对变量值的更改做出反应。

**创建值更改的节点**

执行以下操作之一：

* 将变量从 **变量管理器** 拖到画布上，然后选择 **On `<variable name>` Changed**。

    ![Drag a variable from the Script Canvas Variable Manager to the canvas to create an on-value-changed node.](/images/user-guide/scripting/script-canvas/variable-manager-create-on-value-changed.gif)

* 在图表中创建新的 **OnVariableValueChanged** 事件节点，并使用该字段的齿轮按钮将 **Source** 字段设置为变量。有关向图形添加节点的帮助，请参阅 [在 Script Canvas 中添加和连接节点](/docs/user-guide/scripting/script-canvas/editor-reference/nodes/adding-and-connecting).

    ![Set the Source field of an OnVariableValueChanged event node using the field's gear button.](/images/user-guide/scripting/script-canvas/variable-manager-create-on-value-changed-node.png)

## 删除变量

您可以从图形或 **变量管理器** 中删除变量。

**删除变量**

执行以下操作之一：

* 在画布中选择变量节点，然后按 **Delete**。
* 在 **变量管理器** 中选择变量，然后按 **Delete**。
* 在 **变量管理器** 中 **右键单击** 变量，然后选择 **Delete `<variable name>`**.

    ![Delete a variable in the Script Canvas Variable Manager.](/images/user-guide/scripting/script-canvas/variable-manager-delete-variable-node.png)
