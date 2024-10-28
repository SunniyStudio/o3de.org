---
description: ' 在开放式 3D 引擎动画编辑器中，将动作和动画从一个Actor重定向到另一个Actor。 '
title: 重定向动作
---

使用 “动画编辑器 ”可将一个Actor的动作重定向到另一个Actor。这样，您就可以重复使用为某个Actor创建的动作，并在等待创建新动作的同时，快速为其他Actor创建动作原型。重定向功能对于确保适当缩放Actor也很重要。如果没有这项功能，重定向后的Actor将按照录制动作时原始Actor的尺寸进行拉伸和缩放。使用该功能后，重定向Actor将保持其大小。

例如，你可能有一个六英尺高的人类角色，而你的动作是为该Actor录制的。您也可能有一个 18 英尺高的巨人角色。如果在不重新定位的情况下，在巨型角色上播放人类角色的动作，巨型角色将缩放至人类角色的大小。启用重定向功能后，巨人角色将保持 18 英尺的高度。

{{< important >}}
要重定向动作，您的资产**必须**满足以下要求：

+ **每个动作的第一帧**必须是Actor的绑定姿势。例如，如果您的动画包含 1-100 帧，则必须在第 0 帧添加Actor的绑定姿势（蒙皮网格和骨骼）。

+ 原始Actor和重定向Actor的**骨骼名称和层次结构**必须相同。被重定向的Actor**可能**在其层次结构中包含额外的叶状骨骼，但被重定向的层次结构区域必须完全相同。
{{< /important >}}

{{< note >}}
如果重定向Actor的骨骼名称和变换与原始Actor相同，则无需使用重定向动作过程。
{{< /note >}}

**重定向动作**

1. 在 DCC 工具中，将Actor的绑定姿势（包括皮肤和骨骼）添加到动作的第一帧。然后将文件导出到 O3DE。

1. 打开 O3DE 编辑器。

1. 在**Asset Browser**中，右键单击导入的文件，然后选择**Edit Settings**。

1. 在**Fbx Settings**窗口的**Motions**选项卡上，选择**Add Modifier**，**Motion range**。

    {{< note >}}
   如果没有看到**Motion range**修改器，请确保您的`.fbx`文件具有关键帧。
{{< /note >}}

1. 在**Motion range**下，执行以下操作：

  1. 将**Start frame**设置为不包含绑定姿势的帧。例如，如果原始Actor的绑定姿势位于第 0 帧，则将**Start frame**设置为 1。 这样可以确保绑定姿势不会在您的动作中播放。

  1. 将**End frame**设置为动作的最后一帧。根据上一步的示例，最后一帧将是 100。

  1. 点击**Update**。
   
    {{< note >}}
    如果出现错误，请检查动作中关键帧的数量，并更新**End frame**。
{{< /note >}}

帧的设置如下图所示：

   ![Specify the motion range keyframe for your actor's motion in the O3DE Animation Editor](/images/user-guide/actor-animation/retarget-animations-fbx-settings-motion-range-modifier.png)

1. 对所有要重定位的动作重复步骤 1 至 5。

1. 在 O3DE 编辑器中，选择 **Tools**, **Animation Editor**。

1. 在**动画编辑器**的中心窗格中，在**Anim Graph**选项卡上，打开你的[动画图表](/docs/user-guide/visualization/animation/animation-editor/animation-graph-user-interface/).

1. 在右侧面板中，在**Attributes**标签页中，选择 **Retarget** 多选框。

    ![Select the Retarget check box for your animation graph in the O3DE Animation Editor](/images/user-guide/actor-animation/retarget-animations-attributes-retarget-checkbox.png)

    {{< note >}}
   您还可以在**Motion**、**Blend Space 1D** 和**Blend Space 2D** 节点上切换重定向功能。
{{< /note >}}

1. 在**Anim Graph**选项卡上，保存您的动画图形。

1. 现在要在不同的Actor身上使用您的动作和动画图：

  1. 在**动画编辑器**中，选择**File**，**Open Actor**，然后选择要重新瞄准的Actor。

  1. 在中心窗格的**Motion Sets**选项卡上，打开包含之前修改过的动作的动作集。

  1. 在中心窗格的**Anim Graph**选项卡上，打开之前保存的动画图。

  1. 确认具有重定向动作和动画图的Actor的行为与原始Actor相似。
