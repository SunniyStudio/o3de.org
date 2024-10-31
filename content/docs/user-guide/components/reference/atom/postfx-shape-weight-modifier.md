---
title: PostFX Shape Weight Modifier 组件
linktitle: PostFX Shape Weight Modifier
description: 'Open 3D Engine (O3DE) PostFX Shape Weight Modifier组件参考。'
toc: true
---

**PostFX Shape Weight Modifier**组件将后处理特效（PostFX）的修改限制在**Shape**组件所定义的体积内。后期特效的权重在形状组件的范围内保持不变，在范围外则根据**Fall-off Distance**降至`0.0`。

## 提供方 ##

[Atom Gem](/docs/user-guide/gems/reference/rendering/atom/atom/)


## 依赖

[PostFX Layer 组件](/docs/user-guide/components/reference/atom/postfx-layer/)

[Shape 组件](/docs/user-guide/components/reference/#shape)

## Base 属性

![PostFX Shape Weight Modifier base properties](/images/user-guide/components/reference/atom/post-processing-modifiers/postfx-shape-weight-modifier.png)

| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **Fall-off Distance** | 与形状组件外部边界的距离。在此距离内，PostFX 权重采用线性插值（lerped）。当从下降距离接近Shape分量的外部界限时，权重修改器会从`0.0`平滑过渡到`1.0`。 | `0.0` to infinity | `1.0` |
