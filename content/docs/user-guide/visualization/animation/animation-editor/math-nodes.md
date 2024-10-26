---
description: ' 使用动画编辑器的数学节点在 Open 3D Engine中执行数学运算。 '
title: 使用数学节点
---

O3DE 的**动画编辑器**具有一组数学节点，可对各种输入执行数学运算。数学节点将运算结果作为输出。

**主题**
+ [Rotation Math 2 节点](#rotation-math)
+ [Rotation Limit 节点](#rotation-limit)
+ [Vector Decompose 节点](#vector-conversion)
+ [Boolean Logic 节点](#boolean-logic-node)

## Rotation Math 2 节点 

使用 **Rotation Math 2** 节点，可以对输入旋转进行数学运算，输入旋转用 [四元数](/docs/user-guide/appendix/glossary#quarternion) 表示。

该节点将输入旋转与指定的**Default Value**相乘，以表示输出旋转。**Default Value**指定未连接输入的旋转。该旋转值以欧拉角度数表示（绕 X、Y 和 Z 轴旋转）。

![Example of the Rotation Math 2 node in an animation graph.](/images/user-guide/actor-animation/rotation-math.png)

**要使用Rotation Math 2节点**

1. 连接**Output Rotation**输出rotation输出到**RotationMath2**节点的输入。

1. 选择 **RotationMath2** 节点。

1. 在右侧面板中，在**Attributes**标签页上，指定**Math Function**。你可以指定以下内容：
**Rotate**
将两个输入四元数或一个输入四元数与**Default Value**相乘。
**Inverse rotate**
将 **x** 输入值与 **y** 输入值的倒数相乘。您也可以用它来计算 X 相对于 Y 的相对旋转。

1. 如果只有一个输入旋转，则为 **Default Value**指定未连接的旋转值（X、Y、Z）。 

## Rotation Limit 节点 

![Figure of Rin in the Animation Editor with a rotated foot.](/images/user-guide/actor-animation/rotation-limit-figure.png)

使用 **Rotation Limit** 节点，可以限制输入的旋转。为此，节点会沿着相关轴分解四元数的旋转，并将其角度限制在一定范围内。您可以定义可能的最小和最大角度值，以消除由两个值定义的最短或最长路径角度之间的歧义。

![Example of the Rotation Limit node in an animation graph.](/images/user-guide/actor-animation/rotation-limit-graph.png)

**要使用 **Rotation Limit** 节点**

1. 连接rotation输出到**RotationLimit**节点的**Input Rotation**。

1. 选择 **RotationLimit** 节点。

1. 在右侧面板中，在**Rotation limits**的**Attributes**面板上，为 **X**, **Y**, 和 **Z** 输入**Min angle** 和 **Max angle** 的值。

    {{< note >}}
**Min angle** 不能大于 **Max angle**。. If it does, the error is displayed in red and the value doesn't commit to the graph.
{{< /note >}}

1. For **Twist axis**, select the **X**, **Y**, or **Z Axis**.

   The **Twist axis** specifies which axis the **Rotation Limit** node is to decompose and apply edited constraints.

![Rotation Limit attributes panel.](/images/user-guide/actor-animation/rotation-limit-properties.png)

## Vector Decompose Nodes 

Using **Vector Decompose** nodes, you can output one or more specific values of a vector.

**Example**
You have a 3D vector XYZ that indicates a position in a 3D world, but you only need its height (Z) for a computation. You would input your vector into a **Vector3Decompose** node and use only the Z output for your calculation.

![Example of the Vector Decompose nodes in an animation graph.](/images/user-guide/actor-animation/vector-decompose.png)

You don't need to use **Vector Decompose** nodes if you are simply adding or subtracting one of the X, Y, Z, or W positions. For typical **Vector3** to **Vector2** (and vice versa) conversion, or **Vector3** to **Vector4** (and vice versa) conversion, the **Animation Editor** automatically converts vectors in the following way:
+ **Vector2** to **Vector3** - Adds the **Z** component set to `0`.
+ **Vector3** to **Vector2** - Ignores the **Z** component from **Vector3**.
+ **Vector3** to **Vector4** - Adds the **W** component set to `0`.
+ **Vector4** to **Vector3** - Ignores the **W** component from **Vector4**.

## Boolean Logic Node 

Using the **Boolean Logic** node, you can apply a function to two boolean inputs. Boolean values are always `1` or `0` (true or false), such as a check box item. The **Boolean Logic** node sees any non-zero value as true \(`1`\) and any zero value as false \(`0`\). For example, values `0.54`, `10.43` or -`2.25` are all true \(`1`\). Only `0.0` values are false \(`0`\).

When choosing an output type, you can output from the **Float** output or the **Bool** output. The **Bool** output passes on a `0` or `1`. The **Float** output passes on a float value that you specify in the attributes.

![Example of a Boolean Logic node in an animation graph.](/images/user-guide/actor-animation/boolean-logic-node.png)

### Boolean Logic Node Attributes 

![Boolean Logic node attributes pane.](/images/user-guide/actor-animation/boolean-logic-node-attributes.png)

The **Boolean Logic** node features a set of attributes that performs operations on boolean values.


****

| Attribute | Description |
| --- | --- |
|  **Name**  |  Name of the node.  |
|  **Logic Function**  |  You can set the following functions on boolean inputs:   |
|  **Default Value**  |  Used as a second value when only one input value is specified.  |
|  **Float Result When True**  |  Sets a float value to output when the result of the boolean function is true \(`1`\). You must also use the **Float** output connector to output this value.  |
|  **Float Result When False**  |  Sets a float value to output when the result of the boolean function is false \(`0`\). You must also use the **Float** output connector to output this value.  |

![Example of the Boolean Logic nodes in an animation graph.](/images/user-guide/actor-animation/boolean-logic-node-attributes-1.png)
