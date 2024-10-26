---
description: ' 在Open 3D Engine 动画编辑器中使用 Blend Two 节点来混合两个不需要叠加混合的节点。 '
title: Blend Two 节点
---

使用 **Blend Two** 节点，可以根据权重值在两个输入姿势之间进行混合。例如，**Blend Two** 节点可以根据角色的速度在走和跑之间进行平滑混合。

除了不支持**Additive Blend**模式外，**Blend Two**与**Blend Two Additive**节点类似。

**要使用Blend Two节点**

1. 在 O3DE 编辑器中，选择 **Tools**, **Animation Editor**。

1. 创建一个[混合树](/docs/user-guide/visualization/animation/animation-editor/creating-blend-trees/)。

1. 双击您创建的混合树节点。

1. 选择 **Anim Graph Palette** 标签页，然后选择 **Blending** 标签页。

1. 拖拽 **Blend Two** 节点到动画图表中。

![On the Anim Graph Palette tab, select the Blending tab, and then drag Blend Two into the animation graph.](/images/user-guide/actor-animation/char-animation-editor-blendposes-animgraphpalette-blendtwo.png)

1.

![Blend Two node on the animation graph with inputs and outputs exposed.](/images/user-guide/actor-animation/char-animation-editor-blendposes-inoutputs-blendtwo.png)

   连接节点到以下输入和输出：
   + **Pose 1** - 第1个姿势。
   + **Pose 2** - 第2个姿势
   + **Weight** - 混合权重。

     例如，您可以使用**Float Constant**节点来指定一个介于`0.0`和 `1.0`之间的浮点值。`0.0`表示 100% 的**Pose 1** 和 0% 的**Pose 2**。`0.6`的值表示 **Pose 1** 的 40% 和 **Pose 2** 的 60%。可以指定**Weight**的其他节点包括**Parameter**节点、**Smoothing**节点等。
   + **Output Pose** - 姿势混合的结果。

## Blend Two 节点属性

有关混合节点类型共享的属性设置，请参阅 [Blend 节点属性](/docs/user-guide/visualization/animation/animation-editor/blending-poses/#animation-editor-blending-attributes)。

用于**Blend Two**节点的**Extraction Mode**的计算方法如下：
+ `S` = 源变换delta
+ `T` = 目标变换delta

蒙板中包含Root（或未提供蒙板）：
+ **Blend** = `S` + \(`T` - `S`\) \* weight
+ **Source** = `S`
+ **Target** = `T`

另外，Root不包括在蒙板中：
+ **Blend** = `S`
+ **Source** = `S`
+ **Target** = `Zero`
