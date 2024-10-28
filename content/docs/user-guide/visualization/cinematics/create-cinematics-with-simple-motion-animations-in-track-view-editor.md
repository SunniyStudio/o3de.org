---
description: null
title: 使用Simple Motion组件创建角色动画
draft: true
---

要使用轨迹视图创建角色动画，您可以将**Simple Motion**组件和**Actor**组件添加到实体中。然后，将该实体添加到轨迹视图序列中，并指定希望角色动画化的动作。将动作轨迹添加到轨迹视图序列中时，轨迹视图会在**Simple Motion**组件及其属性上驱动动画：
+ **Motion**
+ **Play speed**
+ **Play time**
+ **Blend in time**
+ **Blend out time**

更多信息，请参阅 **[Simple Motion](/docs/user-guide/components/reference/animation/simple-motion/)** 组件。

{{< note >}}
**Play speed** 属性始终设置为`0.0`。这是因为轨迹视图将设置每帧的**Play time**值，以驱动动作的回放。这样就可以在轨迹视图中进行刷屏和回放，也可以在游戏中进行回放。

将**Simple Motion**组件添加到轨迹视图序列时，**Preview in Editor**属性将自动启用。
{{< /note >}}

**在TrackView编辑器中添加Simple Motion组件**

1. 在 O3DE 编辑器中，右键单击视口并选择 **Create entity**。

1. 输入实体名称。

1. 在**Entity Inspector**中，单击**Add Component**，然后选择**Simple Motion**组件。

1. 添加 **[Actor](/docs/user-guide/components/reference/animation/actor/)** 组件。

1. 在**Actor**组件中，为**Actor asset**指定一个Actor文件。例如，可以指定`Jack.fbx`文件。
   **示例**

   您的实体应如下所示。

   ![Components for the entity to add to the track view sequence.](/images/user-guide/cinematics/cinematics-track-view-simple-motion-component-1.png)

1. 在 O3DE 编辑器中，选择**Tools**、**Track View**。

1. 点击**Add Sequence**图标![Add track view sequence icon](/images/user-guide/cinematics/cinematics-track-view-simple-motion-component-2.png)，输入序列名称，然后点击**OK**。

1. 在视口中选择实体，右键单击序列，然后选择**Add Selected Entity**。

   这将把实体及其组件添加到轨道视图序列中。

   ![Entity and components added to the track view sequence.](/images/user-guide/cinematics/cinematics-track-view-simple-motion-component-3.png)

1. 右键单击**Simple Motion**，选择**Add Track**，然后选择**Motion**选项。

1. 在时间轴上的**Simple Motion**，双击以创建到一个键。

1. 单击键，在**Key Properties**对话框中指定以下内容：

  1. 对于 **Motion**，单击文件夹图标并指定动作文件，例如 `jack_idle_to_walk.motion`。

  1. 指定所需的参数，如**Start Time**和**End Time**。

        {{< note >}}
     您可以设置 **Loop** 参数，这样只要设置了轨迹视图序列，动作就会持续播放。
{{< /note >}}

**示例**

![Key properties for the first motion.](/images/user-guide/cinematics/cinematics-track-view-simple-motion-component-4.png)

1. 点击播放图标![Play the track view sequence](/images/user-guide/cinematics/cinematics-track-view-simple-motion-component-5.png)，查看轨迹视图序列中的动画。您还可以在时间轴上拖动轨迹视图指针。
**示例**

   下面显示的是Actor从空闲到行走动作的动画效果。

   ![Example animation of a single motion in the track view sequence.](/images/shared/shared-cinematics-track-view-simple-motion-component-6-example.gif)

{{< note >}}
在轨迹视图中更新属性时，实体属性的原始值将被覆盖，不会恢复。例如，如果您在轨迹视图中将**Start time**设置为`3.0`，这将更新**Entity Inspector**中的**Simple Motion**组件属性。如果您想在轨迹视图序列完成后重新使用带有**Simple Motion**组件的实体，请更新**Play speed**参数；在轨迹视图中，**Play speed**参数总是重置为零。您也可以通过不重复使用实体来避免这一问题。
{{< /note >}}

## 在轨迹视图编辑器中混合动作

**Simple Motion**组件支持一次混合两个动作：当前播放的动作和之前播放的动作。例如，您可以混合两个动作，这样Actor就可以从行走动作平滑过渡到跑步动作。

您可以设置**Blend In Time**和**Blend Out Time**属性，以实现简单的动画混合。
+ **Blend In Time** - 指定所设置的动作完全融入所需的时间，从零到一秒不等。
+ **Blend Out Time** - 指定一个动作从一秒到零秒完全融合所需的时间。

要混合两个动作，可将两个动画重叠，并将上一个动作的**Blend Out Time**设置为`0.33`秒，将下一个动作的**Blend In Time**设置为`0.33`秒。这样，两个动作就能在轨迹视图序列中平滑地串联在一起。

{{< note >}}
如果您希望动画在游戏开始时启动，请单击**Edit Sequence**图标![Edit track view sequence icon](/images/user-guide/cinematics/cinematics-track-view-simple-motion-component-6.png)并在**Edit Sequence**对话框中选择**Autostart**，然后单击**OK**。更多信息，请参阅[设置序列属性](/docs/user-guide/visualization/cinematics/sequence-props)。
{{< /note >}}

**Blend In Time** 和 **Blend Out Time**参数会影响 DCC 中设置的骨骼重量。

**在轨迹视图编辑器中混合动作**

1. 双击第一个键，在**Blend Out Time**中输入`0.33`。这将允许第一个动作`jack_idle_to_walk.motion`与下一个动作混合。

**示例**

![Key properties for the first motion for blending.](/images/user-guide/cinematics/cinematics-track-view-simple-motion-component-7.png)

1. 在**Simple Motion**轨道上，双击时间线创建第二个键。

1. 再次双击该键，在**Key Properties**对话框中指定以下内容：

  1. 对于 **Motion**，单击文件夹图标并指定下一个动作，如 `jack_strafe_run_forwards.motion` 文件。

  1. 指定所需的参数，如**Start Time**和**End Time**。在**Blend In Time**中输入`0.33`。这样就有足够的时间与前一个动作重叠。
**示例**


![Key properties for the second motion for blending.](/images/user-guide/cinematics/cinematics-track-view-simple-motion-component-8.png)

1. 选择并拖动第二个键，使其与第一个动作轨迹重叠。

1. 重复步骤 2 至 4，添加其他动作。对于即将结束的动作，输入 `0.33`，以**Blend Out Time**。对于下一个开始的动作，输入 `0.33` 作为 **Blend In Time**。
**示例**

   下面是一条混合了四个动作的时间线。

   ![Timeline in a track view sequence with four motions.](/images/user-guide/cinematics/cinematics-track-view-simple-motion-component-9.png)

1. 单击播放图标可查看轨道视图序列。您还可以在时间轴上拖动轨迹视图指针。动作会在轨迹视图序列中融合在一起。
**示例**

![Example of blending motions together in the track view sequence.](/images/user-guide/cinematics/cinematics-track-view-simple-motion-component-10.gif)
