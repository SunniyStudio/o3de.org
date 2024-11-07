---
title: Vegetation Distance Between Filter 组件
linktitle: Vegetation Distance Between Filter
description: 使用 Vegetation Distance Between Filter （过滤器之间的植被距离） 组件来控制 Open 3D Engine （O3DE） 关卡中植被实例之间的距离。
weight: 200
---

添加 **Vegetation Distance Between Filter** 组件以设置植被实例之间的最小距离。

## 提供者

[Vegetation Gem](/docs/user-guide/gems/reference/environment/vegetation/)

## 依赖

在使用 Vegetation Distance Between Filter （过滤器之间的植被距离） 组件时，添加以下必需组件之一：
- [Vegetation Layer Blender](./../vegetation/vegetation-layer-blender)
- [Vegetation Layer Blocker](./../vegetation/vegetation-layer-blocker)
- [Vegetation Layer Blocker (Mesh)](./../vegetation/vegetation-layer-blocker-mesh)
- [Vegetation Layer Spawner](./../vegetation/layer-spawner)

## Vegetation Distance Between Filter 属性

![Vegetation Distance Between Filter component properties](/images/user-guide/components/reference/vegetation-filters/vegetation-distance-between-filter-component.png)

| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **Allow Per-Item Overrides** | 如果为 '`Enabled`'，则启用的植被描述符属性可以覆盖此组件的属性。 | Boolean | `Disabled` |
| **Bound Mode** | 如果设置为 '`Radius`'，则 **Radius Min** 定义植被实例之间的最小半径。如果设置为 '`MeshRadius`'，则附加的 **Mesh** 组件的半径定义过滤器的半径。| `Radius` 或 `MeshRadius` | `Radius` |
| **Radius Min** | 设置植被实例之间的最小半径。 | Float: 0.0 to Infinity | `0.0` |

## DistanceBetweenFilterRequestBus

将以下请求函数与 '`DistanceBetweenFilterRequestBus`' 事件总线接口结合使用，以便与游戏中的 Vegetation Distance Between Filter 组件进行通信。
| 方法名称 | 说明 | 参数 | 返回值 | 可脚本化 |
|-|-|-|-|-|
| `GetAllowOverrides` | 返回 **Allow Per-Item Overrides** 属性的配置。 | None | Boolean | Yes |
| `GetBoundMode` | 返回 **Bound Mode** 属性的值。返回 '`Radius`' 的 '`0`' 和 '`MeshRadius`' 的 '`1`'。 | None | Integer | Yes |
| `GetRadiusMin` | 返回 **Radius Min** 属性的值。 | None | Float | Yes |
| `GetShapeEntityId` | 返回过滤器之间距离的 **Pin To Shape Entity Id** 属性。 | None | EntityId | Yes |
| `SetAllowOverrides` | 设置 **Allow Per-Item Overrides** 属性的配置。 | Boolean | None | Yes |
| `SetBoundMode` | 设置 **Bound Mode** 属性的值。'`0`' 表示 '`Radius`'，'`1`' 表示 '`MeshRadius`'。 | Integer | None | Yes |
| `SetRadiusMin` | 设置 **Radius Min** 属性的值。| Float | None | Yes |
| `SetShapeEntityId` | 设置过滤器之间距离的 **Pin To Shape Entity Id** 属性。  | EntityId | None | Yes |
