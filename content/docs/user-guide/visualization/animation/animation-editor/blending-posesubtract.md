---
description: ' 使用Open 3D Engine动画编辑器中的 Pose Subtract 节点从另一个姿势中减去一个姿势。 '
title: Pose Subtract 节点
---

**Pose Subtract** 节点从**Pose 1** 减去 **Pose 2**。**Pose Subtract** 节点的输出是 (**Pose 1** - **Pose 2**)之间的Delta。

{{< note >}}
使用**Pose Subtract**节点，您可以生成一个添加姿势。然后，您可以在运行时将其提供给**Blend Two Additive**节点，而无需从[DCC](/docs/user-guide/appendix/glossary#dcc)中手动生成。**Pose Subtract** 节点的输出不能用作 **Blend Two（Legacy）** 节点的输入。这是因为**Blend Two（Legacy）** 并不期望一个已经是三角或加法姿势的姿势，而是期望一个绑定姿势。
{{< /note >}}

**要使用 **Pose Subtract** 节点**

1. 在 O3DE 编辑器中，选择**Tools**, **Animation Editor**。

1. 创建一个[混合树](/docs/user-guide/visualization/animation/animation-editor/creating-blend-trees/)。

1. 双击你创建的混合树节点。

1. 选择 **Anim Graph Palette** 标签页，然后选择 **Blending** 标签页。

1. 拖拽 **Pose Subtract** 节点到动画图表中。

![On the Anim Graph Palette tab, select the Blending tab, and then drag Pose Subtract into the animation graph.](/images/user-guide/actor-animation/char-animation-editor-blendposes-animgraphpalette-posesubtract.png)

1.

![Pose Subtract node on the animation graph with inputs and outputs exposed.](/images/user-guide/actor-animation/char-animation-editor-blendposes-inoutputs-posesubtract.png)

   连接节点到以下输入和输出：
   + **Pose 1** - 基础姿势。
   + **Pose 2** - 此姿势将从基础姿势减去。
   + **Output Pose** - 姿势减法 (**Pose 1** - **Pose 2**)的结果。

## Pose Subtract 节点属性 

有关混合节点类型共享的属性设置，请参阅 [Blend 节点属性](blending-poses/#blend-node-attributes)。
