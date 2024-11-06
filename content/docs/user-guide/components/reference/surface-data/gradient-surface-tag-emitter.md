---
linktitle: Gradient Surface Tag Emitter
title: Gradient Surface Tag Emitter 组件
description: 使用 Gradient Surface Tag Emitter 组件使渐变能够在 Open 3D Engine （O3DE） 关卡中发射表面标签。
---

将 **Gradient Surface Tag Emitter** 组件添加到实体，以使渐变能够发出表面标签。

## 提供者

[Surface Data Gem](/docs/user-guide/gems/reference/environment/surface-data)

## 依赖

将 Gradient Surface Tag Emitter 应用于实体时，该实体需要具有以下组件之一：

- [Dither Gradient Modifier](../gradient-modifiers/dither-gradient-modifier)
- [Gradient Mixer](../gradient-modifiers/gradient-mixer)
- [Invert Gradient Modifier](../gradient-modifiers/invert-gradient-modifier)
- [Levels Gradient Modifier](../gradient-modifiers/levels-gradient-modifier)
- [Posterize Gradient Modifier](../gradient-modifiers/posterize-gradient-modifier)
- [Smooth-Step Gradient Modifier](../gradient-modifiers/smooth-step-gradient-modifier)
- [Threshold Gradient Modifier](../gradient-modifiers/threshold-gradient-modifier)
- [Altitude Gradient](../gradients/altitude-gradient)
- [Constant Gradient](../gradients/constant-gradient)
- [FastNoise Gradient](../gradients/fastnoise-gradient)
- [Image Gradient](../gradients/image-gradient)
- [Perlin Noise Gradient](../gradients/perlin-noise-gradient)
- [Random Noise Gradient](../gradients/random-noise-gradient)
- [Reference Gradient](../gradients/reference-gradient)
- [Shape Falloff Gradient](../gradients/shape-falloff-gradient)
- [Slope Gradient](../gradients/slope-gradient)
- [Surface Mask Gradient](../gradients/surface-mask-gradient)

## Gradient Surface Tag Emitter 属性

![Gradient Surface Tag Emitter component properties](/images/user-guide/components/reference/surface-data/gradient-surface-tag-emitter-component.png)

| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **Preview** | 显示应用了 **Surface Bounds** 和 **Threshold** 属性的输入渐变的可视化预览。 |  |  |
| **Surface Bounds** | （可选）将渐变约束到具有 Shape 组件的实体的边界。 | Shape Entity: EntityId | None |
| **Threshold Min** | 设置允许应用标签的输入渐变的最小值。 | 0.0 - 1.0 | `0.1` |
| **Threshold Max** | 设置允许应用标签的输入渐变的最大值。 | 0.0 - 1.0 | `1.0` |
| **Extended Tags** | 渐变将发出的[surface tags](/docs/user-guide/gems/reference/environment/surface-data) 数组。| Array: Surface Tags | None |
