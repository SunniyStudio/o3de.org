---
description: ' Learn how to use the Blend N node in Open 3D Engine Animation Editor to customize input weights when blending a number of input nodes. '
title: Blend N 节点
---

**Blend N** 节点最多可接受 10 个输入，并使用**Weight**参数来决定使用哪些输入及其权重。您可以在**Blend N** 节点的**Weight**输入中指定任何类型的参数。

![The Blend N node properties.](/images/user-guide/actor-animation/animation-editor-blending-blendn.png)

**要使用Blend N 节点**

1. 在 O3DE 编辑器中，选择 **Tools**, **Animation Editor**。

1. 创建一个 [混合树](/docs/user-guide/visualization/animation/animation-editor/creating-blend-trees/)。

1. 双击你创建的混合树节点。

1. 选择 **Anim Graph Palette** 标签页，然后选择 **Blending** 标签页。

1. 拖拽 **Blend N** 节点到动画图表中。

![On the Anim Graph Palette tab, select the Blending tab, and then drag Blend N into the animation graph.](/images/user-guide/actor-animation/animation-editor-blending-blendn-select.png)

1. 连接节点到以下输入和输出：
   + **Pose 0 to 9** - 姿势输入。连接一个或多个输入端。
   + **Weight** - 决定使用哪些姿势输入及其权重的输入。
   + **Output Pose** - 混合姿势的结果。

   ![Blend N node on the animation graph with inputs and outputs exposed.](/images/user-guide/actor-animation/animation-editor-blending-blendn-inoutputs.png)

1. 选择 **Blend N** 节点。

1. 对于每个姿势，按升序排列输入**Max weight trigger**。
**例如**

   如果有三个姿势，必须按升序指定数值。第一个姿势的值应该最低，最后一个姿势的值必须最高。

   ![Example of ascending order for Max weight trigger values.](/images/user-guide/actor-animation/animation-editor-blending-blendn-example.png)

如果输入的一系列数值顺序无效，数值框会变红并显示警告。

![Example of ascending order for Max weight trigger values.](/images/user-guide/actor-animation/animation-editor-blending-blendn-error.png)

您可以使用**Evenly Distribute**功能自动计算权重的平均分配。

**使用Evenly Distribute功能分配权重**

1. 在第一个输入的**Max weight trigger**中，输入最低值。

1. 在最后一个输入的**Max weight trigger**中，输入最高值。

1. 单击 **Evenly Distribute**。此功能计算并平均分配数值。
**示例**

   您有四个输入端。最低输入值设置为`0.0`，最高输入值设置为 “1.0”。点击 **Evenly Distribute**后，中间值将自动计算为平均分布在 `0`和 `1`之间。最终值为 `0.0`、`0.33`、`0.66` 和 `1.0`。

**Weight**参数的值通过其相对于**Max weight trigger**值的值来决定混合哪些输入。**Weight**值自然落在最低**Max weight trigger**值之前、两个值之间或最高**Max weight trigger**值之后。如果低于最低**Max weight trigger**值，那么计算中只使用该姿势。如果高于最高**Max weight trigger**值，则只使用该姿势。如果在两个值之间，则使用这两个姿势。

**示例**
输入端口**Pose 5**、**Pose 7**和**Pose 9**相连，**最大权重触发**值为`-2.0`, `4.0`, 和 `8.0`。如果输入值小于或等于 -2.0，则只使用端口 **Pose 5** 计算输出姿势。如果输入值介于 -2.0 和 4.0 之间，则 **Pose 5** 端口和 **Pose 7** 端口都用于计算输出姿势。如果权重大于`8.0`，则只使用端口**Pose 9**。

分配给一对中每个值的权重取决于 **Weight** 参数相对于姿态值的位置。它计算各自的距离，并根据其位置分配权重。

**示例**
**Weight** 输入设置为 `0.0`。**Pose 5** 设置为`-1.0`，**Pose 7** 设置为`3.0`，两者相差 4.0。由于 `0.0` 的值处于 `-1.0` 和 `3.0` 之间的 25% 点，因此给 **位置 5** 分配了 `0.25` 的权重。余下的\(`0.75`\)配给**Pose 7**。
