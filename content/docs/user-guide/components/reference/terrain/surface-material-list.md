---
title: Terrain Surface Materials List 组件
linktitle: Terrain Surface Materials List
description: 'Open 3D Engine (O3DE) Terrain Surface Materials List 组件参考。'
weight: 100
---

**Terrain Surface Materials List** 组件定义表面类型和渲染材质之间的映射。您可以使用它来更改游戏不同区域中 Surface 类型的外观。当您将材质分配给表面类型时，在所需的 [Axis Aligned Box Shape](/docs/user-guide/components/reference/shape/axis-aligned-box-shape/) 组件的边界内，该类型的所有可见表面都将采用该材质。

此组件在大型地形表面上使用 [宏材质](/docs/user-guide/components/reference/terrain/terrain-macro-material) 和 [详细材质](/docs/user-guide/components/reference/terrain/terrain-detail-material/) 并在它们之间进行混合。这种细节材质与宏材质的混合使您能够使用小型高保真平铺细节材质，这些材质在整个地形中的颜色和光照都有变化。

宏观和细节材质混合基于材质的表面权重。在每个点上具有最高权重值的三种曲面材质混合在一起，以达到 100% 或 1.0。例如，假设您有以下表面材质和权重：
* Grass = 0.25
* Dirt = 0.125
* Sand = 0.125
* Rock = 0.05

在这种情况下，混合会忽略岩石，结果是 50% 的草、25% 的泥土和 25% 的沙子。

有关如何将详细材质与表面材质列表一起使用的示例，请按照**从图像创建地形**教程的 [应用详细材质](/docs/learning-guide/tutorials/environments/create-terrain-from-images/#apply-detail-materials) 部分进行操作。

## 用法
使用 **SurfaceTag** 下拉菜单选择表面类型标签，然后通过单击 {{< icon "file-folder.svg" >}} 并选择材质，或者从 AssetBrowser 窗口拖动材质来分配材质。

## 提供者

[Terrain Gem](/docs/user-guide/gems/reference/environment/terrain)

## 依赖

[Axis-Aligned Box Shape](/docs/user-guide/components/reference/shape/axis-aligned-box-shape)

## 属性

![Terrain Surface Materials List component properties](/images/user-guide/components/reference/terrain/terrain-surface-materials-list-component.png)

| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **Default Material** | 当不存在其他材质表面贴图时要回退到的默认材质。<br><br>**注意：** 默认材质不与其他材质混合，因为它没有表面权重。默认材质的主要预期用途是作为错误材质来查看细节材质尚未映射的每个位置，或者使用单个材质快速覆盖整个地形表面，而无需设置更复杂的映射。 | Material Asset | None |
| **Material Mappings** | 要映射在一起的[surface tags](/docs/user-guide/gems/reference/environment/surface-data)和材质资源的数组。 |  |  |
| **Surface Tag** | 选择要映射到材质的曲面标记。 | Surface:  Surface Tag | None |
| **Material Asset** | 选择要应用于表面的材质资源。 | Material Asset | None |

## TerrainAreaMaterialRequestBus

将以下请求函数与 '`TerrainAreaMaterialRequestBus`' 事件总线接口结合使用，以便与游戏的 Surface Material List 组件进行通信。


| 方法名称 | 说明 | 参数 | 返回值 | 脚本化 |
|-|-|-|-|-|
| `GetTerrainSurfaceMaterialRegion` | 检索存在 '`TerrainSurfaceMaterialMapping`' 的区域的 Aabb。  | None | Aabb | No |
| `GetSurfaceMaterialMappings` | 检索所有已分配的曲面类型、您为其分配的材质以及为此实体设置的边界。  | None | Terrain Surface Material Mapping: Vector | No |
| `GetDefaultMaterial` | 检索此表面材质的默认材质。  | None | Terrain Surface Material Mapping | No |

## TerrainAreaMaterialNotificationBus

| 通知名称 | 说明 | 参数 | 返回值 | 可脚本化 |
|-|-|-|-|-|
| `OnTerrainDefaultSurfaceMaterialCreated` | 在分配并加载默认 Surface 材质时通知侦听器。 | None | EntityId; Material | No |
| `OnTerrainDefaultSurfaceMaterialDestroyed` | 在取消分配默认表面材质时通知侦听器。 | None | EntityId | No |
| `OnTerrainDefaultSurfaceMaterialChanged` | 当默认 Surface 材质更改为其他材质时通知听者。 | None | EntityId; Material | No |
| `OnTerrainSurfaceMaterialMappingCreated` | 在 Surface 和 Material 之间设置新映射时通知听者。| None | EntityId; Surface Tag; Material | No |
| `OnTerrainSurfaceMaterialMappingDestroyed` | 在移除 Surface 和 Material 之间的映射时通知侦听器。 | None | EntityId; Surface Tag | No |
| `OnTerrainSurfaceMaterialMappingTagChanged` | 当 surface 标签更改为现有材质的 tag 时通知侦听器。 | None | EntityId; Surface Tag; Surface Tag | No |
| `OnTerrainSurfaceMaterialMappingMaterialChanged` | 当现有 surface 标签的材质发生更改时通知侦听器。 | None | EntityId; Surface Tag; Material | No |
| `OnTerrainSurfaceMaterialMappingRegionCreated` | 在创建一组表面材质映射时通知侦听器。 | None | EntityId; Aabb | No |
| `OnTerrainSurfaceMaterialMappingRegionDestroyed` | 当一组表面材质映射被销毁时通知侦听者。 | None | EntityId; Aabb | No |
| `OnTerrainSurfaceMaterialMappingRegionChanged` | 当这组表面材质映射的边界发生更改时通知侦听器。 | None | EntityId; Old Region: Aabb; New Region: Aabb | No |
