---
description: ' 学习如何在动画编辑器中使用混合空间。 '
title: 创建混合空间并将其可视化
---

混合空间是根据坐标进行空间组织的动作样本集合。可视化的表示方法是以 xy 轴表示坐标的图形。xy 轴可表示移动速度、移动方向、转弯角度等值。每个动作都由图形或混合空间中的一个点（白点）表示。

当您在混合空间中选取一个点时（通过交互方式或参数控制），角色会自动播放根据样本动作和适当的混合权重计算得出的动作。

在一维混合空间中，动作与沿线上的点相对应。在二维混合空间中，动作与二维空间中的点相对应。

## 先决条件 

在向动画图形添加混合空间节点之前，必须完成以下步骤：
+ 选择一个Actor
+ 选择一个动作集
+ 创建动画图形

更多信息，请参阅 [动画编辑器入门](/docs/user-guide/visualization/animation/animation-editor/quick-start/)。

## 创建混合空间

要创建混合空间，必须添加混合树节点和混合空间节点，然后为混合空间节点的属性指定值。

混合空间节点从**Output Pose**端口输出一个姿势。您可以将该输出连接到**Final Output**节点的**Input Pose**端口，或任何其他接受姿势作为输入的节点的输入端口。

**Blend Space 1D** 节点具有以下端口：
+ **X** - 该输入端口的值表示一维混合空间中当前感兴趣的位置。
+ **Output Pose** - 混合空间节点计算与当前感兴趣位置相对应的混合动作，并从该端口输出所产生的动作。

**Blend Space 2D** 节点具有以下端口：
+ **X** - 该输入端口的值是二维混合空间中当前感兴趣位置的 x 坐标。
+ **Y** - 该输入端口的值是二维混合空间中当前感兴趣位置的 Y 坐标。
+ **Output Pose** - 混合空间节点计算与当前感兴趣位置相对应的混合动作，并从该端口输出所产生的动作。

**创建混合空间并指定属性**

1. 在动画编辑器中，在中间顶部窗格的**Anim Graph**选项卡上，右键单击网格，然后选择**Create Node**、**Sources**、**Blend Tree**。

1. 双击 **Blend Tree** 节点转到混合树视图。

1. 通过以下操作之一将混合空间节点添加到混合树中：
   + 在**Anim Graph**选项卡的混合树视图中，右键单击网格，然后选择**Create Node**, **Blending**, **Blend Space 2D** 或 **Blend Space 1D**。
   + 在**Anim Graph Palette**中，在**Blending**标签页上，拖拽**Blend Space 2D** 或 **Blend Space 1D** 图表到混合树上。

1. 双击混合空间节点，进入混合空间视图。如果您使用的是**Blend Space 2D** 节点，您的视图应如下所示：

![Blend Space 2D node view](/images/user-guide/actor-animation/blend-space-2d-node-view.png)

1. 在**Attributes**窗格中，指定混合空间节点的属性值。这些值用于设置混合空间。

    {{< note >}}
   您可以取消 **Attributes** 窗格的锁定，以便在不滚动的情况下查看属性和值。
{{< /note >}}

    ![Blend Space 2D Attributes pane](/images/user-guide/actor-animation/animation-editor-attributes-pane.png)

   + 要使用所提供的 xy 轴值，请执行以下操作：

     1. 对于 **Calculation method (X-Axis)**, 选择 **Automatically calculate motion coordinates**。

     1. 对于 **X-Axis Evaluator**, 选择一个共同的动作特性。

     1. 对于 **Calculation method (Y-Axis)**, 选择 **Automatically calculate motion coordinates**。

     1. 对于 **Y-Axis Evaluator**，选择另一种常见的动作特性。
   + 要为 xy 轴使用自定义值，请执行以下操作：

     1. 对于 **Calculation method (X-Axis)**, 选择 **Manually enter motion coordinates**。

     1. 对于 **Calculation method (Y-Axis)**, 选择 **Manually enter motion coordinates*。

     您也可以结合使用提供值和自定义值。例如，您可以手动输入 x 轴的动作坐标，并使用**Travel distance**评估器自动计算 y 轴的动作坐标。

1. 在 **Attributes** 面板中，对于 **Motions**，点击 **+** 按钮为混合空间添加源动作资产。 

1. 在**Motion Selection Window**中，选择你要添加到到混合空间的动作，然后点击 **OK**。
   + 如果在**Calculation method**中选择了**Automatically calculate motion coordinates**，并且在**Evaluator**中选择了动作特性，则坐标值会自动计算。
   + 如果在**Calculation method**中选择了**Manually enter motion coordinates**，则必须输入坐标值。

1. 将动作添加到混合空间并计算出坐标值后，请确认混合空间视图是否与下图相似：

![Example blend space view](/images/user-guide/actor-animation/animation-editor-blend-space-example.png)

1. 在混合空间视图中，执行以下操作：

   1. 在混合空间内拖动，更改兴趣点（用红点表示）。

   1. 当高亮显示该点时，会通过混合三角形三个顶点所代表的动作自动计算出相应的动作。查看每个动作旁边的混合权重。

   1. 请注意，距离点较近的动作的混合权重要高于距离点较远的动作。

1. 在 **Attributes** 面中，可以执行以下操作：
   + 查看每个动作的坐标值
   + 更改数值以重新映射动画和混合空间图
   + 从混合空间移除一个动作
