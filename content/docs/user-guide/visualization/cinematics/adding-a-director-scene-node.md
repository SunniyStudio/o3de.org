---
description: ' 在Open 3D Engine中的<guilabel>Track View</guilabel>中添加 Director (Scene) 节点 '
title: 添加 Director (Scene) 节点
---

要确定序列要使用的摄像机，您必须添加一个 [Director (Scene) 节点](/docs/user-guide/visualization/cinematics/track-view/nodes-director/)。

**在Track View中添加 Director 节点**

1. 在 O3DE 编辑器中，选择 **Tools**, **Track View**。

1. 在Track View中，选择或创建序列。

1. 完成以下之一：
   + 右击序列并选择 **Add Director (Scene) Node**。
   + 点击 **Add Director Node** 图标。

   ![Add the Director node in the Track View to manage your track view sequence.](/images/user-guide/cinematics/cinematics-track-view-editor-adding-director-node-1.png)

默认情况下，**Camera**轨道出现在**Director (Scene) Node**上，用于控制哪个摄像机在序列中处于活动状态。这是分配和切换摄像机所需的唯一轨道。

有关可添加的其他轨道的更多信息，请参阅 [Director (Scene) Node](/docs/user-guide/visualization/cinematics/track-view/nodes-director/)。

![Add the Camera track in the Track View](/images/user-guide/cinematics/cinematics-track-view-editor-adding-director-node-2.png)

在**Camera**轨道上添加动画键时，可以从**Key Properties**窗格中选择该键并指定哪个摄像机应处于活动状态。设置摄像机后，您可以在轨道上看到摄像机的名称。

![Add keyframes for the Director node in the Track View to manage your camera.](/images/user-guide/cinematics/cinematics-track-view-editor-adding-director-node-3.png)

要在序列中创建一系列从一个摄像机到另一个摄像机的跳切，可在**Director**节点的**Camera**轨道上添加动画键。**Director** 节点使用为**Camera**轨道设置的键属性来决定切换哪个摄像机。
