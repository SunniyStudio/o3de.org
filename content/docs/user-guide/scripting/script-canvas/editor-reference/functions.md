---
linktitle: 函数
title: Script Canvas 函数
description: 在 Script Canvas 中创建可重用的函数，这些函数可以作为节点插入到其他图形中。
weight: 400
---

使用 **Script Canvas 编辑器** 可以创建可重用的图形，称为函数。函数可用作 Script Canvas 图形中的节点。与传统编程语言中的函数类似，Script Canvas Editor 中的函数可促进代码重用和抽象。它们通过将执行特定任务的一组节点替换为一个函数节点来帮助简化图形。例如，您可以将一系列执行线性插值的节点移动到名为 Interpolate 的函数中，并将执行加速度夹紧的节点移动到名为 ClampAcceleration 的函数中。此外，如果您在多个图表中使用此功能，则 Functions 会使更新更容易，因为您只需在一个位置进行更改。

您的函数与 **Node Palette** 中的所有其他 Script Canvas 节点一起显示，归类在 **Global Functions** 下。

创建函数时，定义该函数的输入和输出变量，以及两者之间产生结果的所有节点。

## 创建 Script Canvas 函数

通过在 Script Canvas 编辑器中选择 **File**、**New Script** 来开始创建新函数。

![Choose File, New Function in the Script Canvas Editor to start a new Script Canvas function.](/images/user-guide/scripting/script-canvas/function-new.png)

或者，您也可以使用编辑器画布右上角的 create （创建） 按钮创建新脚本。

![Use the function create button as an alternate method for creating a new Script Canvas graph file.](/images/user-guide/scripting/script-canvas/function-quick-create.png)


### 函数入口点和出口点

函数需要入口点和出口点。要创建这些点，请右键单击节点的输入或输出执行槽，然后在上下文菜单中选择 **Expose**。通常，您可以从函数中第一个节点的输入执行槽创建一个入口点节点，并从函数中最后一个节点的输出执行槽创建一个出口点节点。

![Create an entry or exit point for a Script Canvas function by right-clicking an execution slot.](/images/user-guide/scripting/script-canvas/function-expose-input.gif)

{{< note >}}
要更改函数节点上的执行槽的名称，请编辑入口点或退出点节点上的 name 字段。
{{< /note >}}

或者，如果您不确定如何将入口点或出口点连接到函数的其余部分，则可以使用工具栏按钮创建入口点或出口点节点：![Function toolbar buttons](/images/user-guide/scripting/script-canvas/function-toolbar-buttons.png)。稍后在您准备好时连接其执行槽。

### 函数数据参数

函数还可以具有输入和输出数据参数。输入参数是传递给函数的值。Output parameters 是函数返回的值。这些都定义为 **变量管理器** 中的变量。函数还可以具有局部变量，这些变量不会在函数的节点上公开。

![Create function data parameters with variables in the Variable Manager.](/images/user-guide/scripting/script-canvas/function-create-parameter.gif)

变量的范围决定了变量是否以及在函数节点上的显示位置。

|范围 |节点上的位置 |用法 |
| --- | --- | --- |
| Local | (None) | 这是一个局部变量，仅供函数使用。 |
| In | Input slot | 这是一个输入参数。此变量的值将传入函数。 |
| Out | Output slot | 这是一个 result 变量。该函数返回其值作为结果。 |
| In / Out | Both sides | 这是一个输入参数，可由函数修改并作为结果返回。 |

## 在图表中使用 Script Canvas 函数

保存 Script Canvas 函数后，它会自动显示在 **Node Palette** 的 **Global Functions** 下。新函数的默认目录是项目的 `scriptcanvas\functions`  目录。如果将它们保存在子目录中，或将它们保存在不同的项目目录下，则目录结构用于对 **Global Functions** 中的函数进行分类。

使用函数就像在 Script Canvas 中使用任何其他节点一样。只需将函数拖放到图表中即可。放置后，画布上会显示一个表示该函数的节点。

![Drag and drop functions from the Node Palette, under the Global Functions category, onto your graph.](/images/user-guide/scripting/script-canvas/function-use-node.gif)

## 函数示例：线性插值

在此示例中，我们创建一个线性插值函数。此函数由以下公式表示： *Result = Start + Time \* (End - Start)*.

在此示例中，您将学习如何执行以下操作：

+ 为函数创建入口点和出口点。
+ 创建输入参数槽。
+ 创建返回值。

最终的函数图应如下所示：

![After completing the steps in this example, you should end up with a linear interpolation function.](/images/user-guide/scripting/script-canvas/function-linear-interpolation.png)

1. 使用 **File**、**New Script** 或使用画布右上角的函数创建按钮启动新函数。

1. 单击左侧的 **Create execution nodeling** 按钮，在画布上创建一个输入节点。

1. 单击 Add data input 旁边的 **+** 按钮，将输入参数添加到执行节点。这将提示您输入新变量的名称和类型。输入 **Start** 作为名称，输入 **Number** 作为类型。对 **Time** 和 **End** 再重复两次

1. 在 Node Inspector 中，展开 Math 类别。

1. 将 **Add**、**Subtract** 和 **Multiply** 节点拖到画布上，并按照上图的方式排列它们。

1. 使用 变量引用](/docs/user-guide/scripting/script-canvas/editor-reference/variables/variable-references)，执行以下操作：

    a. 在 **Subtract** 节点中，引用 *End* 和 *Start* 变量，以便从 *End* 中减去 *Start*。
    
    b. 在 **乘法** 节点中，使用减法的结果并引用 *Time* 变量，以便 *（End - Start）* 乘以 *Time*。
    
    c. 在 **Add** 节点中，使用乘法结果并引用 *Start* 变量，以便将这两个值相加。

1. 为您的函数创建一个退出点。

   a. 单击右侧的 **Create execution nodeling** 按钮，在画布上创建一个输出节点。

   b. 单击 Add data output 旁边的 **+** 按钮，将输入参数添加到输出执行节点。这将提示您输入新变量的名称和类型。输入 **Result** 作为名称，输入 **Number** 作为类型。

   c. **结果** 将出现在 Variable Manager 下的变量列表中。将其拖动到画布上，然后从出现的菜单中选择 **Set Result**。
   
   d. 在 **Add** 节点和输出执行节点之间拖动新的 setter 节点。

1. 将连接从 **Add** 节点输出拖动到 **Result** 节点 **In** 连接器。将 **Add** 节点的 Result 引脚拖动到 **Result** 节点的数字值。

1. 将 **Result** 节点的 out 连接器拖动到输出执行节点的 in 连接器上。

1. 最后，使用 **File**、**Save** 保存函数并将其命名为 **Interpolate**。该函数现在可以在 Script Canvas 图形中使用：

   ![When a function is used in a Script Canvas graph, it appears as a node, using the function's filename as the node name.](/images/user-guide/scripting/script-canvas/function-linear-interpolation-node.png)
  
