---
title: Terrain Layer Spawner 组件
linktitle: Terrain Layer Spawner
description: Open 3D Engine (O3DE) Terrain Layer Spawner 参考。
weight: 100
---

**Terrain Layer Spawner** 组件在给定边界内生成一个地形图层，并控制其数据相对于其他重叠地形图层的优先级。

## 用法

[Terrain World](/docs/user-guide/components/reference/terrain/world) 组件为关卡启用地形系统，但在添加 Terrain Layer Spawner 之前，关卡将不包含任何地形。Terrain Layer Spawner 在给定的世界边界集内将地形区域生成到关卡中。然后，其他组件可以选择性地为 terrain 区域提供高度、颜色和表面数据的组合。

您可以通过修改同一实体上的[Axis Aligned Box Shape](/docs/user-guide/components/reference/shape/axis-aligned-box-shape) 组件来配置地形区域的世界维度。框的 XY 维度是 terrain 区域的 XY 维度，Z 维度表示该区域 terrain 的最小和最大高度。通过调整长方体的高度来修改地形的高度范围。如果高度范围超出 [Terrain World](/docs/user-guide/components/reference/terrain/world) 组件上定义的最小或最大地形世界高度，则高度将被限制以保持在地形系统限制范围内。

默认情况下，地形区域将生成一个平坦的地面，该表面在框的最小 Z 高度处填充 XY 维度。您可以使用 **Use Ground Plane** 设置来启用或禁用此功能。如果具有 Terrain Layer Spawner 的实体包含提供地形高度的组件，例如[Terrain Height Gradient List](/docs/user-guide/components/reference/terrain/height_gradient_list)，则将忽略该设置，并改用提供的高度。

Terrain Layer Spawner 的图层设置控制在存在重叠地形区域的地方，哪个 Spawner 的数据优先。通过首先分配一个图层（**Foregound**（较高优先级）或 **Background**（较低优先级）），然后使用 **Priority** 设置来控制优先级，较高的数字表示该图层中的优先级较高。高优先级的 Terrain Layer Spawner 将完全覆盖重叠区域中低优先级 Terrain Layer Spawner 提供的所有地形数据，无论高优先级地形是否具有任何有效的地形数据。例如，高优先级的 Terrain Layer Spawner 可以通过禁用 **Use Ground Plane** 设置在其他低优先级地形区域中创建洞。

## 提供者

[Terrain Gem](/docs/user-guide/gems/reference/environment/terrain)

## 依赖

[Axis-Aligned Box Shape](/docs/user-guide/components/reference/shape/axis-aligned-box-shape) 组件来定义 terrain 图层的世界边界。

[Terrain World](/docs/user-guide/components/reference/terrain/world) level 组件才能使 Terrain Layer Spawner 正常运行。没有它，关卡的地形系统将被禁用，并且 Terrain Layer Spawner 将不起作用。

[Terrain World Renderer](/docs/user-guide/components/reference/terrain/world-renderer) level 组件是可选的，但必须存在才能使 Terrain Layer Spawner 可见。没有它，地形图层数据将不会渲染，但底层数据仍将存在于世界中，并且可以通过 terrain API 进行查询。

## 属性

![Terrain Layer Spawner component properties](/images/user-guide/components/reference/terrain/terrain-layer-spawner-component.png)

| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **Layer Priority** | 刷怪箱的优先级。Foreground 的优先级高于 Background。 | Foreground or Background | `Foreground` |
| **Sub Priority** | 设置此生成器在层中的优先级。较高的数字将覆盖较低的数字。 | 0 - 10000 | 0 |
| **Use Ground Plane** | 启用此设置可提供未定义地形的默认地平面。 | Boolean | `True` |

## TerrainSpawnerRequestBus

“`TerrainSpawnerRequestBus`”是地形系统用于查询 Terrain Layer Spawner 设置的内部事件总线。其他系统通常不需要使用此事件总线，因为地形系统之外的任何内容都不需要有关单个 Terrain Layer Spawner 的任何信息。但是，如果出现使用案例，可以使用“`TerrainSpawnerRequestBus`”事件总线接口上的以下请求函数来查询各个 Terrain Layer Spawner 组件。

| 方法名称 | 说明 | 参数 | 返回值 | 脚本化 |
|-|-|-|-|-|
| `GetPriority` | 返回地形图层生成器的 **Layer Priority** 和 **Sub Priority**。 | None | Layer Priority: Integer; Sub Priority: Integer | No |
| `GetUseGroundPlane` | 返回 **Use Ground Plane** 的值。 | None | Boolean | No |
