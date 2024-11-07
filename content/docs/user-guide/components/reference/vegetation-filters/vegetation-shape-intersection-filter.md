---
title: Vegetation Shape Intersection Filter 组件
linktitle: Vegetation Shape Intersection Filter
description: 使用 Vegetation Shape Intersection Filter 组件可包含 Open 3D Engine （O3DE） 关卡中形状边界内的植被实例放置。
weight: 400
---

添加 **Vegetation Shape Intersection Filter** 组件，以包含在 **Shape** 组件边界内的植被或阻碍者实例放置。

## 提供者

[Vegetation Gem](/docs/user-guide/gems/reference/environment/vegetation/)

## 依赖

使用 Vegetation Shape Intersection Filter 组件时，添加以下必需组件之一：
- [Vegetation Layer Blender](./../vegetation/vegetation-layer-blender)
- [Vegetation Layer Blocker](./../vegetation/vegetation-layer-blocker)
- [Vegetation Layer Blocker (Mesh)](./../vegetation/vegetation-layer-blocker-mesh)
- [Vegetation Layer Spawner](./../vegetation/layer-spawner)

## Vegetation Shape Intersection Filter 属性

![Vegetation Shape Intersection Filter component properties](/images/user-guide/components/reference/vegetation-filters/vegetation-shape-intersection-filter-component.png)

| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **Filter Stage** | 定义是在修饰符之前还是之后应用滤镜。 | `PreProcess`, `PostProcess`, or `Default` | `Default` |
| **Shape Entity Id** | 选择具有 shape 组件的实体。实例仅放置在形状的边界内。 | EntityId | None |

## ShapeIntersectionFilterRequestBus

将以下请求函数与 '`ShapeIntersectionFilterRequestBus`' 事件总线接口结合使用，以便与游戏中的 Vegetation Shape Intersection Filter 组件进行通信。

| 方法名称 | 说明 | 参数 | 返回值 | 可脚本化 |
|-|-|-|-|-|
| `GetShapeEntityId` | 返回 Shape Intersection Filter 的 **Shape Entity Id** 属性。 | None | EntityId | Yes |
| `SetShapeEntityId` | 设置 Shape Intersection Filter 的 **Shape Entity Id** 属性。  | EntityId | None | Yes |
