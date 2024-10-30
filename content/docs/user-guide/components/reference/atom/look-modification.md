---
title: Look Modification 组件
linktitle: Look Modification
description: 'Open 3D Engine (O3DE) Look Modification 组件参考。'
toc: true
---

通过**Look Modification**组件，您可以使用查找表（LUT）应用调色后处理效果。在应用 LUT 之前，线性色彩值会使用**Shaper Type**属性中选择的传递函数进行改变。然后再将颜色复原为线性颜色。任何超出整形器范围的色彩值都将被箝位。对于低动态范围的内容，通常应使用`Log2 48 nits`整形器。对于高动态范围内容，通常应使用 `Log2 1000 nits` 或 `PQ(SMPTE ST 2084)`整形器。

## 提供方 ##

[Atom Gem](/docs/user-guide/gems/reference/rendering/atom/atom/)

## 依赖

[PostFX Layer 组件](postfx-layer)

## 属性

![Look modification component interface](/images/user-guide/components/reference/atom/look-modification/component.png)

| 属性 | 说明 | 值 | 默认值 |
| - | - | - | - |
| **Enable look modification** | 如果启用，则激活外观修改组件。 | Boolean | `Disabled` |
| **Color grading LUT** | LUT 的资产引用。| LUT Asset | `None` |
| **Shaper Type** | 用于在应用 LUT 之前塑造输入值的传递函数。<li>None: 在应用 LUT 之前不对颜色值进行整形。输入值将被箝位在 0.0 - 1.0 范围内。</li><li>Linear Custom Range: 将**Minimum Exposure**到**Maximum Exposure**的范围线性映射到 0.0 - 1.0 范围，然后应用 LUT。</li><li>Log2 48 nits: 沿 log2 曲线将 -6.5 至 +6.5 光圈值映射到 0.0 - 1.0 范围。这是低动态范围内容的标准整形器。</li><li>Log2 1000 nits: 沿对数 2 曲线将 -12.0 至 +10.0 档的数值映射到 0.0 - 1.0 范围内。这种形状针对 1000 nit 显示器的高动态范围内容进行了优化。</li><li>Log2 2000 nits: 沿对数 2 曲线将 -12.0 至 +11.0 档的数值映射到 0.0 - 1.0 范围内。这种形状针对 2000 nit 显示器的高动态范围内容进行了优化。</li><li>Log2 4000 nits: 沿对数 2 曲线将 -12.0 至 +12.0 档的数值映射到 0.0 - 1.0 范围内。这种形状针对 4000 nit 显示器的高动态范围内容进行了优化。</li><li>Log2 Custom Range: 沿 log2 曲线将所提供的停止值范围内的数值映射到 0.0 - 1.0 范围内。</li><li>PQ(SMPTE ST 2084): 沿感知量化曲线将 0 至 10,000 尼特的数值映射到 0.0 - 1.0 范围内。有关感知量化器曲线的更多信息，请参阅 https://en.wikipedia.org/wiki/Perceptual_quantizer.</li>|`None`,<br><nobr>`Linear Custom Range`</nobr>,<br>`Log2 48 nits`,<br>`Log2 1000 nits`,<br>`Log2 2000 nits`,<br>`Log2 4000 nits`,<br>`Log2 Custom Range`,<br>`PQ(SMPTE ST 2084)`| `Log2 48 nits` |
| **Minimum Exposure** | 该 LUT 支持的最小曝光量（光圈）。小于此值的值将被箝位为`0.0`。<br><br> 此字段仅在**Shaper Type**设置为自定义范围时可用。 | -50.0 - 0.0 | `-6.5` |
| **Maximum Exposure** | 该 LUT 支持的最大曝光量（光圈）。大于此值的值将被箝位为`1.0`。 <br><br> 该字段仅在**Shaper Type**设置为自定义范围时可用。 | 0.0 - 50.0 | `6.5` |
| **LUT intensity** | 该 LUT 的应用强度。 | 0.0 - 1.0 | `1.0` |
| **LUT override** | 该 LUT 相对于 PostFX 系统应用的其他 LUT 的强度。 | 0.0 - 1.0 | `1.0` |
