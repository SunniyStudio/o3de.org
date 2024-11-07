---
title: Vegetation Surface Mask Filter 组件
linktitle: Vegetation Surface Mask Filter
description: 使用 Vegetation Surface Mask Filter 组件，根据 Open 3D Engine （O3DE） 关卡中的表面标签权重分配植被。
weight: 700
---

添加 **Vegetation Surface Mask Filter** 组件，以定义具有表面标签和权重范围的植被和阻挡物放置区域。

## 提供者

[Vegetation Gem](/docs/user-guide/gems/reference/environment/vegetation/)

## 依赖

使用 Vegetation Surface Mask Filter 组件时，添加以下必需组件之一：
- [Vegetation Layer Blender](./../vegetation/vegetation-layer-blender)
- [Vegetation Layer Blocker](./../vegetation/vegetation-layer-blocker)
- [Vegetation Layer Blocker (Mesh)](./../vegetation/vegetation-layer-blocker-mesh)
- [Vegetation Layer Spawner](./../vegetation/layer-spawner)

## Vegetation Surface Mask Filter 属性

![Vegetation Surface Mask Filter component properties](/images/user-guide/components/reference/vegetation-filters/vegetation-surface-mask-filter-component.png)

| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **Filter Stage** | 定义是在修饰符之前还是之后应用滤镜。 | `PreProcess`, `PostProcess`, 或 `Default` | `Default` |
| **Allow Per-Item Overrides** | 如果为 '`Enabled`'，则启用的植被描述符属性可以覆盖此组件的属性。 | Boolean | `Disabled` |
| **Inclusion - Surface Tags** | 一个 [surface tags](/docs/user-guide/gems/reference/environment/surface-data) 数组，如果该位置的 surface 标记权重在 **Inclusion - Weight** 属性范围内，则允许放置植被或阻止程序实例。 | Array: Surface Tags | None |
| **Inclusion - Weight Min** | 设置允许放置植被的 **Inclusion - Surface Tag** 的最小权重。 | Float: 0.0 - 1.0 | `0.1` |
| **Inclusion - Weight Max** | 设置允许放置植被的 **Inclusion - Surface Tag** 的最大权重。 | Float: 0.0 - 1.0 | `1.0` |
| **Exclusion - Surface Tags** | 一个[surface tags](/docs/user-guide/gems/reference/environment/surface-data)数组，如果该位置的 surface 标记权重在 **Exclusion - Weight** 属性范围内，则不允许放置植被或阻止程序实例。 | Array: Surface Tags | None |
| **Exclusion - Weight Min** | 设置阻止植被的 **Exclusion - Surface Tag** 的最小权重。 | Float: 0.0 - 1.0 | `0.1` |
| **Exclusion - Weight Max** | 设置阻止植被的 **Exclusion - Surface Tag** 的最大权重。 | Float: 0.0 - 1.0 | `1.0` |

## SurfaceMaskFilterRequestBus

将以下请求函数与 '`SurfaceMaskFilterRequestBus`' 事件总线接口结合使用，以便与游戏中的 Vegetation Surface Mask Filter 组件进行通信。

| 方法名称 | 说明 | 参数 | 返回值 | 可脚本化 |
|-|-|-|-|-|
| `AddExclusiveTag` | 将曲面标记添加到 **Inclusion - Surface Tags** 数组中。 | Surface Tag: String | None | Yes |
| `AddInclusiveTag` | 将曲面标记添加到 **Inclusion - Surface Tags** 数组中。 | Surface Tag: String | None | Yes |
| `GetAllowOverrides` | 返回 **Allow Per-Item Overrides** 属性的配置。 | None | Boolean | Yes |
| `GetNumExclusiveTags` | 返回 **Exclusion - Surface Tags** 数组中的表面标记数。 | None | Count: Integer | Yes |
| `GetNumInclusiveTags` | 返回 **Inclusion - Surface Tags** 数组中的曲面标记数。 | None | Count: Integer | Yes |
| `GetExclusiveTag` | 返回 **Exclusion - Surface Tags** 数组的指定索引处的 surface 标记。 | Surface Tag Index: Integer | Surface Tag: String | Yes |
| `GetInclusiveTag` | 返回 **Inclusion - Surface Tags** 数组的指定索引处的 Surface 标记。 | Surface Tag Index: Integer | Surface Tag: String | Yes |
| `RemoveExclusiveTag` | 删除 **Exclusion - Surface Tags** 数组的指定索引处的表面标记。 | Surface Tag Index: Integer | None | Yes |
| `RemoveInclusiveTag` | 删除 **Inclusion - Surface Tags** 数组的指定索引处的 Surface 标记。 | Surface Tag Index: Integer | None | Yes |
| `SetAllowOverrides` | 设置 **Allow Per-Item Overrides** 属性的配置。 | Boolean | None | Yes |
