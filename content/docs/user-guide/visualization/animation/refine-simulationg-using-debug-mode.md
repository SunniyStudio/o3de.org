---
description: ' 在 Open 3D Engine中使用调试模式完善模拟对象的运动。 '
title: '使用调试模式改进模拟 '
---

在下面的步骤中，使用调试模式完善模拟对象的运动。

**要在模拟中使用调试模式**

1. 在**Anim Graph**面板中，单击播放图标运行动画图表。

   使用速度滑块降低动画的速度。注意流苏会穿过Actor的脖子和手臂。

    ![Slow down the animation to see how the simulated object moves with the actor.](/images/user-guide/actor-animation/simulated-objects-21.gif)

1. 在 **SimulatedObject0** 节点中，单击右角方框。这将启用节点的调试渲染。

    ![Enable debug mode for the simulated object in the anim graph.](/images/user-guide/actor-animation/simulated-objects-19.gif)

1. 在调试模式下，可以执行以下操作。

    1. 要关闭模拟物体碰撞体，请单击 ![Simulated Objects Icon](/images/user-guide/actor-animation/simulated-objects-20.png)图标。

    1. 要关闭关节碰撞半径，请单击 ![Joint Collision Radius Icon](/images/user-guide/actor-animation/simulated-objects-5.png)图标。

   在调试模式下，渲染窗口中会出现以下内容：

    + Simulated object colliders - 模拟对象与之碰撞的碰撞体。碰撞体显示为浅灰色。

    + Joint angle limit cones - 模拟关节的角度。锥体显示为浅粉色。

    + Joint collision radius - 关节上碰撞半径的大小。半径显示为深灰色。

   在渲染窗口中，你可以点击第一个图标来切换Actor几何图形，只看到模拟对象。

    ![Use the render window to preview how the a tassel moves and collides with the joint colliders.](/images/user-guide/actor-animation/simulated-objects-22.gif)

1. 要固定模拟对象（tassel）的移动，请执行以下操作。

    1. 在**Simulated Object**面板中，找到`L_tassle_02_JNT`。

    1. 在**Simulated Object Inspector**中，选择Pinned`. 这将固定住流苏的第二部分，防止其移动。

    1. 对于 **Joint angle limit**，输入 `80` 以增加关节的摆动。

    1. 在**Simulated Object**面板中，选择`L_tassle_03_JNT`。

    1. 在**Simulated Object Inspector**中，对于**Radius**，输入`0.065`。

1. 要固定模拟关节碰撞体（Actor的胸部和手臂），请执行以下操作。

    1. 在**Skeleton Outliner**中，选择`C_spine_04_JNT` 在这里添加了模拟关节碰撞体。

    1. 在**Simulated Object Inspector**中，对于**Radius**，输入`0.172`。

    1. 在**Skeleton Outliner**中，选择`L_arm_JNT` 在这里添加了模拟关节碰撞体。

    1. 在**Simulated Object Inspector**中，对于**Radius**，输入`0.101`。

1. 如果您对结果满意，请保存Actor。

1. 要禁用调试模式，请再次单击**SimulatedObject0**节点上的右上方方框。

   下面的动画是Actor运行的调试完成版本。请注意，流苏不再穿过Actor的身体。

    ![View the final animation of the actor and the simulated object.](/images/shared/simulated-objects-23.gif)
    
