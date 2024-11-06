---
linkTitle: PhysX Dynamic Rigid Body
title: PhysX Dynamic Rigid Body 组件
description: PhysX Dynamic Rigid Body 组件创建可以移动并与其他 PhysX 实体碰撞的模拟或运动实体对象。
toc: true
---

**PhysX Dynamic Rigid Body** 使实体成为可以移动并与其他 PhysX 实体碰撞的 *模拟* 或 *运动* 实体对象。该实体还必须至少有一个 [PhysX Shape Collider](/docs/user-guide/components/reference/physx/shape-collider/), [PhysX Primitive Collider](/docs/user-guide/components/reference/physx/collider/) 或 [PhysX Mesh Collider](/docs/user-guide/components/reference/physx/mesh-collider/)组件，用于定义实体的碰撞器。

模拟的刚体完全由 PhysX 驱动。模拟的刚体会响应碰撞事件和力而移动，并且不会通过运动或脚本进行动画处理。默认情况下，模拟刚体受重力影响，但可以在 PhysX Dynamic Rigid Body （PhysX 动态刚体） 组件中停用重力。

运动学刚体不完全由 PhysX 驱动。它们具有脚本动画，不受力或重力的影响。例如，门通常具有脚本动画。如果门是运动学刚体，则门可以在脚本动画中移动，并在模拟期间与其他 PhysX 实体碰撞。移动是使用您在脚本中指定的 `SetKinematicTarget` 方法创建的。
{{< note >}}
您应始终将 PhysX Dynamic Rigid Body （PhysX 动态刚体） 组件添加到实体层次结构的顶层。将 PhysX Dynamic Rigid Body （PhysX 动态刚体） 组件添加到子实体可能会导致与实体的世界变换发生冲突，并导致未定义的行为。
{{< /note >}}

## 提供者

[PhysX Gem](/docs/user-guide/gems/reference/physics/nvidia/physx/)

## 属性

![PhysX Dynamic Rigid Body component interface.](/images/user-guide/components/reference/physx/physx-rigid-body-ui-01.png)

