---
description: ' 使用轨迹视图编辑器在 Open 3D Engine中创建和管理电影序列。'
title: 使用Track View编辑器
---

轨迹视图是创建和管理电影序列的主要工具。一个  **[sequence](/docs/user-guide/appendix/glossary#sequence)**是从轨迹视图中生成的内容，用于剪辑或其他预制动画触发。创建序列时，会在关卡中创建一个组件实体。该组件实体存储了您在轨迹视图中指定的所有动画关键数据。

如果您想为游戏生成剪辑或创建脚本来触发动画，您可以使用轨迹视图来控制摄像机、组件实体、关卡中的全局变量等。

**在音轨视图中创建序列**

1. 执行以下操作之一：
   + 在 O3DE 编辑器中，选择 **Tools**, **Track View**。
   + 按下 **T**。

1. 要创建序列，请执行以下操作之一：
   + 选择 **Sequences**, **New Sequence**。
   + 点击 **Add Sequence** 图标 ![Add track view sequence icon](/images/user-guide/cinematics/cinematics-track-view-simple-motion-component-2.png).

1. 输入序列名称，如*Example Sequence*，然后单击**OK**。

1. 在**Entity Outliner**中，会出现一个与序列名称相同的组件实体。该组件实体有一个 **Sequence** 组件，用于存储来自轨迹视图的序列数据。

![Sequence component entity in the Entity Outliner.](/images/user-guide/cinematics/track-view-editor-sequence-entity.png)

创建序列后，可以为其添加属性。序列的任何部分都被视为**节点**。节点可以是对现有组件实体的引用，也可以添加到序列中。例如，如果您想在序列中加入一个活动摄像机，您可以添加**导演**节点。根据动画序列的不同，每个节点可以有一个或多个轨道。轨道*在时间轴上显示与节点上动画属性相关的动画键。

**在序列中添加节点**

1. 在轨迹视图中，右键单击序列或节点浏览器，然后选择 **Add *<Name\>* Node**。

1. 选择节点以更新其属性。

   要了解更多信息，查看 [Track View 编辑器节点](/docs/user-guide/visualization/cinematics/trackview-nodes/).

**主题**
+ [Track View 编辑器工具栏](/docs/user-guide/visualization/cinematics/track-view/editor-toolbars/)
+ [Track View 编辑器节点](/docs/user-guide/visualization/cinematics/trackview-nodes/)
+ [在轨道上添加和移动动画关键帧](/docs/user-guide/visualization/cinematics/adding-removing-animation-keys-on-tracks/)
+ [控制播放头](/docs/user-guide/visualization/cinematics/controlling-the-playhead/)
+ [使用录制模式](/docs/user-guide/visualization/cinematics/using-record-mode/)
+ [使用动画曲线](/docs/user-guide/visualization/cinematics/track-view/editor-animation-curves/)
