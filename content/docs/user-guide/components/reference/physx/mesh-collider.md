---
linkTitle: PhysX Mesh Collider
title: PhysX Mesh Collider 组件
description: PhysX Mesh Collider （PhysX 网格碰撞器） 组件使用 PhysX 网格资产将 PhysX 碰撞器添加到实体，以便该实体可以包含在 PhysX 模拟中。
toc: true
---

**PhysX 网格碰撞器** 组件将 PhysX 碰撞器添加到实体，以便该实体可以包含在 PhysX 模拟中。碰撞器可以由您创建的物理资产、自动生成的凸面网格或已自动适应分解网格的形状来定义。PhysX Mesh Collider （PhysX 网格碰撞器） 组件还可以定义触发区域或力区域。

**PhysX Mesh Collider** 组件可以准确地表示 Mesh 组件提供的网格形状，但与 **PhysX Primitive Collider** 组件相比，性能成本更高。物理碰撞器资产基于由 Asset Processor 处理的网格。有关各种 PhysX 碰撞器资产类型以及如何处理它们的信息，请参阅 [处理 PhysX 碰撞器资产](/docs/learning-guide/tutorials/assets/physx-colliders/)。

{{< note >}}
添加一个带有 PhysX Mesh Collider （PhysX 网格碰撞器） 组件的 [PhysX Static Rigid Body](/docs/user-guide/components/reference/physx/static-rigid-body/)组件，以创建永不移动的 **静态** 实体。添加 [PhysX Dynamic Rigid Body](/docs/user-guide/components/reference/physx/rigid-body/) 组件以创建 **simulated模拟** 或 **kinematic运动学** 实体。模拟实体会随着碰撞和力而移动。运动实体不受碰撞或力的影响，但受脚本化运动驱动。
{{< /note >}}

## 提供者

[PhysX Gem](/docs/user-guide/gems/reference/physics/nvidia/physx/)

## 属性 

![PhysX Mesh Collider component interface.](/images/user-guide/components/reference/physx/physx-mesh-collider-ui-01.png)

### Base 属性

| 属性 | 说明 | 值 | 默认值 |
| - | - | - | - |
| **Collision Layer** | 将碰撞器分配给碰撞层。碰撞层可用于限制 PhysX 对象之间的物理交互。 | 在项目的 [Collision Layers](/docs/user-guide/interactivity/physics/nvidia-physx/configuring/configuration-collision-layers/) 中定义的任何碰撞图层。  | `Default` |
| **Collides With** | 将碰撞器分配给碰撞组。Collision group （碰撞组） 包含此碰撞器可以与之碰撞的碰撞层。 | 在工程的 [Collision Groups（碰撞组）](/docs/user-guide/interactivity/physics/nvidia-physx/configuring/configuration-collision-groups/) 中定义的任何碰撞组。 | `All` |
| **Trigger** | 如果启用，此碰撞器将用作触发器。Trigger 执行与其他碰撞器的快速重叠测试。触发器不施加力或返回接触点信息。使用此选项可加快 PhysX 计算速度，其中碰撞器之间的简单重叠测试就足够了。不支持将三角形网格作为触发器。 | Boolean | `Disabled` |
| **Simulated** | 如果启用，此碰撞器将包含在物理模拟中。 | Boolean | `Enabled` |
| **In Scene Queries** | 如果启用，则可以查询此碰撞器的光线投射、形状投射和重叠。 | Boolean | `Enabled` |
| **Offset** | 设置碰撞器相对于实体的局部偏移位置。 | Vector3: -Infinity to Infinity | X: `0.0`, Y: `0.0`, Z: `0.0` |
| **Rotation** | 设置碰撞器围绕 PhysX Mesh collider （PhysX 网格碰撞器） 组件的 **Offset** （偏移） 的局部旋转。 | Vector3: -180.0 to 180.0 | X: `0.0`, Y: `0.0`, Z: `0.0` |
| **Physics Materials** | 为此碰撞器的每个材质选择一种物理材质。物理材质定义表面的物理属性，例如动态和静态摩擦以及密度。网格碰撞体可以分配多个物理材质。 | 分配一个 `.physxmaterial` 资产 | `(default)` |
| **Tag** | 为此碰撞器设置一个标签。标签可用于快速识别脚本或代码中的组件。 | String | None |
| **Rest offset** | 设置此碰撞器与其他碰撞器之间的最小距离。尽管此属性适用于所有碰撞器，但对于动态碰撞器尤其重要。当影响动态碰撞体的力低于其刚体组件的 **Sleep threshold** 时，动态碰撞体处于静止状态。当动态碰撞体在与任何其他碰撞体接触时停止时，碰撞体将按其 **Rest offset （静止偏移） 值之和分隔。太大的 **Rest offset** 值可能会使动态实体显示为浮动。负的 **Rest offset**值可能会使动态实体看起来相交。在碰撞器与实体的渲染网格不匹配的情况下，您可能需要调整此值。**Rest offset**值必须小于**Contact offset**值。 | Float: -Infinity to 50.0 | `0.0` |
| **Contact offset** | 设置与检测到碰撞的碰撞体的距离。当 PhysX 实体位于其 **Contact offset** 值之和内时，PhysX 实体会生成接触。**Contact offset** 值必须大于 **Rest offset** 值。 | Float: 0.0 to 50.0 | `0.02` |
| **PhysX Mesh** | 为此碰撞器分配一个 `.pxmesh` 碰撞器产品资产。如果关联的网格或角色资产存在 `.pxmesh` 产品资产，则会自动设置此属性。有关创建 PhysX 网格资产碰撞器的更多信息，请参阅[处理 PhysX 碰撞器资产](/docs/learning-guide/tutorials/assets/physx-colliders/). | 产品资产 `.pxmesh` PhysX mesh. |  |
| **Asset Scale** | 独立于实体缩放碰撞器形状。 | Vector3: 0.0 to Infinity | X: `1.0`, Y: `1.0`, Z: `1.0` |
| **Physics Materials from Asset** | 如果启用，则此碰撞器的物理材质将根据网格的 PhysX 资产中的物理材质自动设置。启用此选项后，无法编辑 Physics 材质分配。 | Boolean | `Enabled`|
| **Draw Collider** | 如果启用，碰撞器将显示在视区中。 | Boolean | `Enabled` |
| **Edit** | 进入 collider component edit mode （碰撞器组件编辑模式） 以调整具有视区中操纵器的碰撞器的属性。 |  |  |

