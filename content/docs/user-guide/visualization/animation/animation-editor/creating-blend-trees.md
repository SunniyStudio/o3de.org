---
description: ' 了解如何在 O3DE 动画编辑器中创建混合树。 '
title: 创建混合树
---


在**动画编辑器**中，动画图由定义转换和节点间数据传递方式的节点和连接组成。**动画编辑器**支持状态机和混合树。

混合树是带有输入和输出端口的节点集合，这些端口按数据类型用颜色编码，从左到右读取。输入端口位于节点左侧，输出端口位于节点右侧。混合树会输出连接到**Final Node**的姿态，**Final Node**包含在每个混合树中，且无法删除。如果您选择不将节点连接到**Input Pose**，**Final Node**会输出一个绑定姿势。

**要创建混合树**

1. 在 O3DE 编辑器中，选择 **Tools**, **Animation Editor**。

1. 在 **Animation Editor**中，在 **Anim Graph** 标签页上，点击 **+** 图标创建一个动画图表。

1. 单击**Save**图标。导航到要保存动画图形的目录。键入文件名，然后单击**Save**。

1. 在中心窗格的**Anim Graph**选项卡上，右键单击网格，然后选择 **Create Node**, **Sources**, **Blend Tree**。

![BlendTree node added to Anim Graph](/images/user-guide/actor-animation/anim-graph-blend-tree-node.png)

   或者，在**Anim Graph Palette**的**Sources**选项卡上，将**Blend Tree**拖入动画图形中。
   
   ![BlendTree icon in Anim Graph Palette](/images/user-guide/actor-animation/anim-graph-palette-blend-tree-node.png)

1. 双击您创建的混合树节点。双击节点后，动画图形上方会出现一个新链接，并显示节点名称。**Final Node**也会出现。

![FinalNode visible after double-clicking Blend Tree](/images/user-guide/actor-animation/anim-graph-node-path.png)

1. 执行以下操作以添加节点和连接：

   1. 在动画图形中，右键单击网格并选择**Create Node**。从以下类别中选择一个节点：
      + **Sources**
      + **Blending**
      + **Controllers**
      + **Logic**
      + **Math**
      + **Misc**

1. 重复步骤 6a，为混合树添加更多节点。

1. 通过拖动输入到输出来连接节点。注意以下颜色提示：
      + 灰色 - 灰色辅助虚线表示可以连接的端口。
      + 绿色 - 当可以松开鼠标键时，连接曲线会变成绿色。
      + 红色 - 不允许连接时，连接曲线变为红色。
      + 黄色 - 在过渡状态下，例如在端口之间拖动连接时，连接曲线会变成黄色。

**主题**
+ [创建和可视化混合空间](/docs/user-guide/visualization/animation/animation-editor/blend-spaces/)
+ [使用 Blend 节点混合姿势](/docs/user-guide/visualization/animation/animation-editor/blending-poses/)
