---
linktitle: Mesh Surface Tag Emitter
title: Mesh Surface Tag Emitter 组件
description: 使用 Mesh Surface Tag Emitter （网格表面标记发射器） 组件，使网格能够在 Open 3D Engine （O3DE） 关卡中发射表面标记。
---

将 **Mesh Surface Tag Emitter** 组件添加到实体，以使网格能够发出表面标签。

## 提供者

[Surface Data Gem](/docs/user-guide/gems/reference/environment/surface-data)

## 依赖

将 Mesh Surface Tag Emitter 应用于实体时，该实体需要具有以下组件之一：

- [Actor](../animation/actor)
- [Mesh](../atom/mesh)

## Mesh Surface Tag Emitter 属性

![Mesh Surface Tag Emitter component properties](/images/user-guide/components/reference/surface-data/mesh-surface-tag-emitter-component.png)

| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **Generated Tags** | 静态网格将发出的 [surface tags](/docs/user-guide/gems/reference/environment/surface-data) 数组。 | Array: Surface Tags | None |
