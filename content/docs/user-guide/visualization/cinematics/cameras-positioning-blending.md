---
description: ' 在 Open 3D Engine 的<guilabel>Track View</guilabel>编辑器中移动摄像机。 '
title: 放置 Camera
---

您可以控制摄像机的位置和旋转，从而在序列中操控镜头的朝向。这些都可以随着时间的推移而动画化，使摄像机充满活力。如果有多个摄像机，还可以控制摄像机是立即切换，还是随着时间的推移而混合。

## 动画摄像机

作为推荐的工作流程，先进入录制模式，然后从特定摄像机的视角查看摄像机的动画。更多信息，请参阅 [使用录制模式](/docs/user-guide/visualization/cinematics/using-record-mode/)。

{{< note >}}
您仍然可以通过默认的编辑器摄像机来操作关卡中的组件实体摄像机。不过，这种方法会增加确定摄像头朝向的难度。
{{< /note >}}

**示例**

1. 在关卡中，您有一个组件实体，上面附有一个**Camera**组件。该组件实体是序列的一部分。

1. 默认情况下，附加到该实体的**Transform**组件在序列中具有**Position**和**Rotation**轨迹。

![Create animation keys in the timeline for a sequence for a camera entity.](/images/user-guide/cinematics/cinematics-track-view-editor-using-record-mode-4.png)

1. 要保持游戏对象的当前位置和旋转，请在时间轴上的 `0` 处为两个轨道添加动画键。

![Create animation keys in the timeline for a sequence for a camera entity.](/images/user-guide/cinematics/cinematics-track-view-editor-using-record-mode-5.png)

1. 选择并拖动播放头至时间线上的不同位置。

1. 将轨迹视图窗口移至一侧或停靠，使其处于打开状态，但不会阻挡当前打开的关卡的视口

1. 单击**Start Animation Recording**图标进入录制模式。

1. In the left-corner of the viewport window, right-click **Perspective** and choose **Camera** and then specify the camera that is part of your sequence. The text will change from **Perspective** to **Camera entity: *NameOfYourCamera***.

1. While you are viewing from the perspective of the camera, as you move in the level, the camera moves as well. This includes any rotations. In record mode, this adds animation keys at the current playhead location.

1. 在视口窗口的左角，右键单击**Perspective**并选择**Camera**，然后指定序列中的摄像机。文本将从 **Perspective** 变为 **Camera entity: *NameOfYourCamera***。

1. 当您从摄像机的视角观看时，随着您在关卡中的移动，摄像机也会移动。这包括任何旋转。在录制模式下，这会在当前播放头位置添加动画键。

![Create animation tracks for a Camera component in the timeline for a sequence.](/images/user-guide/cinematics/cinematics-track-view-editor-using-record-mode-6.png)

1. 当您需要在时间轴上添加另一个动画键时，移动播放头，然后在关卡中移动摄像机视图。

1. 完成后，再次单击**Start Animation Recording**图标退出录制模式。

## 混合摄像机

默认情况下，在摄像机之间切换时，变化是即时的。不过，如果您想要更平滑的过渡，可以将摄像机混合在一起。你可以将一个摄像机混入或混出由摄像机控制的序列，也可以将序列中的摄像机混入或混出序列。

### 从游戏摄像机混合到序列摄像机

**使用默认游戏摄像头**

1. 在 **Director** 节点下，为 **Camera** 轨道在时间轴上添加一个动画键。

1. 双击动画键并确认**Camera**属性已设置为**None**。

1. 在时间轴上为**Camera**轨道添加另一个键。您可以在序列的开头或结尾添加关键帧，也可以同时添加。

1. 双击摄像机切换前的动画键，并为**Blend time**指定一个以秒为单位的值。该值将决定当前摄像机与下一个摄像机的混合方式。

    {{< note >}}
   这将在游戏摄像机和序列摄像机之间或从序列摄像机返回到游戏摄像机之间创建一个混合效果，具体取决于您按键的位置和调整的 **Blend time**。
{{< /note >}}

### 在序列中混合摄像机

混合摄像机键可将当前摄像机的位置、旋转和视场混合到**Camera**轨道上的下一个摄像机中。这样可以使剪辑看起来像一个连续的单镜头运动，而不是突然的跳切。

**创建混合摄像机键**

1. 选择混合的第一个摄像机的关键帧。

1. 双击该键并执行以下操作：

   1. 对于 **Camera**, 选择要启动的摄像机。

   1. 对于 **Blend time**，指定一个大于 `0`的值。这是以秒为单位的混合时间。
**示例**

      如果将第一个摄像机添加为轨迹视图节点，那么在使用混合序列摄像机时，必须为 **Position**、**Rotation** 和 **Field of View**轨迹添加至少一个动画键。

      在此序列中，**Camera**轨道从**CinematicsCamera**的`0`秒开始，并在 `2`秒时与**Camera5**融合。

      ![Create animation tracks for a Camera component in the timeline for a sequence.](/images/user-guide/cinematics/cinematics-track-view-editor-blending-cameras-in-sequences.png)
