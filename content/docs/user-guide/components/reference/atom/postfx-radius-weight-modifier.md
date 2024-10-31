---
title: PostFX Radius Weight Modifier 组件
linktitle: PostFX Radius Weight Modifier
description: 'Open 3D Engine (O3DE) PostFx Radius Weight Modifier 组件参考。'
toc: true
---

**PostFx Radius Weight Modifier**组件可根据球体的半径对后处理特效（PostFX）进行加权。球体由**Radius**属性从实体的原点定义。球体周边的后置特效权重为`0.0`，当摄像机接近球体中心时，后置特效权重为`1.0`。


## 提供方

[Atom Gem](/docs/user-guide/gems/reference/rendering/atom/atom/)


## 依赖

[PostFX Layer 组件](/docs/user-guide/components/reference/atom/postfx-layer/)


## Base 属性

![PostFX Radius Weight Modifier base properties](/images/user-guide/components/reference/atom/post-processing-modifiers/postfx-radius-weight-modifier.png)

| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **Radius** | 控制 PostFX Volume球形边界的半径。 | `0.0` to infinity |`0.0` |
