---
linktitle: 变量引用
title: 在 Script Canvas 节点中创建变量引用
description: 在 Open 3D Engine （O3DE） Script Canvas 节点上创建变量引用，作为获取或设置变量值的快捷方式。
weight: 300
---

**变量引用** 提供了一个快捷方式，用于直接从使用它们的 Script Canvas 节点获取和设置变量值。通过将变量从 **Variable Manager** 直接拖动到引脚上，可以将任何数据引脚转换为变量引用。

**创建变量引用**

* 将变量从 Variable Manager 拖动到数据输入引脚上以创建输入引用，或拖动到数据输出引脚以创建输出引用。

    ![Create a variable reference by dragging a variable from Variable Manager onto a Script Canvas node's data pin.](/images/user-guide/scripting/script-canvas/variable-reference-create.gif)

    输入引用的执行方式与 **Get** 变量节点相同，并在节点执行时检索变量的值。

    输出引用的执行方式与 **Set** 变量节点相同，并在节点执行时将该槽的输出分配给指定的变量。

    {{< note >}}
当您具有对该变量的变量引用时，可以更改该变量的名称。但是，您可能会注意到，新变量名称的显示不会立即反映在包含引用的节点中。要刷新节点变量名称引用的显示，请使用指针在节点上移动，或关闭并重新打开 Script Canvas。
    {{< /note >}}

**选择不同的变量引用**

1. 选择数据引脚上变量名称旁边的齿轮按钮。

1. 从列表中选择其他变量。

    ![Change the variable in a variable reference by choosing the gear button next to the variable name on the data pin and selecting a different variable from the list.](/images/user-guide/scripting/script-canvas/variable-reference-change.png)

**将变量引用转换回值或转换为变量节点**

执行以下操作之一：

* 双击数据引脚名称可在引用和值之间切换。
* **右键单击** 数据引脚，然后选择 **Convert to Value** 以将该数据引脚恢复为值。
* 右键单击数据引脚，然后选择 **Convert to Variable Node** 以从数据输入引脚创建 **Get** 变量节点，或从数据输出引脚创建 **Set** 变量节点。

    ![Convert a variable reference back to a value or to a variable node by right-clicking on the data pin and choosing from the Convert options.](/images/user-guide/scripting/script-canvas/variable-reference-convert-back.gif)

将数据引脚转换为变量引用

1. 执行以下操作之一：
   * 双击数据引脚名称可在值和引用之间切换。
   * **右键单击** 数据引脚，然后选择 **Convert to Reference**。

    ![Convert a data pin to use a variable reference by right-clicking on the pin and choosing Convert to Reference.](/images/user-guide/scripting/script-canvas/variable-reference-convert-pin.png)

1. 使用显示的变量名称字段旁边的齿轮按钮，然后选择要引用的变量。

    {{< tip >}}
创建变量引用的另一种方法是将变量从 **Variable Manager** 拖到 data pin上。
    {{< /tip >}}

**将变量节点转换为变量引用**

执行以下操作之一：

* 右键单击 **Get** 变量节点，然后选择 **Convert to References**。这会将节点转换为其后节点上的变量引用。

    {{< note >}}
如果 **Get** 变量节点的数据输出未连接到另一个节点，则变量节点将被删除。
    {{< /note >}}

* **右键单击** **Set** 变量节点，然后选择 **Convert to References**。这会将节点转换为其前面的节点上的变量引用。

    {{< note >}}
如果 Set** 变量节点的数据输入未连接到另一个节点，则会删除该变量节点。
    {{< /note >}}

    ![Convert a variable node into a variable reference on a data pin by right-clicking on the variable node and choosing Convert to References.](/images/user-guide/scripting/script-canvas/variable-reference-convert-variable-node.gif)
