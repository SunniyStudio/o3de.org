---
linkTitle: Chromatic Aberration
title: Chromatic Aberration 组件
description: Chromatic Aberration组件是由Open 3D Engine (O3DE) 中的 Atom Gem 提供的，它可以模拟将波长不同的光线聚焦到不同点的透镜效应。
toc: true
---

**Chromatic Aberration** 组件是一种后期处理效果，它模拟镜头将不同波长的光线聚焦在不同的点上，从而在图像边缘产生色彩条纹。

![Example of chromatic aberration effect](/images/user-guide/components/reference/atom/chromatic-aberration-example.png)

## 提供方

[Atom Gem](/docs/user-guide/gems/reference/rendering/atom/atom)

## 依赖

[PostFX Layer 组件](./postfx-layer)

## 属性

![Bloom component interface](/images/user-guide/components/reference/atom/chromatic-aberration-component.png)

| 属性 | 说明 | 值 | 默认值 |
| - | - | - | - |
| **Enable** | 如果启用，则激活色差效果。 | Boolean | `Disabled` |
| **Overrides - Enabled Override** | 如果启用，所有组件属性都将设置为**Overrides**属性组中指定的值。 | Boolean | `Enabled` |
| **Strength** | 控制色彩位移的大小。  | Float: 0.0 - 1.0 | `0.01` |
| **Blend** | 调整特效与原始图像的混合比例，从而降低特效的清晰度。 | Float: 0.0 - 1.0 | `0.5` |
