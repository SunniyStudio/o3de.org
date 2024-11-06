---
linkTitle: Cloth
description: ' Open 3D Engine Cloth 组件。 '
title: Cloth 组件
---



**Cloth** 组件将其引用的任何网格的顶点视为布料粒子，并应用物理属性、力和约束来模拟布料的行为。您可以将此组件添加到具有 **Mesh** 或 **Actor** 组件的任何实体。您可以向一个实体添加多个 Cloth 组件。

**Cloth** 组件是由 [NVIDIA Cloth gem](/docs/user-guide/gems/reference/physics/nvidia/nvidia-cloth/) 提供的。

对于使用**Cloth**组件的信息，请查看[使用NVIDIA Cloth模拟布料](/docs/user-guide/interactivity/physics/nvidia-cloth/)。

**内容**
+ [Base 属性](#base-properties)
+ [Motion constraints 属性](#motion-constraints-properties)
+ [Backstop 属性](#backstop-properties)
+ [Damping 属性](#damping-properties)
+ [Inertia 属性](#inertia-properties)
+ [Wind 属性](#wind-properties)
+ [Collision 属性](#collision-properties)
+ [Self Collision 属性](#self-collision-properties)
+ [Fabric stiffness 属性](#fabric-stiffness-properties)
+ [Fabric compression 属性](#fabric-compression-properties)
+ [Fabric stretch 属性](#fabric-stretch-properties)
+ [Tether constraints 属性](#tether-constraints-properties)
+ [Quality 属性](#quality-properties)

## Base 属性 

![Base properties of the Cloth component](/images/user-guide/physx/cloth/ui-cloth-component-A.png)

**Simulate in editor**
启用此选项可在编辑器中模拟布料。

**Mesh node**
要模拟为 Cloth 的网格节点。列表中可用的网格在 **FBX Settings** 中应用了 **Cloth** 修改器。

**Mass**
应用于所有布料粒子质量的 Scale multiplier。此属性中的值 **0.0** 使所有布料粒子变为静态。

**Custom Gravity**
启用此选项可覆盖全局重力并为此布料设置自定义重力值。

**Gravity**
设置此布料的重力。默认值 **Z** 轴上的 **-9.81** 为标准重力。
**Gravity** 属性由 **Custom Gravity** 属性启用。

**Gravity Scale**
应用于布料粒子重力的 Scale multiplier。

**Stiffness frequency**
调整布料模拟的整体刚度的刚度指数。此指数每秒应用于以下属性：
+ **Damping**
+ **Damping - Linear drag**
+ **Damping - Angular drag**
+ **Wind - Air drag coefficient**
+ **Wind - Air lift coefficient**
+ **Self collision - Stiffness**
+ **Fabric stiffness**
+ **Fabric compression**
+ **Fabric stretch**
+ **Tether - Stretch**

## Motion constraints 属性 

![Motion constraints properties of the Cloth component](/images/user-guide/physx/cloth/ui-cloth-component-B.png)

**Max distance**
布料粒子移动的最大距离限制（以米为单位）。

**Scale**
应用于所有运动约束的 Scale 值。
**0.0**: 运动约束不起作用。
**1.0**: 运动约束已完全应用。

**Bias**
添加到所有运动约束的 Bias 值（以米为单位）。有效值范围从 **-Infinity** 到 **Infinity**。

**Stiffness**
运动约束的刚度。
**0.0**: 刚度 （Stiffness） 不应用于运动约束。
**1.0**: 刚度完全应用于运动约束。

## Backstop 属性 

![Backstop properties of the Cloth component](/images/user-guide/physx/cloth/ui-cloth-component-C.png)

{{< note >}}
仅当在 **FBX Settings** 中所选 **Mesh node** 的 **Cloth** 修改器中指定了 **Backstop** 顶点颜色流时，Backstop 属性才可用。
{{< /note >}}

**Radius**
逆止球体的半径（以米为单位）。

**Back offset**
布料后面的逆止球体的偏移量（以米为单位）。

**Front offset**
布料前面的逆止球体的偏移量（以米为单位）。

## Damping 属性 

![Damping properties of the Cloth component](/images/user-guide/physx/cloth/ui-cloth-component-D.png)

**Damping**
布料粒子速度的阻尼。
**0.0**: Velocity 不受影响。
**1.0**: Velocity 被归零。

**Linear Drag**
应用于布料粒子的速度部分。
**0.0**: 布料粒子不受影响。
**1.0**: 阻尼的全局布料粒子速度。

**Angular Drag**
应用于转动布料粒子的角速度的一部分。
**0.0**: 布料粒子不受影响。
**1.0**: 阻尼的全局布料粒子角速度。

## Inertia 属性 

![Inertia properties of the NVIDIA Cloth component](/images/user-guide/physx/cloth/ui-cloth-component-E.png)

**Linear**
应用于布料粒子的线性加速度的一部分。
**0.0**: 布料粒子不受影响。
**1.0**: 物理正确的线性加速度。

**Angular**
应用于旋转布料粒子的角加速度的一部分。
**0.0**: 布料粒子不受影响。
**1.0**: 物理校正角加速度。

**Centrifugal**
应用于转动布料粒子的角速度的一部分。
**0.0**: 布料粒子不受影响。
**1.0**: 物理校正角速度。

## Wind 属性 

{{< note >}}
组件风属性创建的风仅影响组件引用的布料。有关创建可影响多个实体中多个零部件的风的说明，请参阅 [创建风力](/docs/learning-guide/tutorials/physx/wind-provider)。
{{< /note >}}

![Wind properties of the Cloth component](/images/user-guide/physx/cloth/ui-cloth-component-F.png)

{{< note >}}
当以下 **Air drag** 和 **Air lift** 系数均为 **0.0** 时，风被禁用。
{{< /note >}}

**Enable local wind velocity**
启用此选项可设置此布料的风 **Local velocity**。禁用时，将使用 Physics：：WindBus 中的速度。

**Local velocity**
世界坐标中的风矢量（方向和大小）。大小越大，风力越强。
风 **Local velocity** 属性由 **Enable local wind velocity** 属性启用。

**Air drag coefficient**
指定应用于布料粒子的阻力空气量。

**Air lift coefficient**
指定应用于布料粒子的提升空气量。

**Air density**
用于阻力和升力计算的空气密度。

## Collision 属性 

![Collision properties of the Cloth component](/images/user-guide/physx/cloth/ui-cloth-component-G.png)

**Friction**
控制布料粒子和碰撞体之间的摩擦量。
**0.0**: **Friction** 已禁用。

**Mass scale**
控制碰撞期间布料粒子质量增加的速度。
**0.0**: **Mass scale** 已禁用。

**Continuous detection**
连续碰撞检测通过计算布料粒子和碰撞体之间的撞击时间来改善碰撞。
质量的提高会影响性能。我们建议您仅在必要时使用 **Continuous detection**。

**Affects static cloth particles**
启用此选项可允许碰撞器移动静态布料粒子。静态布料粒子具有 **0.0** 的反质量。

## Self Collision 属性 

![Self collision properties of the Cloth component](/images/user-guide/physx/cloth/ui-cloth-component-H.png)

**Distance**
碰撞布料粒子必须彼此保持的最小距离（以米为单位）。
**0.0**: **Self collision** 已禁用。

**Stiffness**
自碰撞约束的刚度。
**0.0**: **Self collision** 已禁用。

## Fabric stiffness 属性 

![Fabric stiffness properties of the Cloth component](/images/user-guide/physx/cloth/ui-cloth-component-I.png)

**Horizontal**
水平拉伸和压缩约束的刚度值。
**0.0**: 无水平拉伸和压缩约束。

**Horizontal multiplier**
水平拉伸和压缩约束的 Scale 值。
**0.0**: 未应用水平拉伸和压缩约束。
**1.0**: 完全应用水平拉伸和压缩约束。

**Vertical**
垂直拉伸和压缩约束的刚度值。
**0.0**: 无垂直拉伸和压缩约束。

**Vertical multiplier**
垂直拉伸和压缩约束的缩放值。
**0.0**: 未应用水平拉伸和压缩约束。
**1.0**: 完全应用水平拉伸和压缩约束。

**Bending**
弯曲约束的刚度值。此值定义布料在自身上折叠的难易程度。
**0.0**: 无弯曲约束。

**Bending multiplier**
弯曲约束的 Scale 值。
**0.0**: 未应用弯曲约束。
**1.0**: 完全应用弯曲约束。

**Shearing**
剪切约束的刚度值。该值定义布料扭曲的难易程度。
**0.0**: 无剪切约束。

**Shearing multiplier**
剪切约束的 Scale 值。
**0.0**: 未应用剪切约束。
**1.0**: 完全应用剪切约束。

## Fabric compression 属性 

![Fabric Compression properties of the Cloth component](/images/user-guide/physx/cloth/ui-cloth-component-J.png)

**Horizontal limit**
水平约束的压缩限制。此属性受 **Fabric stiffness** 属性组中的 **Horizontal multiplier** 影响。
**0.0**: 禁用水平压缩。

**Vertical limit**
垂直约束的压缩限制。此属性受 **Fabric stiffness** 属性组中的 **Vertical multiplier** 影响。
**0.0**: 已禁用垂直压缩。

**Bending limit**
弯曲约束的压缩限制。此属性受 **Fabric stiffness** 属性组中的 **Bending multiplier** 影响。
**0.0**: 禁用弯曲压缩。

**Shearing limit**
剪切约束的压缩极限。此属性受 **Fabric stiffness** 属性组中的 **Shearing multiplier** 影响。
**0.0**: 已禁用剪切压缩。

## Fabric stretch 属性 

{{< note >}}
对于 **Fabric stretch** 属性，减少 **Tether constraints** 的 **Stiffness** 或增加其 **Scale** 以允许织物拉伸。
{{< /note >}}

![Fabric stretch properties of the Cloth component](/images/user-guide/physx/cloth/ui-cloth-component-K.png)

**Horizontal limit**
水平约束的拉伸限制。此属性受 **Fabric stiffness** 属性组中的 **Horizontal multiplier** 影响。
**0.0**: 禁用水平拉伸。

**Vertical limit**
垂直约束的拉伸限制。此属性受 **Fabric stiffness** 属性组中的 **Vertical multiplier** 影响。
**0.0**: 已禁用垂直拉伸。

**Bending limit**
弯曲约束的拉伸限制。此属性受 **Fabric stiffness** 属性组中的 **Bending multiplier** 影响。
**0.0**: 禁用弯曲拉伸。

**Shearing limit**
剪切约束的拉伸限制。此属性受 **Fabric stiffness** 属性组中的 **Shearing multiplier** 影响。
**0.0**: 剪切拉伸已禁用。

## Tether constraints 属性 

![Tether constraints properties of the Cloth component](/images/user-guide/physx/cloth/ui-cloth-component-L.png)

**Stiffness**
系绳约束的刚度。
**0.0**: 禁用 Tether 约束。
**1.0**: Tether 约束的行为类似于 Spring。

**Scale**
系绳约束 **Stiffness** 的比例因子、

## Quality 属性 

![Quality properties of the Cloth component](/images/user-guide/physx/cloth/ui-cloth-component-M.png)

**Solver frequency**
目标求解器每秒迭代次数。每秒执行的迭代次数可能因许多性能因素而异。但是，无论设置的值如何，每帧至少会求解一次迭代。

**Acceleration filter iterations**
用于重力和外部加速度的增量时间因子的平均迭代次数。

**Remove static triangles**
启用此选项可删除由静态布料粒子组成的三角形。启用此属性可提高性能，但是，移除的静态布料粒子将不会用于碰撞和自碰撞计算。

**Update normals of static particles**
启用后，静态粒子的法线将根据模拟网格的移动进行更新。禁用后，静态粒子将保持与原始网格相同的法线。
