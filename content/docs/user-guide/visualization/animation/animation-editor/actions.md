---
description: ' 您可以为 Open 3D Engine 动画编辑器动画图形中的状态或转换添加一个操作，以触发参数值变化。 '
title: Actions
---

当动画图形状态或转换达到指定状态时，可以使用操作来触发参数值更改。

您可以在以下情况下设置**Parameter Action**：

**State**

**Set actions to trigger upon:**
+ Entering a state - 当状态完全融入节点，不再处于过渡状态时，操作触发
+ Exiting a state - 当节点的状态被完全混合，节点不再处于活动状态时，触发操作

**Transition**

**Set actions to trigger upon:**
+ Entering a transition - 开始转换时立即触发操作
+ Exiting a state - 当过渡完全融合到目标状态时触发操作

## 为状态添加操作

为状态添加操作，以实现参数值变化，该变化在状态完全融入节点或完全脱离节点后触发。

**为状态添加操作**

1. 在**Animation Editor**中，打开或创建一个动画图表。

1. 选择一个节点。

1. 在**Attributes**面板中，点击**Add action**，然后点击**Parameter Action**。

1. 在**Attributes**面板的**Parameter Action**下，选择**Trigger Mode**。您可以选择以下选项之一：
   + **On Enter** - 当状态完全融入节点，不再处于过渡状态时，执行**State**操作
   + **On Exit** - 当节点的状态已完全融合，节点不再处于活动状态时，执行**State**操作

   ![For the Trigger Mode, select On Enter or On Exit.](/images/user-guide/actor-animation/char-animation-editor-actions-triggermode.png)

1. 点击 **Select parameter** 并选择想要的参数。

   所选参数名称将取代**Select parameter**框中的文本。

   ![Choose Select parameter and select a parameter. The parameter name replaces the text in the Select parameter box.](/images/user-guide/actor-animation/char-animation-editor-actions-selectaction.png)

1. 要开启该操作，请设置 **Trigger Value** 为 `1`。

## 为过渡添加操作

在过渡中添加一个操作，以实现参数值的改变，该改变在过渡开始时或过渡完全融合到目标状态时触发。

**为过渡添加操作**

1. 在**Animation Editor**中，打开或创建一个动画图表。

1. 选择一个过渡。

![Select a transition.](/images/user-guide/actor-animation/char-animation-editor-actions-transition.png)

1. 在 **Attributes** 面板中，点击 **Add action** ，然后点击 **Parameter Action**。

![To add a Parameter Action, click Add action and then Parameter Action.](/images/user-guide/actor-animation/char-animation-editor-actions-addaction-transition.png)

以下选项会触发从属图的更改，但从属图的参数输入不同：
   + **Follower Parameter Action** - 提供了一个常量值
   + **Symbolic Follower Parameter Action** - 提供用户选择的值

1. 在**Attributes**面板中，在**Parameter Action**下，选择**Trigger Mode**。你可以选择以下之一：
   + **On Enter** - 开始转换时立即执行操作
   + **On Exit** - 当过渡完全融合到目标状态时执行操作

   ![For the Trigger Mode, select On Enter or On Exit.](/images/user-guide/actor-animation/char-animation-editor-actions-triggermode.png)

1. 选择 **Select parameter** ，并选择想要的参数。

   所选参数名称将取代**Select parameter**框中的文本。

   ![Choose Select parameter and select a parameter. The parameter name replaces the text in the Select parameter box.](/images/user-guide/actor-animation/char-animation-editor-actions-selectaction.png)

1. 要开启该操作，请设置**Trigger Value** 为 `1`。

    {{< note >}}
   在过渡线上设置触发器时，过渡线上会出现一个正方形，表示该操作。
{{< /note >}}

![Visual representation of an action set on a transition.](/images/user-guide/actor-animation/char-animation-editor-actions-square.png)
