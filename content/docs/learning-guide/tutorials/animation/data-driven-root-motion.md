---
linkTitle: 根运动
title: 数据驱动的根运动
description: 了解数据驱动的根运动、如何从动画中提取根运动，以及如何在 Open 3D Engine （O3DE） 中为角色启用根运动。
weight: 100
toc: true
---

根运动是移动角色实体的动画。本教程详细介绍了什么是根运动，以及如何为角色启用它。

在本教程中，您需要一个角色资源，其中包含已由 [Asset Processor](/docs/user-guide/assets/asset-processor/) 处理的动画。动画应将 actor 移离场景原点。也就是说，动画应沿某个方向移动 actor，例如在空间中向前移动 actor 的行走循环。

## 什么是根运动？

角色实体通常具有许多需要一起移动的组件（网格、骨架、碰撞器等）。如果角色跳跃，则角色的碰撞胶囊体需要应用相同的跳跃运动，以便它与骨架和网格一起移动。这个移动 actor 的所有组件的移动就是 *根运动*。

根运动可以是 *代码驱动* 或 *数据驱动* 的。本教程的重点是数据驱动的根运动，但了解这两种方法会很有帮助。

### 代码驱动

对于代码驱动的根运动，角色的动画循环就地发生，并且离开场景原点的任何移动都由代码（例如脚本）提供。角色在原地奔跑、行走或跳跃，脚本提供适当的定向运动。

以下影片演示了为代码驱动的根运动创建的前向运行循环：

{{< video src="/images/learning-guide/tutorials/animation/jack-run-no-root-motion.mp4" info="Example run cycle without root motion." autoplay="true" loop="true" poster="/images/learning-guide/tutorials/animation/jack-run-poster.png" >}}

请注意，角色在原地运行。在运行时，当收到前向输入事件时，脚本会播放前面的前向运行周期，并根据某些表达式或函数的结果将参与者实体向前移动。

### 数据驱动

对于数据驱动的根运动，将提供运动数据，以将角色移离场景原点。数据通常是动画制作器提供的关键帧动画。以下影片演示了为数据驱动的根运动创建的前向运行循环：

{{< video src="/images/learning-guide/tutorials/animation/jack-run-root-motion.mp4" info="Example run cycle with root motion." autoplay="true" loop="true" poster="/images/learning-guide/tutorials/animation/jack-run-poster.png" >}}

请注意，Actor 在运行时会向前移动。在运行时，当收到 forward input 事件时，将播放 forward run cycle。来自角色骨架根部的动画数据驱动角色实体的向前移动。

{{< note >}}
前面的示例影片将角色的骨骼显示为绿线。从地平面延伸到角色骨盆的绿线是骨架的根骨骼。在理想情况下，角色资源应该具有此根骨骼，并且任何将角色移出场景原点的动画都应应用于此根骨骼。
{{< /note >}}

### 您应该使用哪个？

您使用代码驱动的根运动还是数据驱动的根运动取决于您的项目需求。

在游戏机制的精确角色移动比视觉性能更重要的情况下，代码驱动的根运动可能更可取。例如，平台游戏和格斗游戏通常需要精确的 actor 移动。确保 actor 根据输入移动精确的距离可能比确保 actor 相对于动画的移动在视觉上准确要重要得多。

数据驱动的根运动可以让动画师更好地控制角色的表演。追求真实感和沉浸感的冒险游戏和第一人称游戏通常需要演员的动作表达一些游戏条件或情感。在这些情况下，角色相对于动画的移动可能会优先考虑视觉准确性。

本教程的其余部分重点介绍数据驱动的根运动。您将学习如何确定角色是否正确设置了数据驱动的根运动，根据需要提取根运动，以及在 [Animation Editor](/docs/user-guide/visualization/animation/animation-editor/user-interface) 中启用根运动。

## 根运动设置

数据驱动根运动中的数据通常是动画师应用于角色骨架根骨骼的关键帧动画。在理想情况下，角色和动画源场景文件遵循以下规则：

* 骨架在场景原点处放置了根骨骼。
* 根骨骼连接到角色的骨盆骨骼（或非 Biped 角色上的某个相对骨骼）。
* 任何将角色移离场景原点的运动都会在根骨骼上设置关键帧。

### 检查 actor 设置

要确定角色是否具有带动画的根骨骼，请执行以下操作：

1. 在 **O3DE 编辑器 **中，从 **Tool** 菜单中选择 **Animation Editor**。

1. 在 Animation Editor 中，按 **Ctrl + O** 打开角色。从文件窗口中选择一个角色。

1. 选择视口上方的 {{< icon "visibility-on.svg" >}} **Visibility** 按钮，并从列表中选择 **Line skeleton**，以将角色的骨架显示为绿线。

1. 在**Motion Sets**标签页中，在**Motion Set**组中，选择{{< icon "file-folder.svg" >}} **Folder** 按钮添加动作。从文件窗口中选择一个动作。

1. 单击您刚刚添加到 Motion Set 的动作以将其选中。

1. 在**Time View** 标签页中，点击 {{< icon "play.svg" >}} **Play** 按钮播放动画。

在播放动画时检查动画。如果骨架具有随角色移动的根骨骼（从地平面延伸到角色骨盆的绿线），则角色具有带动画的根骨骼。在以下示例中，请注意表示根骨骼从地面延伸并随角色移动的绿线：

{{< video src="/images/learning-guide/tutorials/animation/jack-run-root-motion.mp4" info="Example run cycle with root motion." autoplay="true" loop="true" poster="/images/learning-guide/tutorials/animation/jack-run-poster.png" >}}

