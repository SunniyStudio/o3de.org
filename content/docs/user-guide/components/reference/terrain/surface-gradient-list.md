---
title: Terrain Surface Gradient List 组件
linktitle: Terrain Surface Gradient List
description: 'Open 3D Engine (O3DE) Terrain Surface Gradient List 组件参考。'
weight: 100
---

**Terrain Surface Gradient List** 组件定义地形图层上渐变和表面类型之间的映射。 将坡度指定给曲面时，坡度将定义该曲面在显示的地形中的强度。例如，您可以为岩石表面指定渐变，以使其部分穿过草地区域。

## 用法

Terrain 表面权重映射控制 terrain 表面上的表面类型的强度。同一点可以存在多个曲面类型和权重。例如，表面可以是 30% 的草、50% 的泥土和 20% 的岩石。其他系统查询这些权重以确定如何渲染地形、将哪些物理属性应用于地形等。

通过将包含渐变组件的实体拖动到 **GradientEntity** 字段，或者单击 {{< icon "picker.svg" >}} 来选择渐变。分配渐变后，您可以使用 **Surface Tag** 下拉菜单选择此渐变所代表的表面类型。您可以使用所需的 [Terrain Layer Spawner](/docs/user-guide/components/reference/terrain/layer_spawner) 配置图层的尺寸和优先级。

在此组件中可以定义多个渐变到表面类型的映射。每个渐变都需要映射到不同的表面类型。多个渐变映射的一个简单示例是具有两种表面类型，其中一个渐变是另一个渐变的倒数。您也可以拥有两个以上的可以混合在一起的。

## 提供者

[Terrain Gem](/docs/user-guide/gems/reference/environment/terrain)

## 依赖

[Terrain Layer Spawner](/docs/user-guide/components/reference/terrain/layer_spawner)

## 属性

![Terrain Surface Gradient List component properties](/images/user-guide/components/reference/terrain/terrain-surface-gradient-list-component.png)
2
| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **Gradient to Surface Mappings** | 要映射在一起的渐变实体和表面标记的数组。 |  |  |
| **GradientEntity** | 要分配给此层的渐变实体。 | Gradient Entity | None |
| **Surface Tag** | 设置此渐变表示的 [surface tag](/docs/user-guide/gems/reference/environment/surface-data)。<br><br>请参阅 [添加表面标签名称](/docs/user-guide/gems/reference/environment/surface-data/#adding-surface-tag-names)指南，了解如何创建自己的表面标签。 | Surface Tag | (unassigned) |

## TerrainAreaSurfaceRequestBus

“`TerrainAreaSurfaceRequestBus`”是地形系统用于查询各个 **Terrain Surface Gradient List** 组件的内部事件总线。其他系统通常不需要使用此 EBus，因为 terrain 系统之外的任何内容都不需要来自各个组件实例的任何信息。但是，如果出现使用案例，可以使用“`TerrainAreaSurfaceRequestBus`”事件总线接口上的以下请求函数来查询各个 **Terrain Surface Gradient List** 组件。

| 方法名称 | 说明 | 参数 | 返回值 | 脚本化 |
|-|-|-|-|-|
| `GetSurfaceWeights` | 返回分配给此组件的所有曲面，以及特定位置的渐变权重值。 | Position: Vector3 | Surface Weights: Surface Tag Weight List | No |
| `GetSurfaceWeightsFromList` | 返回分配给此组件的所有曲面，以及位置列表处渐变的权重值。 | Position: Vector3 List | Surface Weights: Surface Tag Weight List of Lists | No |
