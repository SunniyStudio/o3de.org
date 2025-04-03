---
linkTitle: PhysX Primitive Collider
title: PhysX Primitive Collider 组件
description: PhysX Primitive Collider 组件将 PhysX 碰撞器添加到实体，以便该实体可以包含在 PhysX 模拟中。
toc: true
---

**PhysX Primitive Collider** 组件将 PhysX 碰撞器添加到实体中，以便该实体可以包含在 PhysX 模拟中。碰撞器由在 PhysX Primitive Collider组件中选择的简单形状基元定义。PhysX Primitive Collider 组件还可以定义触发区域或力区域。
{{< note >}}
添加一个带有 PhysX Primitive Collider 组件的 [PhysX Static Rigid Body](/docs/user-guide/components/reference/physx/static-rigid-body/)组件，以创建永不移动的 **静态** 实体。添加 [PhysX Dynamic Rigid Body](/docs/user-guide/components/reference/physx/rigid-body/) 组件以创建 **模拟simulated** 或 **运动学kinematic** 实体。模拟实体会随着碰撞和力而移动。运动实体不受碰撞或力的影响，但受脚本化运动驱动。

{{< /note >}}

## 提供者

[PhysX Gem](/docs/user-guide/gems/reference/physics/nvidia/physx/)

## 属性 

![PhysX Primitive Collider component interface.](/images/user-guide/components/reference/physx/physx-collider-ui-01.png)

### Base 属性