下表包含上述步骤的可能结果，并提供了每个结果的操作过程：

| 结果 | 下一步 |
| --- | --- |
| 角色在原地进行动画处理。 | 此角色可能没有根骨骼，也没有根运动的动画数据。您可以：<ul><li>将根骨骼和动画添加到动画应用程序中的源场景文件</li><li>改用代码驱动的根运动</li></ul> |
| 角色离开场景原点，但表示根骨骼的绿线的一端仍位于场景原点。 | 此 actor 没有根骨骼。动画数据将应用于另一个骨骼（例如骨盆）。您可以：<ul><li>将根骨骼和动画添加到动画应用程序中的源场景文件</li><li>尝试 [提取根运动](#root-motion-extraction)，如下一节所述</li></ul> |
| 角色从场景原点移开，根骨骼随角色移动，并且当动画循环完成时，角色将捕捉回场景原点。 | 角色具有包含动画数据的根骨骼。您可以继续 [启用根运动](#enable-root-motion) 部分。 |

{{< important >}}
为了获得最佳效果，请在数字内容创建应用程序（例如 Blender 或 Maya）中编辑源场景文件，并添加带有动画的根骨骼。
{{< /important >}}

### 根运动提取

如果源场景文件具有将角色移出场景原点的动画，但根骨骼不随角色移动，则源场景文件可能没有根骨骼，并且该动画已应用于另一个骨骼，例如骨盆骨骼。您可以尝试从骨盆中提取动画，以便将其作为根运动应用于角色。

要从动画中提取根运动，您必须使用 [Scene Settings](/docs/user-guide/assets/scene-settings/) 工具自定义动画的处理方式。要使用 **根运动提取** 修饰符创建自定义处理规则，请执行以下操作：

1. 在 O3DE Editor 的 **Asset Browser** 中找到包含要从中提取根运动的动画的 `.fbx` 文件。

1. 在 Asset Browser 中，右键单击动画文件，然后从上下文菜单中选择 **Edit settings...**。

1. 在 Scene Settings 中，选择 **Motions** 选项卡。

1. 选择 **Add Modifier**，然后从修饰符列表中选择 **Root motion extraction**。

    {{< note >}}
源场景文件可能包含多个运动。您需要为每个需要它的运动添加根运动提取修改器。
    {{< /note >}}

1. 在 根运动提取 修改器中，确保 **Sample joint** 属性设置为包含要提取的运动的骨骼，如以下示例所示：

    {{< image-width src="/images/learning-guide/tutorials/animation/root-motion-extraction-modifier-bone.png" alt="Selecting the bone for root motion extraction in Scene Settings." >}}

1. 默认情况下，将提取骨骼的平移。您可以选择忽略 X 轴或 Y 轴平移，并通过在修改器中设置属性来提取旋转。

1. 配置修饰符后，选择 **Update** 以保存您的自定义处理规则。动画由 Asset Processor 自动处理。关闭输出窗口并退出 Scene Settings （场景设置）。

处理运动时，将提取根运动并将其应用于运动产品资产中的根骨骼。您可以通过重复 [检查 actor setup](#check-actor-setup)  部分中的步骤来验证根运动提取是否成功。

下表提供了有关 Root motion extraction （根运动提取） 修改器提供的选项的其他详细信息：

| 选项 | 说明 |
| - | - |
| **Sample Joint** | 从中提取根运动的骨骼。通常，这是髋部或骨盆（或者对于非 Biped 角色的某个相对骨骼）。 |
| **Ignore X-Axis transition** | 启用后，不会提取 X 轴平移。根运动的 X 平移设置为`0.0`. |
| **Ignore Y-Axis transition** | 启用后，不会提取 Y 轴平移。根运动的 Y 平移设置为`0.0`. |
| **Rotation extraction** | 启用后，还会提取根运动的旋转。 |
| **Smoothing method** | 选择要应用于运动数据的平滑方法。平滑数据可减少任何剧烈的位置或旋转变化。 |
| **Smooth position** | 启用后，将平滑根运动动画数据的平移。 |
| **Smooth rotation** | 启用后，将平滑根运动动画数据的旋转。 |
| **Smooth frame number** | 每个数据点应用于应用平滑的样本数（以帧为单位）。值越大，动画数据通常越平滑。 |

## 启用根运动

将根运动应用于角色的根骨骼后，必须按照以下步骤为角色启用根运动：

1. 在 Animation Editor 中，按 **Ctrl + O** 打开角色。从文件窗口中选择一个角色。

1. 在**Motion Sets**标签页中，在 **Motion Set** 组中，点击 {{< icon "file-folder.svg" >}} **Folder** 按钮添加动作。从文件窗口中选择包含根运动的运动。

1. 选择您刚刚添加到 Motion Set 的运动以启用它。

1. 在**Actor Manager**标签页中，单击 Actor 实例以将其选中。

1. 在**Actor Properties**面板中，确保在 **Motion extraction joint** 属性中选择根骨骼。 如果您选择 **Find best match**，Animation Editor 将尝试选择适当的骨骼。

    {{< image-width src="/images/learning-guide/tutorials/animation/enable-root-motion-bone.png" alt="Enable root motion for the actor in Animation Editor." >}}

1. 按 **Ctrl + S** 保存角色。

如果成功启用根运动，您将看到角色重复动画循环，而不会弹回场景原点。这意味着角色的根移动由动画数据驱动。

{{< video src="/images/learning-guide/tutorials/animation/enable-root-motion.mp4" info="Example run cycle with root motion." autoplay="true" loop="true" poster="/images/learning-guide/tutorials/animation/enable-root-motion-poster.png" >}}
