---
linkTitle: PhysX Fixed Joint
description: Open 3D Engine PhysX Fixed Joint 组件。
title: PhysX Fixed Joint 组件
---



使用 **PhysX Fixed Joint** （PhysX 固定关节） 组件，您可以创建一个动态固定关节，该关节将实体约束到关节，在任何轴上都没有自由度。

## PhysX Fixed Joint 组件属性 

![Properties of the PhysX Fixed Joint component](/images/user-guide/physx/physx/ui-physx-fixed-joint-component.png)

**Local Position**
指定关节相对于实体变换的位置。

**Local Rotation**
指定关节相对于实体变换的旋转。

**Lead Entity**
指定将驱动关节的潜在客户（父级）实体。

**Breakable**
启用后，如果施加了足够的力，关节将断开。启用 **Breakable** 将公开 **Maximum Force** 和 **Maximum Torque** 属性。
启用了 **Compute Mass** 属性的 **PhysX Dynamic Rigid Body** 组件可能具有非常大的质量值。如果包含关节组件或引导实体的实体启用了其 **Compute Mass** 属性，则 **Maximum Force** 和 **Maximum Torque** 属性可能需要非常高的值才能防止断裂。

**Maximum Force**
启用 **Breakable** 后，指定关节在断开之前可以承受的最大力。有效值范围从 **0.01** 到 **Infinity**。

**Maximum Torque**
启用 **Breakable** 后，指定关节在断开之前可以承受的最大扭矩。有效值范围从 **0.01** 到 **Infinity**。

**Display Setup in Viewport**
启用后，将显示一条表示关节与其随动轮之间连接的线。

**Select Lead on Snap**
启用后，在组件模式下将关节捕捉到实体会将该实体设置为 **Lead Entity**。包含关节零部件的实体将从此操作中排除。

**Lead-Follower Collide**
启用后，lead 实体和 follower 实体（包含关节组件的实体）将发生碰撞。

**Edit**
单击后，将启用组件编辑模式。在组件编辑模式下，除 **PhysX Ball Joint** 组件外，所有组件都被锁定。**PhysX Ball Joint**（PhysX 球关节）组件的属性可以在 **Perspective**中编辑。按 **Tab** 在组件编辑模式之间循环。单击 **Done** 退出组件模式。
