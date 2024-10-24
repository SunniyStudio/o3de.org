---
description: ' 在 Open 3D Engine 中为模拟对象设置动画图形。 '
title: 设置模拟对象动画图节点
---

设置好模拟对象后，创建一个动画图形来查看模拟效果。这样就可以预览流苏是如何随Actor的主要动作而移动的。

**为模拟对象设置动画图形节点**

1. 在 **Anim Graph** 面板中，点击 **+** 图标创建一个动画图表。

1. **右击** 图表，选择 **Create Node**, **Sources**, **Blend Tree**。

    ![Create a blend tree node for your anim graph for the simulated object.](/images/user-guide/actor-animation/simulated-objects-7.png)

1. 双击 **BlendTree0** 节点，右击动画图表网格，选择 **Create Node**, **Physics**, **Simulated Object**。

    ![Create a simulated object node in the BlendTree0 node.](/images/user-guide/actor-animation/simulated-objects-8.png)

将**SimulatedObject0**节点的**Pose**输出连接到**FinalNode0**节点的**Input Pose**。

1. **右击**动画图形，选择 **Create Node**, **Sources**, **Motion**。

1. 将 **Motion0** 节点的 **Output Pose** 与 **Simulated Object0** 节点的 **Pose** 输入相连。

   您的图表应如下所示。

    ![Create an anim graph for the simulated object.](/images/user-guide/actor-animation/simulated-objects-10.png)

1. 选择**Motion0**节点，在**Attributes**面板中点击 **+** 图标，为节点添加一个动作。

1. 在**Motion Selection Window**，选择一个动作，如`run`，然后单击**OK**。

1. 保存动画图形并输入名称，例如 `simulatedobjects.animgraph`。

1. 选择**SimulatedObject0** 节点，在**Attributes**面板中点击**Select simulated objects**。

    ![Select a simulated object for the anim graph.](/images/user-guide/actor-animation/simulated-objects-11.png)

1. 在对话框中选择模拟对象。

    ![Select the simulated object that you created.](/images/user-guide/actor-animation/simulated-objects-12.png)

1. 保存动画图形。

   在**Anim Graph**面板中，点击播放图标运行动画图。为每个关节设置的碰撞体不会与其他碰撞体碰撞，流苏穿过Actor的手臂和胸部。

   在下一个步骤中，您将创建另一个碰撞体，并将其连接到Actor的骨架上。这样可以防止流苏进入Actor的身体。

    {{< video src="/images/user-guide/actor-animation/simulated-objects-13.mp4" info="Animate the actor and the simulated object." autoplay="true" loop="true" width="500" >}}
