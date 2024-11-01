---
linktitle: Shape Reference
title: Shape Reference Component
description: Use the Shape Reference component to reference and re-use shape components in your Open 3D Engine (O3DE) level.
weight: 100
---

Add the **Shape Reference** component to reference and re-use shape components in your **Open 3D Engine (O3DE)** level.

## Provider

[O3DE Core (LmbrCentral) Gem](/docs/user-guide/gems/reference/o3de-core)

## Sphere Shape properties

![Shape Reference component properties](/images/user-guide/components/reference/shape/shape-reference-component.png)

| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **Shape Entity Id** | Selects an entity with a valid Shape component. | Entity Id | None |


## ReferenceShapeRequestBus

Use the following request functions with the `ReferenceShapeRequestBus` EBus interface to communicate with Shape Reference components in your game.

| 方法名称 | 说明 | 参数 | 返回值 | 脚本化 |
|-|-|-|-|-|
| `GetShapeEntityId` | Returns the value of **Shape Entity Id**. | None | Entity Id | Yes |
| `SetShapeEntityId` | Sets the value of **Shape Entity Id**. | Entity Id | None | Yes |
