---
description: ' 在 Open 3D Engine中为Actor设置一个模拟对象，如流苏。 '
title: 设置模拟对象
---

在以下步骤中，为人物身上的流苏添加动作。

**设置模拟对象**

1. 在**Animation Editor**中，选择 **Layouts**，然后选择 **SimulatedObjects**。

1. 在**Skeleton Outliner**中，选择要添加到模拟对象中的骨骼。在搜索栏中输入名称或筛选结果。

1. 选择骨骼，右击，选择 **Simulated Object**, **Add selected joints**, **<New simulated object>**。

    `L_tassle_01_JNT` 骨骼及其子代被选中。

    ![Select bones to create a simulated object in the Animation Editor.](/images/user-guide/actor-animation/simulated-objects-1.png)

1. 在对话框中输入模拟对象的名称，然后点击 **OK**。

    ![Name your simulated object.](/images/user-guide/actor-animation/simulated-objects-2.png)

   模拟对象会出现在**Simulated Object**面板中。

    ![New simulated objects appear in the Simulated Object panel.](/images/user-guide/actor-animation/simulated-objects-3.png)

1. 在**Simulated Objects**面板中，选择一个对象或关节。您可以自定义的属性将显示在**Simulated Object Inspector**面板中。

    {{< note >}}
   创建动画图形后，可以预览模拟。目前，请保留默认值。
{{< /note >}}

   模拟对象链\(`L_tassle_01_JNT`\)中的第一个关节是**销钉**。钉住的关节会跟随原关节移动。如果有多个关节，如果不想让它们作为模拟对象移动，可以将它们钉住。根节点总是被固定的。

    ![Review the properties for the simulated object and specify whether to pin them.](/images/user-guide/actor-animation/simulated-objects-4.gif)

1. 在渲染窗口中，查看模拟对象中每个关节的碰撞半径。选择模拟对象查看所有碰撞体，或选择关节查看单个碰撞体。

    {{< tip >}}
在渲染窗口中，点击 ![Joint Collision Radius Icon](/images/user-guide/actor-animation/simulated-objects-5.png) 图标来开关碰撞体。

**Collider radius**是模拟对象关节避开模拟对象中指定的碰撞体的距离。如果该值为 `0`，则关节会停留在碰撞体表面。

完成下一部分后，您可以调整**Collider radius**，以获得自己喜欢的结果。
    {{< /tip >}}

    {{< video src="/images/user-guide/actor-animation/simulated-objects-5.mp4" info="View how the collider radius of each joint and how it moves with the animation of the actor." autoplay="true" loop="true" width="800" >}}

1. 要保存你的Actor，点击**Actor Manager**中的保存图标。

    ![Save your actor in the Actor Manager.](/images/user-guide/actor-animation/simulated-objects-6.png)
