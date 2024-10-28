---
description: ' 在Open 3D Engine的Track View编辑器中对序列使用录制模式。'
title: 使用录制模式
---

轨迹视图的**Play**工具栏上有一个**Start Animation Recording**图标，用于进入录制模式。在录制模式下，您可以直接在视口中操作序列的组件实体。动画键会根据时间线播放头的当前位置，自动放置在时间线上相应的节点轨道上。

**使用记录模式**

1. 在轨道视图中，选择或创建所需的序列。

1. 在**Play**工具栏中，单击**Start Animation Recording**图标。

1. 在视口中，操作序列中的组件实体。您的更改将以动画键的形式出现在时间轴上。

1. 要停止录制，请再次单击**Start Animation Recording**图标。

    {{< note >}}
您必须将播放头移动到时间轴上的不同位置，动画才能随时间播放。否则，当您操作关卡中的组件实体时，会覆盖同一时间线上位置的按键。

您还可以使用**Start Auto Recording**选项。选择该选项后，播放头会在指定时间沿时间线自动移动。默认值为一秒。这意味着播放头每一秒向前跳过一次。您可以指定一个较低的值，这意味着播放头设置按键的频率更高。不过，这可能会导致在录制完成后必须修改很多键。
{{< /note >}}

**开始自动录制**

1. 在Track View中，选择或创建所需的序列。

1. 在**Play**工具栏中，单击**Start Auto Recording**图标。播放头会根据下拉菜单中指定的设置在时间轴上自动移动。

1. 在视口中，操作序列中的一个组件实体。当播放头移动并对组件实体进行更改时，您的更改会以动画键的形式出现在时间轴上。

1. 要停止自动录制，请再次单击**Start Auto Recording**图标。

**示例 1： 记录组件实体变化**
下面的示例使用了**Transform**组件，但同样的想法也适用于序列中指定为动画键的任何组件轨道。在记录模式下，每次更改组件实体时都无需手动设置键。

1. 在关卡中，您有一个属于序列一部分的组件实体。

1. 默认情况下，组件实体上的 **Transform** 组件已经在序列中为 **Position** 和 **Rotation** 属性设置了轨道。

![Position and Rotation properties in the timeline for a sequence.](/images/user-guide/cinematics/cinematics-track-view-editor-using-record-mode-1.png)

1. 为了保持游戏对象的当前位置和旋转，请在时间轴的`0`处为两个轨道添加动画键。

![Create animation keys in the timeline for a sequence.](/images/user-guide/cinematics/cinematics-track-view-editor-using-record-mode-2.png)

1. 选择并拖动播放头到时间线上的不同位置。此示例将播放头移动到`1`秒。

1. 将轨道视图窗口移至一侧或停靠，使其仍处于打开状态，但不会阻挡当前打开的水平视口。

1. 单击**Start Animation Recording**图标进入录制模式。

1. 在视口或**Entity Outliner**中，选择组件实体。

1. 使用**Transform**工具将实体向后移动到关卡中。您也可以在 **Transform** 组件属性中输入不同的值。

1. 查看序列的时间轴。新的按键会以`1`秒的速度出现在**Position**轨道上。

![View the newly added key in the timeline for a sequence.](/images/user-guide/cinematics/cinematics-track-view-editor-using-record-mode-3.png)

1. 将播放头移到要设置另一个动画键的位置，然后再次调整属性值。

1. 完成后，再次单击**Start Animation Recording**图标退出录制模式。

**示例 2： 从摄像机视图记录摄像机运动**
下面的示例展示了当您从指定摄像机的角度在关卡中定位摄像机时，如何在序列中自动添加动画键。

1. 在关卡中，您有一个组件实体，其**Camera**组件是序列的一部分。

1. 默认情况下，组件实体上的 **Transform** 组件在序列中具有 **Position** 和 **Rotation** 属性的轨迹。

![Rotation and Position tracks in the timeline for a sequence.](/images/user-guide/cinematics/cinematics-track-view-editor-using-record-mode-4.png)

1. 要保持游戏对象的当前位置和旋转，请将两个轨道上的动画键指定为时间轴上的`0`。

![Create animation keys in the timeline for a sequence for a camera entity.](/images/user-guide/cinematics/cinematics-track-view-editor-using-record-mode-5.png)

1. 将播放头移动到时间线上的不同位置。

1. 将轨迹视图窗口移至一侧或停靠，使其仍处于打开状态，但不会挡住当前打开的关卡的视口。

1. 单击**Start Animation Recording**图标进入录制模式。

1. 在视口窗口的左角，右键单击**Perspective**并选择**Camera**，然后指定序列中的摄像机。文本将从 **Perspective** 变为 **Camera entity: *NameOfYourCamera****。

1. 当您从摄像机的视角观看时，当您在关卡中移动时，摄像机也会移动。这包括任何旋转。在录制模式下，这会在当前播放头位置添加动画键。

![Create animation tracks for a Camera component in the timeline for a sequence.](/images/user-guide/cinematics/cinematics-track-view-editor-using-record-mode-6.png)

1. 当您需要在时间轴上设置另一个动画键时，移动播放头，然后在关卡中移动摄像机视图。

1. 完成后，再次单击**Start Animation Recording**图标退出录制模式。
