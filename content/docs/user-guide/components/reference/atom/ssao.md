---
title: SSAO 组件
linktitle: SSAO
description: 'Open 3D Engine (O3DE) SSAO 组件参考。'
toc: true
---

屏幕空间环境遮蔽（**SSAO**）组件通过使用深度缓冲区来近似场景中的光线遮蔽。这种遮蔽有时被称为**Contact Shadows**。

{{< image-width "/images/user-guide/components/reference/atom/ssao-bistro.png" "700" "Image of SSAO applied to the Bistro scene" >}}


## 提供方

[Atom Gem](/docs/user-guide/gems/reference/rendering/atom/atom/)


## 属性

{{< image-width "/images/user-guide/components/reference/atom/ssao-component.jpg" "500" "SSAO Component Properties" >}}

| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **Enable SSAO** | 如果启用，则激活 SSAO。默认情况下，即使关卡不包含 SSAO 组件，也会激活 SSAO。要停用 SSAO，请添加 SSAO 组件并禁用此属性。 | Boolean | Enabled |
| **SSAO Strength** | SSAO 效果的强度。该值越大，SSAO 的颜色越深。| 0.0 - 2.0 | 1.0 |
| **Sampling Radius** | 屏幕 UV 空间中 SSAO 效果的采样半径。减小或增大该值可分别获得更局部或全局的 SSAO 效果。 | 0.0 - 0.25 | 0.05 |
| **Enable Blur** | 如果启用，将对计算出的 SSAO 缓冲区进行模糊处理。激活模糊后，SSAO 图像看起来会更平滑。 | Boolean | Enabled |
| **Blur Strength** | 对 SSAO 采用多强的模糊效果。数值越低，模糊效果越弱，图像越清晰，噪点越少。| 0.0 - 0.95 | 0.85 |
| **Blur Sharpness** |  模糊效果的清晰度。数值越大，模糊效果越清晰，数值越小，模糊效果越柔和。SSAO 模糊会尽量尊重几何边缘，以避免 SSAO 在边缘或物体上模糊。 | 0.0 - 400.0 | 200.0 |
| **Blur Edge Threshold** | 边缘阈值，模糊效果会忽略指定值的深度变化。这个值越小，效果越好。 | 0.0 - 1.0 | 0.0 |
| **Enable Downsample** | 如果启用，则首先对深度缓冲区进行下采样，并对下采样结果执行 SSAO 和模糊处理，然后再对结果进行上采样。这种优化会使 SSAO 的细节更少。 | Boolean | Enabled |
