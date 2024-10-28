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
**Min angle** 不能大于 **Max angle**。如果出现这种情况，则会以红色显示错误，并且该值不会提交到图表中。
{{< /note >}}

1. 对于 **Twist axis**, 选择 **X**, **Y**, 或 **Z Axis**。

   **Twist axis**指定**Rotation Limit**节点要分解和应用已编辑约束的轴。

![Rotation Limit attributes panel.](/images/user-guide/actor-animation/rotation-limit-properties.png)

## Vector Decompose 节点 

通过使用 **Vector Decompose** 节点，可以输出向量的一个或多个特定值。

**示例**
您有一个表示三维世界中某个位置的三维向量 XYZ，但您只需要它的高度（Z）来进行计算。您可以将矢量输入一个 **Vector3Decompose** 节点，然后只使用 Z 输出进行计算。

![Example of the Vector Decompose nodes in an animation graph.](/images/user-guide/actor-animation/vector-decompose.png)

如果只是添加或减去 X、Y、Z 或 W 位置中的一个，则无需使用**Vector Decompose**节点。对于典型的 **Vector3** 到 **Vector2**（反之亦然）的转换，或 **Vector3** 到 **Vector4**（反之亦然）的转换，**动画编辑器**会按以下方式自动转换矢量：
+ **Vector2** to **Vector3** - 添加 **Z** 分量，设置为 `0`。
+ **Vector3** to **Vector2** - 忽略**Vector3**的**Z**分量。
+ **Vector3** to **Vector4** - 添加**W**分量，设置为 `0`。
+ **Vector4** to **Vector3** - 忽略**Vector4**的**W**分量。

## Boolean Logic 节点 

使用**Boolean Logic**节点，您可以对两个布尔输入应用一个函数。布尔值总是 `1` 或 `0`（真或假），例如复选框项。布尔逻辑**节点将任何非零值视为真\(`1`\)，将任何零值视为假\(`0`\)。例如，值 `0.54`、`10.43` 或 -`2.25` 都是 true \(`1`\)。只有`0.0`的值是假的\(`0`\)。

选择输出类型时，可以使用 **Float** 输出或 **Bool** 输出。**Bool**输出传递`0`或`1`。而 **Float** 输出则传递您在属性中指定的浮点数值。

![Example of a Boolean Logic node in an animation graph.](/images/user-guide/actor-animation/boolean-logic-node.png)

### Boolean Logic 节点属性 

![Boolean Logic node attributes pane.](/images/user-guide/actor-animation/boolean-logic-node-attributes.png)

**Boolean Logic** 节点具有一组属性，可对布尔值执行操作。


****

| 属性 | 说明 |
| --- | --- |
|  **Name**  |  节点的名称。  |
|  **Logic Function**  |  您可以在布尔输入上设置以下功能：   |
|  **Default Value**  |  当只指定一个输入值时，用作第二个值。  |
|  **Float Result When True**  |  设置布尔函数结果为 true \(`1`\)时输出的浮点值。您还必须使用 **Float** 输出连接器来输出该值。  |
|  **Float Result When False**  |  设置布尔函数结果为 false \(`0`\) 时输出的浮点值。您还必须使用 **Float** 输出连接器来输出该值。  |

![Example of the Boolean Logic nodes in an animation graph.](/images/user-guide/actor-animation/boolean-logic-node-attributes-1.png)
