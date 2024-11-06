---
linktitle: PhysX Collider Surface Tag Emitter
title: PhysX Collider Surface Tag Emitter 组件
description: 使用 PhysX Collider Surface Tag Emitter （PhysX 碰撞器表面标记发射器） 组件使 PhysX 碰撞器能够在 Open 3D Engine （O3DE） 关卡中发射表面标记。
---

将 **PhysX Collider Surface Tag Emitter** 组件添加到实体，以使 PhysX 碰撞器能够发出表面标签。

## 提供者

[Surface Data Gem](/docs/user-guide/gems/reference/environment/surface-data)

## 依赖

将 PhysX Collider Surface Tag Emitter （PhysX 碰撞器表面标记发射器） 应用于实体时，该实体需要具有以下组件之一：

- [PhysX Primitive Collider](../physx/collider)
- [PhysX Mesh Collider](../physx/mesh-collider)
- [PhysX Heightfield Collider](../physx/heightfield-collider)
- [PhysX Shape Collider](../physx/shape-collider)

## PhysX Collider Surface Tag Emitter 属性

![PhysX Collider Surface Tag Emitter component properties](/images/user-guide/components/reference/surface-data/physx-collider-surface-tag-emitter-component.png)

| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **Generated Tags** | PhysX 碰撞器的外表面将发出的 [surface tags](/docs/user-guide/gems/reference/environment/surface-data) 数组。 | Array: Surface Tags | None |
| **Extended Tags** | PhysX 碰撞器内部将发出的 [surface tags](/docs/user-guide/gems/reference/environment/surface-data) 数组。 | Array: Surface Tags | None |
