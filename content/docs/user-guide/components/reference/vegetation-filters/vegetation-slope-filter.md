---
title: Vegetation Slope Filter 组件
linktitle: Vegetation Slope Filter
description: 使用 Vegetation Slope Filter 组件在 Open 3D Engine （O3DE） 关卡的地形坡度限制内分布植被。
weight: 500
---

添加 **Vegetation Slope Filter** 组件，以将植被或阻碍器实例放置限制为一系列地形坡度值。

## 提供者

[Vegetation Gem](/docs/user-guide/gems/reference/environment/vegetation/)

## 依赖

使用 Vegetation Slope Filter 组件时，添加以下必需组件之一：
- [Vegetation Layer Blender](./../vegetation/vegetation-layer-blender)
- [Vegetation Layer Blocker](./../vegetation/vegetation-layer-blocker)
- [Vegetation Layer Blocker (Mesh)](./../vegetation/vegetation-layer-blocker-mesh)
- [Vegetation Layer Spawner](./../vegetation/layer-spawner)

## Vegetation Slope Filter 属性

![Vegetation Slope Filter component properties](/images/user-guide/components/reference/vegetation-filters/vegetation-slope-filter-component.png)

| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **Filter Stage** | 定义是在修饰符之前还是之后应用滤镜。 | `PreProcess`, `PostProcess`, 或 `Default` | `Default` |
| **Allow Per-Item Overrides** | 如果为 '`Enabled`'，则启用的植被描述符属性可以覆盖此组件的属性。 | Boolean | `Disabled` |
| **Slope Min** | 设置植被实例放置的最小地形坡度值。 | Float: 0.0 - 180.0 | `0.0` |
| **Slope Max** | 设置植被实例放置的最大地形坡度值。 | Float: 0.0 - 180.0 | `180.0` |

## SurfaceSlopeFilterRequestBus

将以下请求函数与 '`SurfaceSlopeFilterRequestBus`' 事件总线接口结合使用，以便与游戏中的 Vegetation Slope Filter 组件进行通信。

| 方法名称 | 说明 | 参数 | 返回值 | 可脚本化 |
|-|-|-|-|-|
| `GetAllowOverrides` | 返回 **Allow Per-Item Overrides** 属性的配置。 | None | Boolean | Yes |
| `GetAltitudeMax` | 返回 **Slope Max** 属性的值。 | None | Float | Yes |
| `GetAltitudeMin` | 返回 **Slope Min** 属性的值。 | None | Float | Yes |
| `SetAllowOverrides` | 设置 **Allow Per-Item Overrides** 属性的配置。 | Boolean | None | Yes |
| `SetAltitudeMax` | 设置 **Slope Max** 属性。 | Float | None | Yes |
| `SetAltitudeMin` | 设置 **Slope Min** 属性。 | Float | None | Yes |
