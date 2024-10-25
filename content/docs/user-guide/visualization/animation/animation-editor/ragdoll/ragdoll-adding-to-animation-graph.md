---
description: ' 在 Open 3D Engine 动画编辑器的动画图形中添加布娃娃。'
title: 将布娃娃添加到动画图形中
weight: 200
---

创建动画图形来控制角色的布偶模拟时，需要执行以下操作：

+ 准备Actor资产。
+ 调整动画图形以启用布偶。
+ 预览。

动画图谱可控制角色的布偶模拟。当您的角色过渡到具有布偶节点的混合树时，布偶会自动激活并在**O3DE 编辑器**的游戏模式中模拟。当角色脱离该状态时，布偶会停止激活。布偶节点会在**动画编辑器**中输出一个绑定姿势。

**创建从运行状态过渡到布偶状态的动画图**

1. 在菜单栏右侧的**动画编辑器**中，从下拉列表中选择**AnimGraph**。这将改变布局。

    ![Change the Animation Editor layout by choosing AnimGraph from the drop-down list](/images/user-guide/actor-animation/ragdoll-animation-editor-layout-option-animgraph.png)

1. 在**Anim Graph**窗格中，单击**+**图标创建新的动画图形。

1. 右键单击网格，然后选择**Sources**，**Motion**。或者，在**Node Palette**的**Sources**部分，将**Motion**拖入动画图形。

    ![Add a Motion node to the animation graph from the context menu or the Node Palette in the Animation Editor](/images/user-guide/actor-animation/ragdoll-anim-graph-context-menu-motion-node.png)

1. 在动画图表中选择 **Motion** 节点。

1. 在 **Inspector** 面板中，完成以下操作：

   1. 对于 **Name**， 为动作输入名称。例如，**Run**。

   1. 在**Select motions**下点击 **+** 图表。在**Motion Selection**窗口中，选择一个动作，然后点击**OK**。

1. 右击网格，然后点击**Sources**, **Blend Tree**。同样，在**Node Palette**中，在**Sources**部分，将**Blend Tree**拖拽到动画图表中。
    ![Add a Blend Tree node to the animation graph from the context menu or the Node Palette in the Animation Editor](/images/user-guide/actor-animation/ragdoll-anim-graph-palette-blend-tree.png)

1. 在动画图表中选择 **Blend Tree** 节点。

1. 在**Inspector**面板中，为混合树输入名称。例如，**Ragdoll**。

1. 在动画图表中，连接**Motion**节点到**Blend Tree**节点。例如，连接**Run**节点到**Ragdoll**节点。

    ![Connect the Motion node to the Blend Tree node in the animation graph in the Animation Editor](/images/user-guide/actor-animation/ragdoll-animation-graph-connect-motion-and-ragdoll-nodes.png)

1. 双击**Blend Tree**节点。

1. 右击网格，然后选择**Physics**, **Activate Ragdoll Joints**。同样，在**Node Palette**中，在**Physics**部分，将**Activate Ragdoll Joints**拖拽到动画图表中。

1. 将**Activate Ragdoll Joints**的**Output Pose** 连接到**Final Node**的 **Input Pose**。

    ![Connect Activate Ragdoll Joints node to Final Node in the animation graph](/images/user-guide/actor-animation/ragdoll-animation-graph-activate-ragdoll-joints.png)

1. 在动画图形的根部，选择从**Motion**节点开始并连接到**Blend Tree**节点的过渡线。例如，选择连接 **Run** 节点和 **Ragdoll** 节点的过渡线。

    ![Select the transition line that connects the Motion node to the Blend Tree node in the Animation Editor](/images/user-guide/actor-animation/ragdoll-animation-graph-transition-line.png)

1. 在**Inspector**面板中，点击 **Add condition** ，然后选择 **Time Condition**。

    ![Add a Time Condition from the Attributes pane in the Animation Editor](/images/user-guide/actor-animation/ragdoll-animation-graph-add-time-condition.png)

1. 在 **Time Condition** 下，设置 **Countdown Time**。

1. 在动画图表中，点击 **Motion** 节点进行预览。
