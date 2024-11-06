---
linktitle: Shape Surface Tag Emitter
title: Shape Surface Tag Emitter 组件
description: 使用 Shape Surface Tag Emitter 组件使形状能够在 Open 3D Engine （O3DE） 关卡中发出表面标记。
---

将 **Shape Surface Tag Emitter** 组件添加到实体，以使形状能够发出表面标签。

## 提供者

[Surface Data Gem](/docs/user-guide/gems/reference/environment/surface-data)

## 依赖

将 Shape Surface Tag Emitter 应用于实体时，该实体需要具有以下组件之一：

- [Axis Aligned Box Shape](../shape/axis-aligned-box-shape)
- [Box Shape](../shape/box-shape)
- [Capsule Shape](../shape/capsule-shape)
- [Compound Shape](../shape/compound-shape)
- [Cylinder Shape](../shape/cylinder-shape)
- [Disk Shape](../shape/disk-shape)
- [Polygon Prism Shape](../shape/polygon-prism-shape)
- [Quad Shape](../shape/quad-shape)
- [Shape Reference](../shape/shape-reference)
- [Sphere Shape](../shape/sphere-shape)
- [Tube Shape](../shape/tube-shape)

## Shape Surface Tag Emitter 属性

![Shape Surface Tag Emitter component properties](/images/user-guide/components/reference/surface-data/shape-surface-tag-emitter-component.png)

| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **Generated Tags** | 形状的外表面将发出的[surface tags](/docs/user-guide/gems/reference/environment/surface-data) 数组。 | Array: Surface Tags | None |
| **Extended Tags** | 形状内部将发出的[surface tags](/docs/user-guide/gems/reference/environment/surface-data) 数组。 | Array: Surface Tags | None |
