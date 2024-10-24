---
description: ' 在 Open 3D Engine中设置模拟物体碰撞体，使Actor的身体与流苏产生互动。 '
title: 设置模拟物体碰撞体
---

在以下步骤中，为脊柱和左臂添加碰撞体，以减少流苏的移动。这样可以确保流苏有东西可以碰撞，从而防止它穿过Actor的身体。

**设置模拟物体碰撞体**

1. 在**Skeleton Outliner**中，选择要添加模拟物体碰撞体的一个或多个关节。在本例中，使用 `C_spine_04_JNT`关节。

1. 右键单击选区并选择 **Simulated object collider**, **Add collider**, **Add capsule**。这样就会创建一个定义关节碰撞区域的形状。

   如果球形更适合你的Actor，你也可以添加一个球形。

    ![Add a capsule shape to create a collider area for your skeleton.](/images/user-guide/actor-animation/simulated-objects-14.png)

    在**Skeleton Outliner**中，模拟对象碰撞体 ![Simulated Object Collider Icon](/images/user-guide/actor-animation/simulated-objects-20.png) 图标出现在关节旁边。

1. 在**Simulated Object Inspector**中，调整胶囊，使其大于Actor的几何体。

    在本例中，将**Height**改为`0.344`，**Radius**改为`0.162`。

    默认情况下，关节的名称\(`C_spine_04_JNT`\)也是碰撞体的名称。

    ![Create a simulated object collider for the spine and make changes, so that it's slightly larger than the actor's shape.](/images/user-guide/actor-animation/simulated-objects-15.png)

1. 重复**步骤 1** 添加`L_arm_JNT`关节。这将为左上臂创建另一个模拟对象碰撞体。

1. 重复**步骤 2** 和 **步骤 3**，为手臂设置碰撞体。

1. 调整胶囊，使其与机械臂相匹配。在本例中，输入以下值：
    + **Height** to `0.322`
    + **Radius** to `0.081`
    + **Rotation: X** to `180`
    + **Radius: Y** to `89.99`
    + **Radius: Z** to `180`

   默认情况下，关节的名称\(`L_arm_JNT`\)也是碰撞体的名称。

    ![Create a simulated object collider for the arm and make changes, so that it's slightly larger than the actor's shape.](/images/user-guide/actor-animation/simulated-objects-16.png)

1.  在**Skeleton Outliner**中，选择连接模拟对象碰撞体的关节。

    {{< tip >}}
    在渲染窗口中，取消选择第一个图标（**Solid**），然后选择第二个图标（**Wireframe**），即可查看胶囊碰撞体。
    {{< /tip >}}

    在渲染窗口中，碰撞体显示为紫色。如果取消选择关节，对撞器将显示为灰色。

    现在您已经为脊柱和手臂添加了模拟对象，请为模拟对象添加这些对撞器。

    ![View the capsule colliders that you attached to the joints of the actor.](/images/user-guide/actor-animation/simulated-objects-17.png)

1. 在**Simulated Objects**面板中，选择模拟对象。

1. 在**Simulated Object Inspector**中，对**Collides with**，选择可用的碰撞体。这样，模拟物体的关节（流苏）就能与Actor的身体（脊柱和左臂）发生碰撞。

    ![Enable the tassel to collide with the actor's body during an animation.](/images/user-guide/actor-animation/simulated-objects-18.gif)

1. 在**Actor Manager**中，保存Actor。您可能需要等待资产处理器处理完您的更改。
