---
description: ' 使用 PhysX 关节在 Open 3D Engine 中的实体之间创建动态关节约束。 '
title: 使用 PhysX 的动态关节
weight: 400
---

PhysX 关节组件约束一个 PhysX 动态刚体（称为 *跟随者*）相对于另一个刚体（称为 *前导*）的位置和方向。跟随者刚体将在关节周围的零个、一个或两个轴上具有旋转自由度，具体取决于 PhysX 关节的类型。

下面的示例图像是不同关节类型的简单演示。在每个示例中，蓝色球体是前导刚体。红色球体是跟随者刚体，它必须始终是模拟的刚体。关节组件始终是 follower 实体的一部分。将关节添加到从动节点后，需要将其配置为放置在引线周围（使用关节组件的 **Local Position** 和 **Local Rotation** 属性）。在此示例中，球和铰链引线是静态刚体，但它们也可以是运动学或模拟刚体。

![PhysX Joints example](/images/user-guide/physx/physx/anim-joints-example.gif)

**内容**
+ [PhysX 关节类型](#physx-joint-types)
+ [PhysX 关节设置](#physx-joint-setup)
+ [PhysX 关节配置](#physx-joint-configuration)
  + [位置模式](#position-mode)
  + [旋转模式](#rotation-mode)
  + [对齐位置模式](#snap-position-mode)
  + [对齐旋转模式](#snap-rotation-mode)
  + [Maximum Force （最大力） 和 Maximum Torque （最大扭矩） 模式](#maximum-force-and-maximum-torque-modes)
  + [摆动限制模式](#swing-limits-mode)
  + [Twist limits 模式](#twist-limits-mode)
  + [刚度和阻尼模式](#stiffness-and-damping-modes)
+ [稳定性说明](#notes-on-stability)

## PhysX 关节类型

有关三种 PhysX 关节类型的信息，请参阅下面的链接组件参考：
+ [ PhysX Ball Joint 组件参考](/docs/user-guide/components/reference/physx/ball-joint/) - **PhysX Ball Joint** 组件允许 Follower 刚体在两个轴上自由旋转。
+ [ PhysX Fixed Joint 组件参考](/docs/user-guide/components/reference/physx/fixed-joint/) - **PhysX Fixed Joint** 组件不允许 Follower 刚体在任何轴上自由旋转。
+ [ PhysX Hinge Joint 组件参考](/docs/user-guide/components/reference/physx/hinge-joint/) - **PhysX Hinge Joint** 组件允许 Follower 刚体在一个轴上自由旋转。
+ [ PhysX Prismatic Joint 组件参考](/docs/user-guide/components/reference/physx/prismatic-joint/) - **PhysX Prismatic Joint** 组件保持相同的旋转，但允许从动刚体沿一个轴自由移动。

## PhysX 关节设置 

每种关节类型的设置都是相同的。

1. 为 **leader** 刚体创建实体。

   1. 创建新实体。右键单击 **Perspective**，然后从上下文菜单中选择 **Create enity**。

   1. 添加 **PhysX Static Rigid Body** 或 **PhysX Dynamic Rigid Body**（类型 *kinematic* 或 *simulated*）组件，具体取决于您是否希望领导者移动。

   1. 将 PhysX 碰撞器添加到实体。

1. 为 **follower** 刚体和关节创建一个实体。

   1. 创建新实体。

   1. 将 **PhysX Dynamic Rigid Body** （类型 *simulated*） 组件添加到实体。

   1. 将 PhysX 碰撞器添加到实体。这是 angle limits 正常工作所必需的。在没有 PhysX 碰撞器的情况下，关节仍然可以工作，但有角度限制，并且可能不会强制执行。使用触发器碰撞器时也是如此。

   1. 添加其中一个 PhysX 关节组件：
      + **PhysX Ball Joint**
      + **PhysX Fixed Joint**
      + **PhysX Hinge Joint**
      + **PhysX Prismatic Joint**

   1. 通过单击 **Lead Entity** 属性右侧的 **Target** 按钮，然后在 **Perspective** 中选择引线实体，将引线实体分配给 PhysX 关节。

   1. 调整关节的位置和方向，将其移动到引线的位置。使用 PhysX 关节组件中的 **Local Position** 和 **Local Rotation** 字段。您可以通过单击 **Edit** 按钮进入组件模式，并在 **Perspective**中配置关节。

{{< note >}}
从动轮刚体不需要具有引导刚体。当 follower 没有 leader 时，它将受到全局位置的约束。
{{< /note >}}

## 使用组件编辑模式的 PhysX 关节配置

关节组件具有一个 **Edit** 按钮，用于启用组件编辑模式。在组件编辑模式下，您可以在 **Perspective** 中编辑关节的属性。您可以在组件编辑模式下使用多个编辑上下文之一。按 **Tab** 键可在编辑模式上下文之间循环。当前上下文显示在 **Perspective** 窗格的底部。

### Position mode 

**Applies to:** 球形接头、固定接头和铰链接头

![PhysX joint position mode](/images/user-guide/physx/physx/ui-physx-joint-position-mode.png)

Position （位置） 模式显示一个平移 Gizmo，您可以单击并拖动该 Gizmo 来调整关节相对于实体变换的 **Local Position（本地位置）**。

### Rotation mode 

**Applies to:** 球形接头、固定接头和铰链接头

![PhysX joint rotation mode](/images/user-guide/physx/physx/ui-physx-joint-rotation-mode.png)

Rotation （旋转） 模式显示一个旋转小工具，您可以在任何轴上单击并拖动该小工具，以调整关节相对于实体变换的 **Local Rotation** （局部旋转）。

### Snap position mode 

**Applies to:** 球形接头和铰链接头

![PhysX joint snap position mode](/images/user-guide/physx/physx/ui-physx-joint-snap-position-mode.png)

当您将鼠标悬停在实体上时，Snap position （对齐位置） 模式会显示高亮显示边界框和目标。单击实体以将关节 **Local Position** 对齐到高亮显示的实体的位置。如果在关节属性中启用了 **Select Lead on Snap**，则该实体将被分配给关节的 **Lead Entity** 属性。可以选择除 follower 实体之外的任何实体。

### Snap rotation mode 

**Applies to:** 球形接头

![PhysX joint snap rotation mode](/images/user-guide/physx/physx/ui-physx-joint-snap-rotation-mode.png)

当您将鼠标悬停在实体上时，Snap rotation 模式会显示高亮显示边界框和目标。单击实体以将关节 **Local Rotation** 与高亮显示的实体的旋转对齐。可以选择除引导实体之外的任何实体。

### Maximum Force and Maximum Torque modes 

**Applies to:** 球形接头、固定接头和铰链接头

![PhysX joint maximum force and maximum torque modes](/images/user-guide/physx/physx/ui-physx-joint-breakable-properties-mode.png)

最大力和最大扭矩模式显示一个灰色框，您可以单击并拖动该框来调整 **最大力** 和 **最大扭矩** 属性。最大力和最大扭矩模式和属性仅在为关节启用 **Breakable** 属性时可用。

### Swing limits mode 

**Applies to:** 球形接头

![PhysX joint swing limits mode](/images/user-guide/physx/physx/ui-physx-joint-swing-limit-mode.png)

“摆动限制”模式在关节的局部根部显示一个环形 Gizmo，可用于旋转关节 x 轴上的摆动限制，以及一个缩放 Gizmo，可用于在关节的 y 轴和 z 轴上均匀或不均匀地缩放摆动限制。仅当为球关节组件启用了 **Limits** 属性时，Swing limits 模式才可用。

### Twist limits mode 

**Applies to:** 铰链关节

![PhysX joint twist limits mode](/images/user-guide/physx/physx/ui-physx-joint-twist-limit-mode.png)

Twist limits （扭曲限制） 模式显示两个环形小工具，您可以单击并拖动这两个小工具来调整 **正角度限制** 和 **负角度限制** 属性。红色环调整正限制，绿色环调整负限制。仅当为铰链关节组件启用了 **Limits** 属性时，Twist limits 模式才可用。

### Stiffness and Damping modes 

**Applies to:** 球形接头和铰链接头

![PhysX joint stiffness and damping modes](/images/user-guide/physx/physx/ui-physx-joint-soft-limit-properties-mode.png)

Stiffness （刚度） 和 damping （阻尼） 模式显示一个灰色框，您可以单击并拖动该框来调整 **Stiffness** 和 **Damping** 属性。刚度和阻尼模式和属性仅在为关节启用 **Soft limit** 属性时可用。

## 稳定性说明

PhysX 关节使用的迭代求解器可能无法在某些配置中保持约束。例如，求解器可能无法收敛。结果是模拟过程中的运动不稳定或意外。PhysX 文档介绍了有助于避免此类情况的配置。请参阅 NVIDIA 的 PhysX 关节文档中的 [配置关节以实现最佳行为](https://docs.nvidia.com/gameworks/content/gameworkslibrary/physx/guide/Manual/Joints.html#configuring-joints-for-best-behavior) 。
