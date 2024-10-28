---
description: ' 了解 Open 3D Engine 如何支持电影和脚本事件序列。 '
linktitle: 影片
title: 创建电影序列
---

电影动画，也称为序列或场景，是一种交互式电影动画，可对对象和事件进行随时间变化的控制。您可以使用 O3DE 为游戏添加场景。

您还可以添加脚本事件，以便在游戏中触发一系列对象、动画和声音。玩家可以从自己（第一人称）或他人（第三人称）的视角查看这些序列。

序列由以下元素组成（按层次顺序排列），通过Track View创建和管理：
+ **Node** - 每个序列包括一个顶级director（场景）节点、一个或多个摄像机节点、图像效果节点和实体节点。
+ **Track** - 根据类型的不同，每个节点都由多个轨道组成，如位置、动画、声音、灯光、文本和事件。轨迹显示在轨迹时间线窗格中。
+ **Key** - 键是特定时间的属性设置。在播放序列时，键会根据在Track View中设置的出入切线值进行插值。

**主题**
+ [使用Track View 编辑器](/docs/user-guide/visualization/cinematics/track-view/)
+ [填充场景](/docs/user-guide/visualization/cinematics/populating-a-scene/)
+ [为场景中的角色制作动画](/docs/user-guide/visualization/cinematics/animation-intro/)
+ [采集图像帧](/docs/user-guide/visualization/cinematics/image-capture/)
+ [使用控制台变量调试电影场景](/docs/user-guide/visualization/cinematics/debugging/)
