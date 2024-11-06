---
linkTitle: PhysX Heightfield Collider
title: PhysX Heightfield Collider 组件
description: 使用 PhysX Heightfield Collider  组件为 Open 3D Engine （O3DE） 中的地形等高度场创建碰撞。
toc: true
---

**PhysX Heightfield Collider** 组件根据 [Axis-Aligned Box Shape](/docs/user-guide/components/reference/shape/axis-aligned-box-shape/)组件提供的形状定义创建 NVIDIA PhysX 模拟碰撞器几何体。

{{< note >}}
附加到具有 **Axis-Aligned Box Shape 组件** 和 **Terrain Physics Collider** 的实体的 PhysX Heightfield Collider （PhysX 高度场碰撞器） 组件将创建静态（非移动）实体。
{{< /note >}}

## 提供者

[PhysX](/docs/user-guide/gems/reference/physics/nvidia/physx/)

## 依赖

[Axis-Aligned Box Shape](/docs/user-guide/components/reference/shape/axis-aligned-box-shape)

## PhysX Heightfield Collider 属性 

![\[PhysX Heightfield Collider component interface.\]](/images/user-guide/component/physx/physx/ui-physx-heightfield-collider-A.png)

| 属性 | 说明 | 值 | 默认值 |
| - | - | - | - |
| Collision Layer | 分配给此碰撞器的碰撞层。有关详细信息，请参阅 [碰撞图层](/docs/user-guide/interactivity/physics/nvidia-physx/configuring/configuration-collision-layers/). | Collision layer | `Default` |
| Collides With | 包含此碰撞器与之碰撞的图层的碰撞组。有关详细信息，请参阅 [碰撞组](/docs/user-guide/interactivity/physics/nvidia-physx/configuring/configuration-collision-groups/). | Collision group | `All` |
| Tag |  为此高度场碰撞器设置标签。标签可用于快速识别脚本或代码中的组件。 | String | |
| Rest offset |  PhysX 形体静止时，由其静止偏移值的总和分隔。**Rest offset **值必须小于**Contact offset**值。 | -Infinity to `50.0` | `0.0` |
| Contact offset | 当 PhysX 实体位于其接触偏移值的总和内时，它们会生成接触。**Contact offset** 值必须大于 **Rest offset** 值。 | `0.0` to `50.0` | `0.02` |
| Draw collider |  在视区中渲染此高度场碰撞器。默认情况下处于禁用状态。 | Boolean | `Disabled` |
| Use Baked Heightfield |  在动态生成的高度场或烘焙的高度场之间进行选择。烘焙高度场无法在运行时修改。动态 heightfield 可以在运行时通过更改 heightfield 提供程序来修改。默认情况下处于禁用状态。 | Boolean | `Disabled` |
| Baked Heightfield Relative Path |  显示生成的烘焙高度场资产路径的只读字段。 | String | |
| Bake Heightfield | 烘焙高度场资产。 | - | - |

## 作为触发器的碰撞体

触发器允许碰撞器执行高效的重叠测试。标记为触发器的碰撞体在与其他碰撞体相交时不会施加力。这对于检测某物何时进入特定区域或两个对象何时重叠非常有用。使用 Lua 或 Script Canvas 检测重叠。

{{< note >}}
由于触发器不执行触点解析，因此触发器与另一个碰撞器之间的触点不可用。
{{< /note >}}

## 控制台变量

PhysX Heightfield Collider （PhysX 高度场碰撞器） 有以下控制台变量可用：

| 名称 | 说明 | 值 | 默认值 |
| - | - | - | - |
| physx_heightfieldDebugDrawDistance | Distance for PhysX Heightfields （PhysX 高度场） 调试可视化 | Float | 50.0 |
| physx_heightfieldDebugDrawBoundingBox | 绘制用于高度场调试可视化的边界框 | Boolean | False |
