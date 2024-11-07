---
title: Vegetation Altitude Filter 组件
linktitle: Vegetation Altitude Filter
description: 使用 Vegetation Altitude Filter 组件将植被限制在 Open 3D Engine （O3DE） 关卡中的高度范围内。
weight: 100
---

添加 **Vegetation Altitude Filter** 组件，以将植被或阻挡器实例放置在特定高度范围内。

## 提供者

[Vegetation Gem](/docs/user-guide/gems/reference/environment/vegetation/)

## 依赖

使用 Vegetation Altitude Filter 组件时，添加以下必需组件之一：
- [Vegetation Layer Blender](./../vegetation/vegetation-layer-blender)
- [Vegetation Layer Blocker](./../vegetation/vegetation-layer-blocker)
- [Vegetation Layer Blocker (Mesh)](./../vegetation/vegetation-layer-blocker-mesh)
- [Vegetation Layer Spawner](./../vegetation/layer-spawner)

## Vegetation Altitude Filter 属性

![Vegetation Altitude Filter component properties](/images/user-guide/components/reference/vegetation-filters/vegetation-altitude-filter-component.png)

| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **Filter Stage** | 定义是在修饰符之前还是之后应用滤镜。 | `PreProcess`, `PostProcess`, or `Default` | `Default` |
| **Allow Per-Item Overrides** | 如果为 '`Enabled`'，则启用的植被描述符属性可以覆盖此组件的属性。 | Boolean | `Disabled` |
| **Pin To Shape Entity Id** | 如果选择了具有 **Shape** 组件的实体，则形状的边界将覆盖此组件的 **Altitude Min** 和 **Altitude Max** 属性。 | EntityId | None |
| **Altitude Min** | 设置植被实例放置的最小高度。 | Float: -Infinity to Infinity | `0.0` |
| **Altitude Max** | 设置植被实例放置的最大高度。 | Float: -Infinity to Infinity | `128.0` |

## SurfaceAltitudeFilterRequestBus

将以下请求函数与 '`SurfaceAltitudeFilterRequestBus`' 事件总线接口结合使用，以便与游戏中的植被高度过滤器组件进行通信。

| 方法名称 | 说明 | 参数 | 返回值 | 可脚本化 |
|-|-|-|-|-|
| `GetAllowOverrides` | 返回 **Allow Per-Item Overrides** 属性的配置。 | None | Boolean | Yes |
| `GetAltitudeMax` | 返回 **Altitude Max** 属性的值。 | None | Float | Yes |
| `GetAltitudeMin` | 返回 **Altitude Min** 属性的值。 | None | Float | Yes |
| `GetShapeEntityId` | 返回高度筛选器的 **Pin To Shape Entity Id** 属性。| None | EntityId | Yes |
| `SetAllowOverrides` | 设置 **Allow Per-Item Overrides** 属性的配置。 | Boolean | None | Yes |
| `SetAltitudeMax` | 设置 **Altitude Max** 属性。 | Float | None | Yes |
| `SetAltitudeMin` | 设置 **Altitude Min** 属性。 | Float | None | Yes |
| `SetShapeEntityId` | 设置高度筛选器的 **Pin To Shape Entity Id** 属性。  | EntityId | None | Yes |
