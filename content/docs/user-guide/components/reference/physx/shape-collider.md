---
linkTitle: PhysX Shape Collider
title: PhysX Shape Collider 组件
description: PhysX Shape Collider组件添加一个 PhysX 碰撞器，该碰撞器将形状组件利用到实体，以便该实体可以包含在 PhysX 模拟中。
toc: true
---

**PhysX Shape Collider**组件添加基于 **Shape** 组件的 PhysX 碰撞器，以便实体可以包含在 PhysX 模拟中。PhysX Shape Collider （PhysX 形状碰撞器） 组件还可以定义触发器区域或力区域。

{{< note >}}
添加带有 PhysX Shape Collider （PhysX 形状碰撞器） 组件的 [PhysX Static Rigid Body](/docs/user-guide/components/reference/physx/static-rigid-body/) 组件，以创建永不移动的 **static** 实体。添加 [PhysX Dynamic Rigid Body](/docs/user-guide/components/reference/physx/rigid-body/) 组件以创建 **模拟** 或 **运动学** 实体。模拟实体会随着碰撞和力而移动。运动实体不受碰撞或力的影响，但受脚本化运动驱动。
{{< /note >}}

## 提供者

[PhysX Gem](/docs/user-guide/gems/reference/physics/nvidia/physx/)

## 依赖

PhysX Shape Collider 需要以下 Shape 组件之一：

* [Box Shape](/docs/user-guide/components/reference/shape/box-shape/)
* [Capsule Shape](/docs/user-guide/components/reference/shape/capsule-shape/)
* [Cylinder Shape](/docs/user-guide/components/reference/shape/cylinder-shape/)
* [Polygon Prism Shape](/docs/user-guide/components/reference/shape/polygon-prism-shape/)
* [Quad Shape](/docs/user-guide/components/reference/shape/quad-shape/)
* [Sphere Shape](/docs/user-guide/components/reference/shape/sphere-shape/)

## 用例

尽管 PhysX Shape Collider （PhysX 形状碰撞器） 类似于 [PhysX Primitive Collider](/docs/user-guide/components/reference/physx/collider/) 组件，但您可能更愿意在以下情况下使用 PhysX Shape Collider （PhysX 形状碰撞器）：

* Shape 组件定义的形状信息在代码或脚本中的其他位置使用。例如，该形状定义了另一个体积（如音频体积），并且您希望使碰撞器几何体和体积保持同步。
* 您想要使用 Shape （形状） 组件，例如 Polygon Prism Shape （多边形棱柱形状），该组件不是由 PhysX Primitive Collider （PhysX 基元碰撞器） 提供的。
* 您有现有的 Shape （形状） 组件，并且不想迁移它们以使用 PhysX Primitive Collider （PhysX 基元碰撞器） 组件。

## 限制

与 PhysX Primitive Collider （PhysX 基元碰撞器） 组件相比，PhysX Shape Collider （PhysX 形状碰撞器） 组件有一些限制：

* 每个实体只能使用一个 Shape （形状） 组件，因此每个实体仅支持一个 PhysX Shape Collider （PhysX 形状碰撞器） 组件。但是，任意数量的 PhysX Primitive Collider （PhysX 基元碰撞器） 组件也可以用于同一实体。
* PhysX Shape Collider （PhysX 形状碰撞器） 组件的位置和旋转不能相对于实体位置偏移。

## 属性 

![PhysX Shape Collider component interface](/images/user-guide/components/reference/physx/physx-shape-collider-ui-01.png)

