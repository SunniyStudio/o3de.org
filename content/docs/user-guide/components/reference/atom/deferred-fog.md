---
title: Deferred Fog 组件
linktitle: Deferred Fog
description: 使用 Open 3D Engine (O3DE) 中的Deferred Fog组件创建场景雾或分层/地面雾效果。
toc: true
---

**Deferred Fog**组件可创建屏幕空间雾效果，可用作场景雾或分层/地面雾。您可以使用噪点纹理为雾添加可选的**云湍流**效果。

云湍流是通过沿着雾层的光线行进并结合两个移动噪声倍频程来实现的，从而产生**云雾**效果。您可以通过缩放噪点纹理的 UV 坐标和定义纹理的移动速度来配置每个八度音阶。您还可以指定两个八度音阶之间的混合程度。

{{< note >}}
目前，Deferred Fog不会与照明产生交互作用。
{{< /note >}}


![Example of fog layer with turbulence](/images/user-guide/components/reference/atom/deferred-fog/basic-example.png)

## 提供方 ##

[Atom Gem](/docs/user-guide/gems/reference/rendering/atom/atom/)


## 依赖

[PostFX Layer 组件](./postfx-layer)


## 属性


![Deferred Fog component interface](/images/user-guide/components/reference/atom/deferred-fog/deferred-fog-component-ui.png)

| 属性 | 说明 | 值 | 默认值 |
| - | - | - | - |
| **Fog Color** |  逐渐与场景融合的雾的颜色。当雾向**Fog End Distance**融合时，颜色会变得更加不透明。  | Color | R: `115`, G: `115`, B: `153` |
| **Fog Start Distance** | 雾开始时与观众的距离，以米为单位。在这个距离上，雾开始逐渐与场景融合，直到在**Fog End Distance**上完全遮蔽背景场景。 | Float: 0.0 - 5000.0 | `1.0` |
| **Fog End Distance** | 雾完全遮蔽背景场景的距离（以米为单位）。从**Fog Start Distance**到**Fog End Distance**，雾会与场景融合，在场景和遮蔽的雾层之间形成渐变过渡。在此距离内外，场景呈现纯色，完全被雾层遮盖。  | Float: 0.0 - 5000.0 | `5.0` |
| **Fog Bottom Height** | 激活**Enable Fog Layer**时，雾层底部的高度（以米为单位）。 | Float: -5000.0 - 5000.0 | `0.01` |
| **Fog Max Height** | 激活**Enable Fog Layer**时，雾层顶部的高度，以米为单位。 | Float: -5000.0 - 5000.0 | `1.0` |
| **Noise Texture** | 单通道噪音纹理，用于在激活**Enable Turbulence Properties** 时定义雾湍流的外观。 | A `.streamingimage` 产品资产。  | `textures/cloudnoise_01.jpg.streamingimage` |
| **Noise Texture First Octave** | 缩放第一个八度的**Noise Texture**的 UV 坐标。 | Vector2: -Infinity to Infinity | X: `0.01`, Y: `0.01` |
| **Noise Texture First Octave Velocity** | 第一个八度噪音纹理的移动速度，单位为米/秒。 | Vector2: -Infinity to Infinity | X: `0.002`, Y:`0.0032` |
| **Noise Texture Second Octave** | 缩放第二个八度的**Noise Texture**的 UV 坐标。 | Vector2: -Infinity to Infinity | X: `0.0239`, Y: `0.0239` |
| **Noise Texture Second Octave Velocity** | 第二个八度噪音纹理的移动速度，单位为米/秒。 | Vector2: -Infinity to Infinity | X: `0.00275`, Y: `-0.004` |
| **Octaves Blend Factor** |第一个八度音和第二个八度音之间的混合程度。`1`的值只显示第一个八度音阶，而`0`的值只显示第二个八度音阶。 | Float: 0.0 - 1.0 | `0.4` |
| **Enable Fog Layer** | 如果启用，雾会被限制在沿 Z 轴的两点之间，从**Fog Bottom Height**到**Fog Max Height**。 | Boolean | `False` |
| **Enable Deferred Fog** | 如果启用，则激活场景中的雾化效果。 | Boolean | `False` |
| **Enable Turbulence Properties** | 如果启用，可通过使用**Noise Texture**在雾中产生云湍流。通过配置第一和第二倍频程来生成不同的云湍流。  | Boolean  | `False`  |