| 属性 | 说明 | 值 | 默认值 |
| - | - | - | - |
| **Type** | 确定如何控制刚体的移动/位置。模拟的刚体响应碰撞事件和力，其运动由 PhysX 模拟。运动学刚体不受重力或其他力的影响，可以通过脚本移动。 | `Simulated`, `Kinematic` | `Simulated` |
| **Initial linear velocity** | 设置激活实体时刚体的起始线速度（以米/秒为单位）。这将创建沿线性速度矢量方向的运动。 | Vector3: -Infinity to Infinity | X: `0.0`, Y: `0.0`, Z: `0.0` |
| **Initial angular velocity** | 设置激活实体时刚体的起始角速度（以弧度/秒为单位）。这将在角速度矢量的方向上创建旋转。 | Vector3: -Infinity to Infinity | X: `0.0`, Y: `0.0`, Z: `0.0` |
| **Linear damping** | 设置线性速度随时间变化的衰减速率，即使没有力作用在刚体上。如果未应用线性力，则非零值最终会停止刚体。值必须为非负数。 | Float: 0 to Infinity | `0.05` |
| **Angular damping** | 设置角速度随时间变化的衰减速率，即使没有力作用在刚体上。如果未施加扭矩力，则非零值最终会停止刚体。值必须为非负数。 | Float: 0.0 to Infinity | `0.15` |
| **Sleep threshold** | 设置每单位质量的动能，低于该动能时，刚体可以进入休眠状态。值必须为非负数。 | Float: 0.0 to Infinity | `0.005` |
| **Start asleep** | 如果启用，则 PhysX Dynamic Rigid Body （PhysX 动态刚体） 组件在激活实体时处于休眠状态，并在施加足够的力时唤醒。 | Boolean | `Disabled` |
| **Interpolate motion** | 如果启用，则模拟产生的运动将平滑。为需要平滑运动的对象（如车辆）启用此属性。  | Boolean | `Disabled` |
| **Gravity enabled** | 如果启用，则刚体会受到重力的影响。这仅适用于模拟刚体。 | Boolean | `Enabled` |
| **CCD enabled** | 如果启用，则执行连续冲突检测 （CCD）。此属性对于高速对象非常有用，可确保准确的碰撞检测。激活此属性将显示两个附加属性，即 **Min advance coefficient** 和 **CCD Friction**。要使用连续碰撞检测，您还必须在 **PhysX Configuration** 窗口中激活 **Continuous Collision Detection**。有关更多信息，请参阅 [PhysX 配置](/docs/user-guide/interactivity/physics/nvidia-physx/configuring/configuration-global/#physx-configuration-global-world)。 | Boolean | `Disabled` |
| **Min advance coefficient** | 微调连续碰撞检测。较低的值会减少剪切，但会影响运动平滑度。 | Float: 0.01 to 0.99 | `0.15` |
| **CCD friction** | 如果启用，则在解决 CCD 碰撞时应用摩擦。 | Boolean | `Disabled` |
| **Compute mass** | 如果启用，则计算刚体的质量。 | Boolean | `Disabled` |
| **Linear Axis - Lock X** | 如果启用，力不会在刚体的 X 轴上创建平移。 | Boolean | `Disabled` |
| **Linear Axis - Lock Y** | 如果启用，力不会在刚体的 Y 轴上创建平移。 | Boolean | `Disabled` |
| **Linear Axis - Lock Z** | 如果启用，力不会在刚体的 Z 轴上创建平移。 | Boolean | `Disabled` |
| **Angular Axis - Lock X** | 如果启用，力不会在刚体的 X 轴上产生旋转。 | Boolean | `Disabled` |
| **Angular Axis - Lock Y** | 如果启用，力不会在刚体的 Y 轴上创建旋转。 | Boolean | `Disabled` |
| **Angular Axis - Lock Z** | 如果启用，力不会在刚体的 Z 轴上产生旋转。 | Boolean | `Disabled` |
| **Mass** | 如果禁用了 **Compute Mass**（计算质量）**，则可以为 PhysX Dynamic Rigid Body （PhysX 动态刚体） 指定 **Mass** 值（以千克为单位）。 | Float: 0.0 to Infinity | `1.0` |
| **Compute COM** | 如果启用，则会自动计算刚体的质心。 | Boolean | `Enabled` |
| **COM offset** | 如果 **Compute COM** 处于禁用状态，则可以将质心指定为偏移量。 | Vector3: -Infinity to Infinity | X: `0.0`, Y: `0.0`, Z: `0.0` |
| **Compute inertia** | 如果启用，则根据刚体的质量和形状计算惯性。 | Boolean | `Enabled` |
| **Inertia diagonal** | 如果禁用 **Compute inertia**，则可以将 **Inertia diagonal** 指定为惯性张量的对角线元素。这是在每个轴上旋转刚体所需的扭矩。 | Vector3: 0.0 to Infinity | X: `1.0`, Y: `1.0`, Z: `1.0` |
| **Maximum angular velocity** | 将角速度限制为指定值。这在刚体以不切实际的快速角速度旋转的情况下非常有用。 | Float: 0.0 to Infinity | `100.0` |
| **Include non-simulated shapes in Mass** | 如果启用，则非模拟形状将包含在质量、质心和惯性计算中。 | Boolean | `Disabled` |
| **Debug draw COM** | 如果启用，则显示此刚体的质心。 | Boolean | `Disabled` |
| **Solver Position Iterations** | 计算物体位置更新时使用的迭代次数。较高的值可能会提高模拟保真度，但会增加计算成本。 | Integer: 1 to 255 | `4` |
| **Solver Velocity Iterations** | 计算物体的速度更新时使用的迭代次数。较高的值可能会提高模拟保真度，但会增加计算成本。 | Integer: 1 to 255 | `1` |
