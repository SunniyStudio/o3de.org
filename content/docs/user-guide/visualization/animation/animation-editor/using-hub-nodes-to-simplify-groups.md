---
description: ' 在 Open 3D Engine 的动画编辑器（Animation Editor）中使用 Hub 节点可简化动画图形中节点组之间的转换。 '
title: 使用Hub节点简化节点组
---

**Hub**节点是动画图形中节点组之间的连接点。这个传递节点输出或转发进入它的节点的姿势。集线器节点作为状态机的中心点，可降低转换的复杂性。通过将多个节点连接到中枢，可以将具有相同过渡条件的过渡线组合在一起，并有策略地组织节点，创建易于阅读的状态机。

**示例**
下面的动画图中有许多组和过渡。

![Animation graph without hub nodes.](/images/user-guide/actor-animation/animation-editor-using-hub-nodes-nohubgraph.png)
带有 **Hub** 节点的相同图形简化了组之间的转换，使图形更加简洁易读。

![Animation graph without hub nodes.](/images/user-guide/actor-animation/animation-editor-using-hub-nodes-graphwithhubs.png)

**在动画图形中使用Hub节点**

1. 通过以下操作之一在动画图中添加一个 **Hub** 节点：
  + 在**Anim Graph Palette**中，选择**Sources**选项卡，然后将**Hub**拖到图形中。
  + 在图形中，右键单击并选择**Create Node**、**Sources**、**Hub**。

   ![Animation graph without hub nodes.](/images/user-guide/actor-animation/animation-editor-using-hub-nodes-palette.png)

1. 根据需要重复添加任意数量的 **Hub** 节点。

1. 在 **Hub** 节点之间添加多个同类节点，如动作节点或状态机，并创建转换。
**示例**

   将动作节点放在两个 **Hub** 节点之间，可简化动作之间的转换。

   ![Animation graph without hub nodes.](/images/user-guide/actor-animation/animation-editor-using-hub-nodes-example.png)

   在示例中，**attack01** 和 **Hub0** 之间的转换是共享转换。如果删除 **Hub0** 节点，就可以在进入 **attack01**\* 节点的四个过渡中，分别添加来自 **attack01** 的两个条件。

1. 对于进入**Hub**节点的所有转换，将其**Transition Time**设置为`0.0`秒。这将确保不会在过渡中增加额外的延迟。

![Animation graph without hub nodes.](/images/user-guide/actor-animation/animation-editor-using-hub-nodes-transitiontime.png)