| 属性 | 说明 | 值 | 默认值 |
| - | - | - | - |
| **Collision Layer** | 将碰撞器分配给碰撞层。碰撞层可用于限制 PhysX 对象之间的物理交互。 | 在项目的 [Collision Layers](/docs/user-guide/interactivity/physics/nvidia-physx/configuring/configuration-collision-layers/) 中定义的任何碰撞图层。  | `Default` |
| **Collides With** | 将碰撞器分配给碰撞组。Collision group （碰撞组） 包含此碰撞器可以与之碰撞的碰撞层。 | 在工程的 [Collision Groups（碰撞组）](/docs/user-guide/interactivity/physics/nvidia-physx/configuring/configuration-collision-groups/) 中定义的任何碰撞组。 | `All` |
| **Trigger** | 如果启用，此碰撞器将用作触发器。Trigger 执行与其他碰撞器的快速重叠测试。触发器不施加力或返回接触点信息。使用此选项可加快 PhysX 计算速度，其中碰撞器之间的简单重叠测试就足够了。不支持将 Triangle meshes 和 **Quad Shapes** 作为触发器。 | Boolean | `Disabled` |
| **Simulated** | 如果启用，此碰撞器将包含在物理模拟中。 | Boolean | `Enabled` |
| **In Scene Queries** | 如果启用，则可以查询此碰撞器的光线投射、形状投射和重叠。 | Boolean | `Enabled` |
| **Physics Materials** | 为此碰撞器的每个材质选择一种物理材质。物理材质定义表面的物理属性，例如动态和静态摩擦以及密度。一个碰撞体可以分配多个物理材质。 | 分配的 `.physxmaterial` 资产 | `(default)` |
| **Tag** | 为此碰撞器设置一个标签。标签可用于快速识别脚本或代码中的组件。| String | None |
| **Rest offset** | 设置此碰撞器与其他碰撞器之间的最小距离。尽管此属性适用于所有碰撞器，但对于动态碰撞器尤其重要。当影响动态碰撞体的力低于其刚体组件的 **Sleep threshold** 时，动态碰撞体处于静止状态。当动态碰撞体在与任何其他碰撞体接触时停止时，碰撞体将按其 **Rest offset** 值之和分隔。太大的 **Rest offset** 值可能会使动态实体显示为浮动。负的 **Rest offset**值可能会使动态实体看起来相交。在碰撞器与实体的渲染网格不匹配的情况下，您可能需要调整此值。**Rest offset**值必须小于**Contact offset**值。 | Float: -Infinity to 50.0 | `0.0` |
| **Contact offset** | 设置与检测到碰撞的碰撞体的距离。当 PhysX 实体位于其 **Contact offset** 值之和内时，PhysX 实体会生成接触。**Contact offset** 值必须大于 **Rest offset** 值。 | Float: 0.0 to 50.0 | `0.02` |
| **Draw Collider** | 在视区中渲染碰撞体。| Boolean | `Enabled` |

## 复杂的多边形棱柱形状

[Polygon Prism Shape](/docs/user-guide/components/reference/shape/polygon-prism-shape/) 会自动细分为凸部分，这意味着多边形棱柱可以与动态刚体一起使用，或用作 PhysX 模拟中的触发器。如果修改了多边形棱柱的顶点，则细分会自动更新。

![A complex polygon prism can't be converted to convex geometry.](/images/user-guide/components/reference/physx/physx-shape-collider-polyprism.png)

如果修改了顶点，使多边形棱柱不再是简单的多边形，则无法将多边形棱柱细分为凸块。如果多边形棱柱无法细分为凸块，则 **O3DE 编辑器控制台**中将显示错误，如以下示例所示。

![A complex polygon prism console error.](/images/user-guide/components/reference/physx/physx-shape-collider-error.png)

## Colliders as triggers 

触发器允许碰撞器执行高效的重叠测试。标记为触发器的碰撞体在与其他碰撞体相交时不会受到力的影响。这对于检测某物何时进入特定区域或两个对象何时重叠非常有用。使用 Lua 或 **Script Canvas** 来检测重叠。

{{< note >}}
由于触发器不执行触点解析，因此触发器与另一个碰撞器之间的触点不可用。
不支持将 Triangle meshes 和 **Quad Shapes** 作为触发器。
{{< /note >}}
