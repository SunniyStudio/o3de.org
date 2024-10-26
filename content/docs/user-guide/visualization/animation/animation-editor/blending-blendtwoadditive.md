---
description: ' 了解如何使用 Open 3D Engine 动画编辑器中的Blend Two Additive来添加混合两个输入姿势。  '
title: Blend Two Additive 节点
---

**Blend Two Additive** 节点将姿势 2 输入与姿势 1 输入进行相加混合。

![Blend Two Additive node.](/images/user-guide/actor-animation/char-animation-editor-blendposes-blendtwoadditive.png)

{{< note >}}
该节点的功能类似于启用**Additive**后的**Blend Two (Legacy)**节点。主要区别在于，**Blend Two（Legacy）** 节点在应用叠加混合时会减去绑定姿势。此外，**Blend Two Additive**节点希望姿势 2 是一个叠加姿势。这意味着，如果您希望**“Blend Two Additive**的功能与**Blend Two (Legacy)**节点相同，您必须首先从姿势 2 中减去绑定姿势。
{{< /note >}}

**要使用Blend Two Additive节点**

1. 在 O3DE 编辑器中，选择**Tools**, **Animation Editor**。

1. 创建一个[混合树](/docs/user-guide/visualization/animation/animation-editor/creating-blend-trees/)。

1. 双击您创建的混合树节点。

1. 选择 **Anim Graph Palette** 标签页，然后选择 **Blending** 标签页。

1. 拖拽 **Blend Two Additive** 节点到动画图表中。

![On the Anim Graph Palette tab, select the Blending tab, and then drag Blend Two Additive into the animation graph.](/images/user-guide/actor-animation/char-animation-editor-blendposes-animgraphpalette.png)

1.

![Blend Two Additive node on the animation graph with inputs and outputs exposed.](/images/user-guide/actor-animation/char-animation-editor-blendposes-inoutputs.png)

连接节点到以下输入和输出：
   + **Pose 1** - 基础姿势。
   + **Pose 2** - 叠加到**Pose 1**的姿势。
   + **Weight** - 叠加的权重。

     例如，您可以使用 **Float Constant** 节点指定一个介于 `0.0` 和 `1.0` 之间的浮点数值。`0.0`表示**Pose 2** 完全不影响**Pose 1**。数值为 `1.0`表示 **Pose 2** 完全添加到 **Pose 1** 上。可以指定**Weight**的其他节点包括**Parameter**节点、**Smoothing**节点等。
   + **Output Pose** - 混合姿势的结果，可以想象为 `Pose 1 + (Pose 2 * Weight)`。

## Blend Two Additive 节点属性 

有关混合节点类型共享的属性设置，请参阅 [Blend Node 属性](/docs/user-guide/visualization/animation/animation-editor/blending-poses/#animation-editor-blending-attributes)。

 **Blend Two Additive**节点的**Extraction Mode**具有遮罩和叠加混合功能，与**Extraction Mode**相比，增加了转场的复杂性。

使用**Blend Two Additive**节点进行运动提取的输出计算如下。
+ `S` = 源变换delta
+ `T` = 目标变换delta

蒙板中包含Root（或未提供蒙板）：
+ **Blend** = `S` + `T` \* weight
+ **Source** = `S`
+ **Target** = `T`

另外，Root不包括在蒙板中：
+ **Blend** = `S`
+ **Source** = `S`
+ **Target** = `S`