| 属性 | 说明 | 值 | 默认值 |
| - | - | - | - |
| **Collision Layer** | 将碰撞器分配给碰撞层。碰撞层可用于限制 PhysX 对象之间的物理交互。 | 在项目的 [Collision Layers](/docs/user-guide/interactivity/physics/nvidia-physx/configuring/configuration-collision-layers/) 中定义的任何碰撞图层。  | `Default` |
| **Collides With** | 将碰撞器分配给碰撞组。Collision group （碰撞组） 包含此碰撞器可以与之碰撞的碰撞层。 | 在工程的 [Collision Groups（碰撞组）](/docs/user-guide/interactivity/physics/nvidia-physx/configuring/configuration-collision-groups/) 中定义的任何碰撞组。 | `All` |
| **Trigger** | 如果启用，此碰撞器将用作触发器。Trigger 执行与其他碰撞器的快速重叠测试。触发器不施加力或返回接触点信息。使用此选项可加快 PhysX 计算速度，其中碰撞器之间的简单重叠测试就足够了。 | Boolean | `Disabled` |
| **Simulated** | 如果启用，此碰撞器将包含在物理模拟中。 | Boolean | `Enabled` |
| **In Scene Queries** | 如果启用，则可以查询此碰撞器的光线投射、形状投射和重叠。 | Boolean | `Enabled` |
| **Offset** | 设置碰撞器相对于实体的局部偏移位置。 | Vector3: -Infinity to Infinity | X: `0.0`, Y: `0.0`, Z: `0.0` |
| **Rotation** | 为 PhysX Primitive collider 碰撞器组件的 **Offset** （偏移） 设置碰撞器的局部旋转。 | Vector3: -180.0 to 180.0 | X: `0.0`, Y: `0.0`, Z: `0.0` |
| **Physics Materials** | 为此碰撞器的每个材质选择一种物理材质。物理材质定义表面的物理属性，例如动态和静态摩擦以及密度。原始碰撞体具有整个碰撞体的单个物理材质。 | A `.physxmaterial` asset assigned. | `(default)` |
| **Tag** | 为此碰撞器设置一个标签。标签可用于快速识别脚本或代码中的组件。 | String | None |
| **Rest offset** | 设置此碰撞器与其他碰撞器之间的最小距离。尽管此属性适用于所有碰撞器，但对于动态碰撞器尤其重要。当影响动态碰撞体的力低于其刚体组件的 **Sleep threshold** 时，动态碰撞体处于静止状态。当动态碰撞体在与任何其他碰撞体接触时停止时，碰撞体将按其 **Rest offset** 值之和分隔。太大的 **Rest offset** 值可能会使动态实体显示为浮动。负的 **Rest offset** 值可能会使动态实体看起来相交。在碰撞器与实体的渲染网格不匹配的情况下，您可能需要调整此值。**Rest offset** 值必须小于 **Contact offset ** 值。 | Float: -Infinity to 50.0 | `0.0` |
| **Contact offset** | 设置与检测到碰撞的碰撞体的距离。当 PhysX 实体位于其 **Contact offset** 值之和内时，PhysX 实体会生成接触。**Contact offset** 值必须大于 **Rest offset** 值。| Float: 0.0 to 50.0 | `0.02` |
| **Shape** | 见 [Shape 属性](#shape-properties) | `Sphere`, `Box`, `Capsule`, `Cylinder` | `Box` |
| **Draw Collider** | 如果启用，碰撞器将显示在视区中。 | Boolean | `Enabled` |
| **Edit** | 进入碰撞器组件编辑模式以调整具有视区中操纵器的碰撞器的属性。 |  |  |

### Shape 属性
设置碰撞器组件的碰撞器。基元形状碰撞器不是网格。它们由描述长方体、球体、胶囊体或圆柱体的简单尺寸参数定义。基元形状碰撞器具有高性能，但它们可能无法准确表示 **Mesh** 组件提供的网格表面。

{{< tabs name="shape-ui" >}}
{{% tab name="Sphere" %}}

![PhysX Primitive Collider component interface, Sphere.](/images/user-guide/components/reference/physx/physx-collider-ui-03.png)

| 属性 | 说明 | 值 | 默认值 |
| - | - | - | - |
| **Radius** | 球体碰撞体的半径乘数。球体基元的大小是 **Radius** 值乘以实体的[Transform](/docs/user-guide/components/reference/transform/)组件中 **Scale** 属性中的最大值。 | Float: 0.0 to Infinity | `0.5` |

{{% /tab %}}
{{% tab name="Box" %}}

![PhysX Primitive Collider component interface, Box.](/images/user-guide/components/reference/physx/physx-collider-ui-04.png)

| 属性 | 说明 | 值 | 默认值 |
| - | - | - | - |
| **Dimensions** | 盒体碰撞体的宽度、深度和高度。| Vector3: 0.0 to Infinity | X: `1.0`, Y: `1.0`, Z: `1.0` |

{{% /tab %}}
{{% tab name="Capsule" %}}

![PhysX Primitive Collider component interface, Capsule.](/images/user-guide/components/reference/physx/physx-collider-ui-05.png)

| 属性 | 说明 | 值 | 默认值 |
| - | - | - | - |
| **Height** | 胶囊碰撞器的高度。胶囊体的 **Height** 值必须至少是 **Radius** 值的两倍。例如，如果胶囊体的 **Radius** 为 `5.0`，则最小 **Height** 为 `10.0`。 | Float: 0.0 to Infinity | `1.0` |
| **Radius** | 胶囊碰撞器的半径。胶囊的 **Radius** 值不能大于 **Height** 值的一半。例如，如果胶囊体的 **Height** 为`10.0`，则最大 **Radius** 为`5.0`。 | Float: 0.0 to Infinity | `0.25` |

{{% /tab %}}
{{% tab name="Cylinder" %}}

![PhysX Primitive Collider component interface, Cylinder.](/images/user-guide/components/reference/physx/physx-collider-ui-06.png)

| 属性 | 说明 | 值 | 默认值 |
| - | - | - | - |
| **Subdivision** | 圆柱体细分计数。 | Int: `3` to `125` | `16` |
| **Height** | 圆柱体碰撞体的高度。 | Float: 0.0 to Infinity | `1.0` |
| **Radius** | 圆柱体碰撞体的半径。 | Float: 0.0 to Infinity | `1.0` |

{{% /tab %}}
{{< /tabs >}}

## Collider component mode

在 collider （碰撞器） 组件模式下，您可以在视区中使用操纵器编辑碰撞器。要进入碰撞器组件模式，请在 **Entity Inspector** 中选择 PhysX Primitive Collider （PhysX 基元碰撞器） 组件属性底部的 **Edit** （编辑） 按钮。

### Sub component modes

碰撞器组件模式中提供了三种编辑模式。

| 模式 | 说明 |
| - | - |
| **Resize** | 编辑碰撞器尺寸。调整大小操纵器控制柄表示为黑色方块。|
| **Offset** | 相对于碰撞体的实体变换平移碰撞体。 |
| **Rotation** | 围绕组件的 **Offset** 旋转碰撞器。|

### Resize (Sphere Shape)

Sphere resize （球体大小调整） 模式具有一个线性操纵器，用于控制 **Radius （半径） 属性。

![PhysX Primitive Collider component mode sphere resize manipulator](/images/user-guide/components/reference/physx/physx-collider-resize-sphere.png)

### Resize (Box Shape)

长方体调整大小模式有六个线性操纵器，长方体的每一侧各一个。操纵器控制 **Dimensions** 属性的宽度、深度和高度方面。默认情况下，可以单独编辑长方体的每个面。按住 **Shift** 可对称地编辑相对的面对。

![PhysX Primitive Collider component mode box resize manipulator](/images/user-guide/components/reference/physx/physx-collider-resize-box.png)

### Resize (Capsule Shape)

Capsule resize （胶囊大小调整大小） 模式具有三个线性操纵器。胶囊顶部和底部的操纵器控制 **Height** 属性。默认情况下，胶囊的每一端都可以单独编辑。按住 **Shift** 可对称地编辑两端。侧面的操纵器控制 **Radius** 属性。

![PhysX Primitive Collider component mode capsule resize manipulator](/images/user-guide/components/reference/physx/physx-collider-resize-capsule.png)

### Resize (Cylinder Shape)

圆柱体调整大小模式具有三个线性操纵器。圆柱体顶部和底部的操纵器控制 **Height** 属性。默认情况下，可以单独编辑圆柱体的每一端。按住 **Shift** 可对称地编辑两端。侧面的操纵器控制 **Radius** 属性。

![PhysX Primitive Collider component mode cylinder resize manipulator](/images/user-guide/components/reference/physx/physx-collider-resize-cylinder.png)

### Offset

Offset （偏移） 模式具有三轴平移操纵器。

![PhysX Primitive Collider component mode offset manipulator](/images/user-guide/components/reference/physx/physx-collider-offset-mode.png)

### Rotation

旋转模式具有三轴旋转操纵器。

![PhysX Primitive Collider component mode rotate manipulator](/images/user-guide/components/reference/physx/physx-collider-rotate-mode.png)

### Collider component mode 快捷键

以下导航热键在 Collider 组件模式下可用。

| 快捷键 | 操作 |
| - | - |
| **1** | 偏移模式。 |
| **2** | 旋转模式。 |
| **3** | 调整大小模式。 |
| **Ctrl + Mouse Wheel Down** | 下一个模式。 |
| **Ctrl + Mouse Wheel Up** | 上一个模式。 |
| **R** | 重置当前模式。这实际上是撤消操作。您可以逐步完成 Resize、Offset 和 Rotation 模式，然后按 **R** 将更改重置为当前模式。 |
| **ESC** | 退出组件模式。 |

## Colliders as triggers将碰撞体用作触发器

触发器允许碰撞器执行高效的重叠测试。标记为触发器的碰撞体在与其他碰撞体相交时不会受到力的影响。这对于检测某物何时进入特定区域或两个对象何时重叠非常有用。使用 Lua 或 Script Canvas 检测重叠。

{{< note >}}
由于触发器不执行触点解析，因此触发器与另一个碰撞器之间的触点不可用。
{{< /note >}}
