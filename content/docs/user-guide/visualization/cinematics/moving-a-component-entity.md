---
description: ' 在 Open 3D Engine 的<guilabel>Track View</guilabel>编辑器中移动序列的组件实体。 '
title: 移动序列中的组件实体
---

每个组件实体都有一个**Transform**组件。默认情况下，将组件实体添加到序列中时，**Position**和**Rotation**轨迹会自动出现。你可以操作序列中组件实体的**Position**和**Rotation**属性。如果想调整组件实体的整体大小，还可以添加 **Scale** 轨道。

每个轨道都有一个用于**XYZ**属性的子轨道，这样你就可以对每个轴进行动画操作。

![Transform properties in a sequence.](/images/user-guide/cinematics/cinematics-track-view-editor-moving-component-entity-1.png)

您可以根据需要为每个音轨添加任意数量的按键，并使用 ****Curve Editor**** 调整过渡效果。

更多信息，请参阅 [使用动画曲线](/docs/user-guide/visualization/cinematics/track-view/editor-animation-curves/)。

移动组件实体时，我们建议采用以下工作流程：
+ 在关卡中操纵组件实体，然后手动添加动画键。

  更多信息，请参阅 [在轨道上添加和删除动画键](/docs/user-guide/visualization/cinematics/adding-removing-animation-keys-on-tracks/)
+ 使用记录模式设置按键。有关更多信息，请参阅 [使用记录模式](/docs/user-guide/visualization/cinematics/using-record-mode/)。

**使用记录模式设置Transform键**

1. 在Track View中，创建或选择序列并添加组件实体。请参阅 [添加组件实体](/docs/user-guide/visualization/cinematics/adding-component-entities/)。

1. 默认情况下，附加到实体的 **Transform** 组件会自动将 **Position** 和 **Rotation** 轨道添加到序列中。

![Position and Rotation properties in the timeline for a sequence.](/images/user-guide/cinematics/cinematics-track-view-editor-using-record-mode-1.png)

1. 要保持游戏对象的当前位置和旋转，请在时间轴的`0`处为两个轨道添加一个动画键。

1. 选择并拖动播放头到时间轴上的不同位置。

1. 将轨道视图窗口移至一侧或停靠，使其仍处于打开状态，但不会挡住当前打开关卡的视口。

1. 单击**Start Animation Recording**图标进入录制模式。

1. 在视口或**Entity Outliner**中，选择组件实体。

1. 使用平移工具将实体向后移动到图层中。您也可以在 **Transform** 组件属性中输入值。

1. 查看序列的时间轴。新键出现在 **Position** 轨道上的 `1` 秒位置。

![View the newly added key in the timeline for a sequence.](/images/user-guide/cinematics/cinematics-track-view-editor-using-record-mode-3.png)

1. 将播放头移到要设置另一个动画键的位置，然后再次调整属性值。

1. 完成后，再次单击**Start Animation Recording**图标退出录制模式。
