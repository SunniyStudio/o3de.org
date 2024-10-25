---
description: ' 在 Open 3D Engine 动画编辑器中设置布娃娃。 '
title: 设置布娃娃
weight: 100
---

{{< important >}}
目前无法自动防止物理几何**PhysX Character Controller**组件与**PhysX Ragdoll**组件碰撞，这可能导致布娃娃出现意外行为。可以通过使用[碰撞过滤](/docs/user-guide/interactivity/physics/nvidia-physx/configuring/configuration-collision-layers/)或在激活布偶时禁用**PhysX Character Controller** 组件上的物理功能来避免碰撞。
{{< /important >}}

要设置布娃娃，请执行以下操作：
+ [为布娃娃定义关节](#step-1-define-joints-for-the-ragdoll)
+ [设置布娃娃碰撞体](#step-2-set-up-ragdoll-colliders)
+ [创建关节限制](#step-3-create-joint-limits)

此外，你的演员必须有一个动作提取节点。您的布偶必须有一个根节点，它是动作提取节点的直接子节点。例如，Rin 角色的动作提取节点使用`root`。对于布偶根节点，Rin 角色使用的是 `C_pelvis_JNT`，它是 `root` 的子节点。

在进行布偶设置时，显示或隐藏**Atom Render Window**中的某些元素可能非常有用。您可以使用 “渲染选项 ”下拉菜单，该菜单以![Use the render options in the Atom Render Window to show or hide elements such as ragdoll colliders, joint limits and hit detection colliders](/images/user-guide/actor-animation/ragdoll-skeleton-render-options.svg)图标表示，以显示或隐藏**Ragdoll Colliders** (rendered in orange), **Ragdoll Joint Limits**, **Hit Detection Colliders** (rendered in blue), **Cloth Colliders** (rendered in purple), **Line Skeleton** 和其他元素。

## 第 1 步: 为布娃娃定义关节

按以下步骤为布偶选择关节。通常情况下，您希望布偶中的关节数量大大少于动画骨骼中的关节数量，因为模拟指骨等小关节通常不会产生什么益处，反而会产生性能成本。

**为布偶选择关节**

1. 在 O3DE 编辑器，选择 **Tools**, **Animation Editor**。

1. 在 **Animation Editor**中，在菜单栏右侧，从下拉菜单中选择 **Physics** 。这将改变布局。

1. 选择 **File**, **Open Actor** ，并选择你的Actor。

1. 在 **Skeleton Outliner** 中，多重选择您想包含在布偶中的关节。您也可以通过左键单击**Atom 渲染窗口**中的**Line Skeleton**视图来选择单个关节。

1. 右键单击其中一个选定的关节，然后选择 **Ragdoll**, **Add to ragdoll**。

    {{< note >}}
   您可以随时为布娃娃添加关节。
    {{< /note >}}
    {{< note >}}
   如果在布偶中添加了一个关节，那么它的所有祖先也会自动添加进来。这种方法非常有用，只需添加几个末端效应器，如手、脚和头部的关节，就能快速添加一个完整的层次结构。同样，如果从布偶中移除一个关节，它的所有后代也会被移除。
    {{< /note >}}

    ![Add a selected joint to the ragdoll in the Skeleton Outliner in the Animation Editor](/images/user-guide/actor-animation/ragdoll-skeleton-outliner-add-to-ragdoll.png)

1. 点击搜索文本框旁边的过滤器 {{< icon "filter.svg" >}} 图标，然后选择 **Ragdoll joints and colliders**。这只会显示布偶中的关节，而不会显示整个动画骨架。

    ![Use the Ragdoll joints and colliders filter to show only joints in the ragdoll in the Animation Editor](/images/user-guide/actor-animation/ragdoll-skeleton-outliner-ragdoll-joints-colliders-filter.png)

    **Skeleton Outliner** 显示的图标表示：
    ![Icon showing that a joint is part of the ragdoll](/images/user-guide/actor-animation/ragdoll-skeleton-outliner-joint-icons-joint-limit.svg) 关节是布娃娃的一部分  
    ![Icon showing that a joint holds ragdoll colliders](/images/user-guide/actor-animation/ragdoll-skeleton-outliner-joint-icons-collider.svg) 一个可容纳布娃娃碰撞体的关节
    ![Icon showing that a joint holds hit detection colliders](/images/user-guide/actor-animation/ragdoll-skeleton-outliner-joint-icons-hit-detection.svg) 保存命中碰撞体的关节
    ![Icon showing that a joint holds cloth colliders](/images/user-guide/actor-animation/ragdoll-skeleton-outliner-joint-icons-cloth.svg) 保存布料碰撞体的关节  

    ![Icons in the Skeleton Outliner show how joints are related to the ragdoll, ragdoll colliders, and hit detection colliders](/images/user-guide/actor-animation/ragdoll-skeleton-outliner-joint-icons.png)

1. 在 **Atom Render Window**中，然后，使用![Use the render options in the Atom Render Window to show or hide ragdoll colliders and joint limits and hit detection colliders](/images/user-guide/actor-animation/ragdoll-skeleton-render-options.svg)图标表示的渲染选项下拉菜单，显示或隐藏布偶碰撞体（渲染为橙色）、布偶关节限制、命中检测碰撞体（渲染为蓝色）和布料碰撞体（渲染为紫色）。

    {{< note >}}
   如果您的布娃娃碰撞体、命中检测碰撞体或布料碰撞体大小相同，您可能需要隐藏不工作的碰撞体。
{{< /note >}}

1. 在**Inspector**选项卡上，您可以查看和修改所选关节的布偶属性。例如，刚体质量、睡眠阈值和碰撞体。

    ![View and modify ragdoll properties for a selected joint on the Inspector tab in the Animation Editor](/images/user-guide/actor-animation/ragdoll-skeleton-ragdoll-tab-properties.png)

## Step 2: 设置布娃娃碰撞体 

**添加和移除布偶碰撞体**

布偶的每个关节可以有 0 个、1 个或多个布偶碰撞体，这些碰撞体会影响布偶与其他物理对象的碰撞方式。布偶支持盒状、球状和胶囊状几何形状。默认情况下，在向布偶添加关节时，会自动创建一个胶囊碰撞体，该碰撞体大致接近演员网格中受该关节影响最大的部分的形状。如果使用不同的几何体效果更好，可以删除默认的碰撞体，然后按照下文所述添加另一个碰撞体。

![Deleting a ragdoll collider](/images/user-guide/actor-animation/ragdoll-remove-collider.png)

要添加碰撞体，请在**动画编辑器**的**Inspector**选项卡上单击**Add Property**，然后在**Ragdoll Collider**部分选择 **Box**, **Capsule** 或 **Sphere**。如果您已经为**Hit Detection**或**Cloth**配置创建了碰撞体，也可以复制这些配置中的碰撞体。

![Add a ragdoll collider shape (box, capsule, or sphere) on the Inspector tab in the Animation Editor](/images/user-guide/actor-animation/add-ragdoll-collider-options.png)

**为碰撞体配置属性**

可以使用**Skeleton Outliner**或点击**Atom Render Window**中的**Line Skeleton**或**Ragdoll Collider**视图来选择布娃娃中的关节。如果选择的是单个关节，则可以使用操纵器调整碰撞体的**Offset** (position) 和 **Rotation**，以及**Capsule**碰撞体的**Height**和**Radius**，操纵器可从**Atom 渲染窗口**左上角的**Viewport UI Cluster**访问。通过点击**Viewport UI Cluster**中的以下图标，可以选择不同的操纵器模式。

| 模式 | 图标 | 说明 |
| - | - | - |
| **Offset** | ![Viewport UI ragdoll collider translation icon](/images/user-guide/actor-animation/ragdoll-viewport-ui-collider-translation.svg) | 激活操纵器，允许修改碰撞体相对于其关节的位置。如果关节上至少有一个布偶碰撞体，则可使用该模式。 |
| **Rotation** | ![Viewport UI ragdoll collider rotation icon](/images/user-guide/actor-animation/ragdoll-viewport-ui-collider-rotation.svg) | 激活操纵器，允许修改碰撞体相对于其关节的旋转角度。如果关节中至少有一个布偶碰撞体，则可使用该模式。|
| **Dimensions** | ![Viewport UI ragdoll collider dimensions icon](/images/user-guide/actor-animation/ragdoll-viewport-ui-collider-dimensions.svg) | 激活操纵器，允许修改**Capsule**碰撞体的**Height**和**Radius**。如果关节中至少有一个布偶碰撞体，且其第一个碰撞体具有**Capsule**几何形状，则该模式可用。 |

{{< note >}}
目前，如果一个关节有多个布偶碰撞体，操纵器只支持编辑第一个碰撞体。
{{< /note >}}

如果您有多个布偶碰撞体，或者您更喜欢直接编辑值，也可以在属性编辑器中配置碰撞体属性。

1. 在**Inspector** 选项卡，在布偶碰撞体属性中执行以下操作：

   1. 设置**Offset**和**Rotation**，将碰撞体移动到正确位置。**Offset**和**Rotation**是相对于关节变换而言的。

   1. 调整碰撞体尺寸（例如，为**Capsule**设置**Height**和**Radius**），以调整碰撞体的大小。

   ![Set the Offset, Rotation, Height, and Radius properties for the ragdoll collider on the Inspector tab in the Animation Editor](/images/user-guide/actor-animation/ragdoll-collider-options-offset-rotation-height-radius.png)

您还可以在属性编辑器中设置其他属性，例如 [物理材料](/docs/user-guide/interactivity/physics/nvidia-physx/materials/) 和 [碰撞过滤](/docs/user-guide/interactivity/physics/nvidia-physx/configuring/configuration-collision-layers/)。

**保存更改**

选择 **File**，**Save Selected Actors**。这将把布偶数据保存到角色的`.assetinfo`文件中。然后，“资产处理器 ”会将布偶数据保存到`.actor`文件中。

**避免自我碰撞**

布偶会自动抑制骨架中相邻关节之间的碰撞。这意味着相邻的碰撞体可以重叠，但不相邻的一对碰撞体不应相交。如果这对碰撞体相交，它们就会在模拟布偶时发生碰撞。这将导致不稳定的行为，因为碰撞体试图分离，而关节则试图将布娃娃保持在一起。如果在层次结构中不相邻的两个关节有重叠的碰撞体，碰撞体将以红色显示，如下图所示。

![Example of ragdoll self-collision](/images/user-guide/actor-animation/ragdoll-self-collision-example.png)

要解决重叠问题，可以调整碰撞体的大小或使用 [碰撞过滤](/docs/user-guide/interactivity/physics/nvidia-physx/configuring/configuration-collision-layers/) ，这样它们就不会相互碰撞。

## Step 3: 创建关节限制

关节限制可以限制关节的活动范围，避免摆出不真实的姿势。例如，如果人类角色的膝关节超过了腿伸直的位置，就会显得不自然。在**PhysX**中，关节限制分为两个部分，即**Swing Limits**和**Twist Limits**。要理解这两个限制，请以肘关节为例。肘关节是上臂（以下称为母体）和前臂（子体）这两个身体部位之间的关节。

**Twist Limit** 描述的是围绕与子刚性连接的轴的旋转。在设置关节限位时，该轴线可以相对于子任意定向，但如果该轴线直接指向子（在示例中从肘部到手腕），通常最容易想象。如果想象一下将手臂伸直放在面前，就可以围绕前臂旋转，使手掌朝上或朝下，但无论朝哪个方向旋转都会很不舒服，也就是说，围绕该轴线旋转的幅度是有限制的。如果你用手指夹着一支笔，与手掌成直角，那么当你旋转前臂时，笔会呈现扇形。

**Swing Limit** 描述了与子代相连的轴可以如何相对于父代移动。继续以肘部为例，如果你考虑保持上臂不动，你可以弯曲肘部使手接触到肩膀，也可以伸展肘部使手臂伸直。此外，还可以向左或向右运动（想想掰手腕）。总之，你可以想象一个固定在上臂上的圆锥体，它描述了前臂所有可能的运动方向。需要注意的是，由于前臂的运动范围并不对称（在某些方向上肘部的弯曲幅度会大于其他方向），因此锥体需要相对于上臂的方向进行旋转。 

综上所述，每个关节限位都可以配置几个参数：
+ 连接到关节子实体的轴，呈现为一个长箭头。通常最简单的方法是将其沿子主体对齐，关节限位在首次创建时会自动这样配置。
+ 相对于父体的**Swing Limit**的方向，它被渲染为一个圆锥体。
+ **Swing Limit**锥体的角度范围。
+ **Twist Limit**的最小值和最大值，呈现为扇形。

如果选择的是单个关节，则可以通过**Atom 渲染窗口**左上方的**Viewport UI Cluster**（如果有碰撞体集群，则位于其下方）访问操纵器来修改这些参数。点击**Viewport UI Cluster**中的以下图标可选择不同的操纵器模式。

| 模式 | 图标 | 说明 |
| - | - | - |
| **Parent local rotation** | ![Viewport UI ragdoll joint parent local rotation icon](/images/user-guide/actor-animation/ragdoll-viewport-ui-joints-parent-frame.svg) | 激活操纵器，允许修改关节母框架的旋转。这样，摆动限位锥的锥形和扭转限位的弧形就可以相对于关节的父框架定向。 |
| **Child local rotation** | ![Viewport UI ragdoll joint child local rotation icon](/images/user-guide/actor-animation/ragdoll-viewport-ui-joints-child-frame.svg) | 激活操纵器，允许修改关节子框架的旋转。这样，在关节限制内移动的轴就可以相对于关节框架调整方向。 |
| **Swing Limit** | ![Viewport UI ragdoll joint swing limits icon](/images/user-guide/actor-animation/ragdoll-viewport-ui-joints-swing-limits.svg) | 激活操纵器，允许修改 Y 和 Z 方向摆动限制的大小。 |
| **Twist Limit** | ![Viewport UI ragdoll joint twist limits icon](/images/user-guide/actor-animation/ragdoll-viewport-ui-joints-twist-limits.svg) | 激活操纵器，允许修改最小和最大扭转限制。|
| **Joint Limit Automatic Fit** | ![Viewport UI ragdoll joint automatic fit icon](/images/user-guide/actor-animation/ragdoll-viewport-ui-joints-auto-fit.svg) | 根据从已加载的**Motion Set**中的姿势采样计算最佳关节极限。要使用此功能，需要在[**Motion Sets**](/docs/user-guide/visualization/animation/animation-editor/motion-set-user-interface/) 面板中加载一个**Motion Set**。|  

关节限位配置也可以使用属性编辑器进行调整。

如果**Actor**的当前姿势导致**Swing Limit**或**Twist Limit**被违反，该限制将以红色显示。


