---
title: Terrain Height Gradient List 组件
linktitle: Terrain Height Gradient List
description: 使用 Open 3D Engine （O3DE） 关卡中的 Terrain Height Gradient List （地形高度渐变列表） 组件将渐变转换为高度数据。
---

**Terrain Height Gradient List** 从一个或多个梯度列表中提供地形系统的高度数据。通过缩放同一实体上[Axis Aligned Box Shape](/docs/user-guide/components/reference/shape/axis-aligned-box-shape)组件的高度来调整高度范围。

存在于同一实体上的 [Terrain Layer Spawner](/docs/user-guide/components/reference/terrain/layer_spawner) 将通过“`TerrainAreaHeightRequestBus`”查询 **Terrain Height Gradient List**。如果列表包含多个梯度，则将使用每个梯度中查询的每个位置的最高点。

## 提供者

[Terrain Gem](/docs/user-guide/gems/reference/environment/terrain)

## 依赖

[Axis Aligned Box Shape](/docs/user-guide/components/reference/shape/axis-aligned-box-shape)

[Terrain Layer Spawner](/docs/user-guide/components/reference/terrain/layer_spawner)

## Terrain Height Gradient List 属性

![Terrain Height Gradient List component properties](/images/user-guide/components/reference/terrain/terrain-height-gradient-list-component.png)

| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **Gradient Entities** | 具有 **Gradient** 组件的实体数组。 | Array: EntityId | None |

## TerrainAreaHeightRequestBus

将以下请求函数与 '`TerrainAreaHeightRequestBus`' 事件总线接口结合使用，以便与游戏的 Terrain Height Gradient List 组件进行通信。

| 方法名称 | 说明 | 参数 | 返回值 | 脚本化 |
|-|-|-|-|-|
| `GetHeight` | 返回 Query Position 的 Vector3，其中 Z 值更新为查询位置处的地形高度。 此外，还返回一个布尔值，该值指示 Query Position 处是否存在地形。 | Query Position: Vector3 | Terrain Height: Vector3, Terrain Exists: Boolean | No |
| `GetHeights` | 将 Vector3 列表作为输入，并返回 Vector3，其中 Z 值更新为查询位置的地形高度。 此外，还会更新布尔值列表，指示每个 Query Position 处是否存在地形。 | Query Positions: List of Vector3 | Terrain Height: List of Vector3, Terrain Exists: List of Boolean | No |
