---
linkTitle: PhysX Hinge Joint
description: Open 3D Engine PhysX Hinge Joint 组件。
title: PhysX Hinge Joint 组件
---



使用 **PhysX Hinge Joint** （PhysX 铰链关节） 组件，您可以创建一个动态铰链关节，该关节将实体约束到关节，并可以自由地围绕关节的 x 轴旋转。

## PhysX Hinge Joint 组件属性 

![Properties of the PhysX Hinge Joint component](/images/user-guide/physx/physx/ui-physx-hinge-joint-component.png)

**Local Position**
指定关节相对于实体变换的位置。

**Local Rotation**
指定关节相对于实体变换的旋转。

**Lead Entity**
指定将驱动关节的lead（父级）实体。

**Breakable**
启用后，如果施加了足够的力，关节将断开。启用 **Breakable** 将公开 **Maximum Force** 和 **Maximum Torque** 属性。
启用了 **Compute Mass** 属性的 **PhysX Dynamic Rigid Body** 组件可能具有非常大的质量值。如果包含关节组件或引导实体的实体启用了其 **Compute Mass** 属性，则 **Maximum Force** 和 **Maximum Torque** 属性可能需要非常高的值才能防止断裂。

**Maximum Force**
启用 **Breakable** 后，指定关节在断开之前可以承受的最大力。有效值范围从 **0.01** 到 **Infinity**。

**Maximum Torque**
启用 **Breakable** 后，指定关节在断开之前可以承受的最大扭矩。有效值范围从 **0.01** 到 **Infinity**。

**Display Setup in Viewport**
启用后，将显示三个平面，显示关节的方向和限制。红色和绿色平面显示 **Positive angular limit** 和 **Negative angular limit**。白色平面显示关节的 0 度旋转。三个基准面的共享边是铰链接榫的 x 轴。一条线显示关节和 **Follower Entity** 之间的连接。

**Select Lead on Snap**
启用后，在组件模式下将关节捕捉到实体会将该实体设置为 **Lead Entity**。包含关节零部件的实体将从此操作中排除。

**Lead-Follower Collide**
启用后，lead 实体和 follower 实体（包含关节组件的实体）将发生碰撞。

**Limit**
启用后，引导实体围绕关节轴的移动受角度限制。启用 **Limit** 将公开 **Soft Limit**、**Positive angular limit**和 **Negative angular limit** 属性。

**Soft Limit**
启用后，允许引导实体围绕关节轴的移动超过指定的限制。启用 **Soft Limit** 后，当引导实体旋转超过限制时，引导实体的移动将被视为弹簧，并将减慢，然后弹回到限制区域。启用 **Soft Limit** 将显示 **Damping** 和 **Stiffness** 属性。

**Damping**
启用 **Soft Limit** 后，当超出旋转限制时，弹簧的驱动力相对于从动件的速度。有效值范围为 **0.001** 到 **1000000.0**。

**Stiffness**
启用 **Soft Limit** 后，当超出旋转限制时，弹簧的驱动力相对于从动件的位置。有效值范围为 **0.001** 到 **1000000.0**。

**Positive angular limit**
启用 **Limit** 后，将围绕关节轴的正旋转限制。有效值范围为 **0.1** 到 **360.0**。

**Negative angular limit**
启用 **Limit** 后，将围绕关节轴的负旋转限制。有效值范围为 **0.1** 到 **360.0**。

**Edit**
单击后，将启用组件编辑模式。在组件编辑模式下，除 **PhysX Ball Joint** 组件外，所有组件都被锁定。**PhysX Ball Joint**（PhysX 球关节）组件的属性可以在 **Perspective**中编辑。按 **Tab** 在组件编辑模式之间循环。单击 **Done** 退出组件模式。