## Collider component mode碰撞组件模式

在 collider （碰撞器） 组件模式下，您可以在视区中使用操纵器编辑碰撞器。要进入碰撞器组件模式，请在 Entity Inspector （实体检查器） 中选择 PhysX Mesh Collider （PhysX 网格碰撞器） 组件属性底部的 **Edit** （编辑） 按钮。

### Sub component modes 子组件模式

碰撞器组件模式中提供了三种编辑模式。

| 模式 | 说明 |
| - | - |
| **Resize** | 调整大小操纵器表示为修改 **Asset Scale** 的缩放操纵器。|
| **Offset** | 相对于碰撞体的实体变换平移碰撞体。 |
| **Rotation** | 围绕组件的 **Offset** 旋转碰撞器。 |

### Resize

Physics Asset resize （物理资产大小调整） 模式具有三轴缩放操纵器。

![PhysX Mesh Collider component mode physics asset resize manipulator](/images/user-guide/components/reference/physx/physx-mesh-collider-resize.png)

### Offset

Offset （偏移） 模式具有三轴平移操纵器。

![PhysX Mesh Collider component mode offset manipulator](/images/user-guide/components/reference/physx/physx-mesh-collider-offset-mode.png)

### Rotation

旋转模式具有三轴旋转操纵器。

![PhysX Mesh Collider component mode rotate manipulator](/images/user-guide/components/reference/physx/physx-mesh-collider-rotate-mode.png)

### Collider component mode hotkeys

以下导航热键在 Collider 组件模式下可用。

| 快捷键 | 操作 |
| - | - |
| **1** | 偏移模式。 |
| **2** | 旋转模式。 |
| **3** | 调整大小模式。 |
| **Ctrl + Mouse Wheel Down** | 下一个模式。 |
| **Ctrl + Mouse Wheel Up** | 上一个模式。 |
| **R** | 重置当前模式。这实际上是撤消操作。您可以逐步浏览 Resize、Offset 和 Rotation 模式，然后按 **R** 将更改重置为当前模式。 |
| **ESC** | 退出组件模式。 |

## Colliders as triggers

触发器允许碰撞器执行高效的重叠测试。标记为触发器的碰撞体在与其他碰撞体相交时不会受到力的影响。这对于检测某物何时进入特定区域或两个对象何时重叠非常有用。使用 Lua 或 Script Canvas 检测重叠。

{{< note >}}
由于触发器不执行触点解析，因此触发器与另一个碰撞器之间的触点不可用。
不支持将三角形网格作为触发器。
{{< /note >}}
