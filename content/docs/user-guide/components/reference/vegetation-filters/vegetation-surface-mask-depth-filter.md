---
title: Vegetation Surface Mask Depth Filter 组件
linktitle: Vegetation Surface Mask Depth Filter
description: 在 Open 3D Engine （O3DE） 关卡中使用 Vegetation Surface Mask Depth Filter 组件，将植被的分布限制在表面标签周围的区域。
weight: 600
---

添加 **Vegetation Surface Mask Depth Filter** 组件，以将植被或阻碍器实例放置在特定表面标签周围的区域。

## 提供者

[Vegetation Gem](/docs/user-guide/gems/reference/environment/vegetation/)

## 依赖

使用 Vegetation Surface Mask Depth Filter 组件时，添加以下必需组件之一：
- [Vegetation Layer Blender](./../vegetation/vegetation-layer-blender)
- [Vegetation Layer Blocker](./../vegetation/vegetation-layer-blocker)
- [Vegetation Layer Blocker (Mesh)](./../vegetation/vegetation-layer-blocker-mesh)
- [Vegetation Layer Spawner](./../vegetation/layer-spawner)

## Vegetation Surface Mask Depth Filter 属性

![Vegetation Surface Mask Depth Filter component properties](/images/user-guide/components/reference/vegetation-filters/vegetation-surface-mask-depth-filter-component.png)

| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **Filter Stage** | 定义是在修饰符之前还是之后应用滤镜。 | `PreProcess`, `PostProcess`, or `Default` | `Default` |
| **Allow Per-Item Overrides** | 如果为 '`Enabled`'，则启用的植被描述符属性可以覆盖此组件的属性。 | Boolean | `Disabled` |
| **Upper Distance Range** | 设置植被实例和 **Depth Comparison Tags** 之间的最大高度差。正值对应于 surface 标签上方的高度，负值对应于 surface 标签下方的高度。  | Float: -Infinity to Infinity | `1000.0` |
| **Lower Distance Range** | 设置 Vegetation 实例和 **Depth Comparison Tags** 之间的最小高度差。正值对应于 surface 标签上方的高度，负值对应于 surface 标签下方的高度。 | Float: -Infinity to Infinity | `-1000.0` |
| **Depth Comparison Tags** | 一个[surface tags](/docs/user-guide/gems/reference/environment/surface-data)数组，用于提供海拔数据以查询海拔高度。 | Array: Surface Tags | None |

## SurfaceMaskDepthFilterRequestBus

将以下请求函数与 '`SurfaceMaskDepthFilterRequestBus`' 事件总线接口结合使用，以便与游戏中的 Vegetation Surface Mask Depth Filter 组件进行通信。

| 方法名称 | 说明 | 参数 | 返回值 | 可脚本化 |
|-|-|-|-|-|
| `AddTag` | 将曲面标记添加到 **Depth Comparison Tags** 数组中。 | Surface Tag: String | None | Yes |
| `GetAllowOverrides` | 返回 **Allow Per-Item Overrides** 属性的配置。 | None | Boolean | Yes |
| `GetLowerDistance` | 返回 **Lower Distance Range** 属性的值。 | None | Float | Yes |
| `GetNumTags` | 返回 **Depth Comparison Tags** 数组中的表面标记数。 | None | Count: Integer | Yes |
| `GetTag` | 返回 **Depth Comparison Tags** 数组的指定索引处的 surface 标签。 | Surface Tag Index: Integer | Surface Tag: String | Yes |
| `GetUpperDistance` | 返回 **Upper Distance Range** 属性的值。 | None | Float | Yes |
| `RemoveTag` | 删除 **Depth Comparison Tags** 数组的指定索引处的表面标签。 | Surface Tag Index: Integer | None | Yes |
| `SetAllowOverrides` | 设置 **Allow Per-Item Overrides** 属性的配置。 | Boolean | None | Yes |
| `SetLowerDistance` | 设置 **Lower Distance Range** 属性的值。 | Float | None | Yes |
| `SetUpperDistance` | 设置 **Upper Distance Range** 属性的值。 | Float | None | Yes |
