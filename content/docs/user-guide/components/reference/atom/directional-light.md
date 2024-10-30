---
title: Directional Light 组件
linktitle: Directional Light
description: 'Open 3D Engine (O3DE) Directional Light 组件参考。'
toc: true
---

**Directional Light** 组件将光线从无限远的一点投向一个方向，类似于太阳光。该组件还支持阴影投射。


## 提供方 ##

[Atom Gem](/docs/user-guide/gems/reference/rendering/atom/atom/)


## Directional Light 属性

![Directional Light component interface.](/images/user-guide/components/reference/atom/light-component-ui/directional-light.png)

| 属性 | 说明 | 值 | 默认值 |
| - | - | - | - |
| **Color** | 光的颜色。颜色就像完全白色光源的遮罩或凝胶。例如，1000 勒克斯的白光实际上是 1000 勒克斯，但 1000 勒克斯的灰光只有 180 勒克斯。 | Color | `255, 255, 255` |
| **Intensity mode** | 允许以勒克斯或 EV100 为单位指定光值。勒克斯是线性光值，而 EV100 是对数光值（类似于相机光圈）。 | `Lux` or `Ev100` | `Ev100` |
| **Intensity** | 以**Intensity mode**设置的光度单位表示的光强度。 | 对于`Ev100`使用任意数，对`Lux`使用任意正数 | `4.0` |
| **Angular diameter** | 光源的角直径（度）。用于确保定向光的镜面高光不会在镜面光滑的表面上完全消失。从地球上看太阳的角直径约为 0.5°。  | 0.0 - 5.0 | `0.5` |
| **Shadow** | 参见下方的[Shadow 属性](#shadow-properties) 。 | | |
| **Global Illumination** | 参见下方的 [Global Illumination 属性](#global-illumination-properties)。 | | |

### Shadow 属性

| 属性 | 说明 | 值 | 默认值 |
| - | - | - | - |
| **Camera** | 从摄像机中可以看到阴影的实体。用于计算阴影级联的视场角。 | EntityId | None |
| **Shadow far clip** | 到阴影远夹角平面的距离。这是阴影投射到光源的最大距离。 | 0 到 无限 | `100.0` |
| **Shadowmap size** | 每个阴影贴图级联的纹理宽度和高度。 | `256`, `512`, `1024`, `2048` | `1024` |
| **Cascade count** | 阴影级联的数量。 | 1 - 4 | `4` |
| **Automatic splitting** | 如果启用，则使用**Split ratio**，否则使用**Far depth cascade**值。 | Boolean | Enabled |
| **Split Ratio** | 阴影级联的线性（0）和对数（1）分割方案之间的比率。 | 0.0 - 1.0 | `0.9` |
| **Far depth cascade** | 每个级联的远深度，x/y/z/w 表示级联 1/2/3/4。级联数小于 4 时，未使用的级联将被忽略。 | 0.0 到 **Shadow far clip** | X: 25.0, Y: 50.0, Z: 75.0, W: 100.0 |
| **Ground height** | 地面相对于摄像机的高度，用于校正级联的位置。 | -Infinity to Infinity | `0.0` |
| **Cascade correction** | 如果启用，将对阴影级联进行调整，以优化特定摄像机位置的外观。 | Boolean | Disabled |
| **Debug coloring** | 如果启用，阴影级联将以颜色显示其位置。红色、绿色、蓝色和黄色用于显示级联 1 至 4。 | Boolean | Disabled |
| **Shadow filter method** | 柔化阴影边缘的滤波方法 <ul><li>None: No filtering</li><li>PCF: Percentage-closer filtering</li><li>ESM: Exponential shadow maps</li><li>ESM+PCF: ESM with a PCF fallback</li></ul> | `None`, `PCF`, `ESM`, `ESM+PCF` | `None` |
| **Filtering sample count** | 沿边缘过滤阴影时使用的样本数。仅适用于 PCF 和 ESM+PCF。 | 4 - 64 | `32` |
| **Shadow Receiver Plane Bias Enable** | 如果启用，则会调整阴影接收器的平面，以减少大型 PCF 内核上的阴影。 | Boolean | Enabled | 
| **Shadow Bias** | 通过在阴影空间沿 Z 轴施加固定偏差来减少粉刺。 | 0.0 - 0.2 | `0.0015` |
| **Normal Shadow Bias** | 通过沿几何法线偏置阴影贴图查找，减少粉刺。 | 0.0 - 10.0 | `2.5` |
| **Blend between cascades** | 如果启用，阴影级联会彼此平滑融合。 | Boolean | Disabled |
| **Fullscreen Blur** | 如果启用，将对阴影应用全屏模糊。 | Boolean | Enabled |
| **Fullscreen Blur Strength** | 设置全屏阴影模糊的强度。 | 0 - 0.95 | `0.67` |
| **Fullscreen Blur Sharpness** | 设置全屏阴影边缘模糊的锐度。 | 0 - 400 | `50` |

### Global Illumination 属性

| 属性 | 说明 | 值 | 默认值 |
| - | - | - | - |
| **Affects GI** | 如果启用，该灯光会影响漫反射全局照明。 | Boolean | Enabled |
| **Factor** | 将此光照量乘以漫反射全局光照量。 | 0.0 - 2.0 | `1.0` |
