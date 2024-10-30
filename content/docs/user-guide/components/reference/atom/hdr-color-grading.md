---
title: HDR Color Grading 组件
linktitle: HDR Color Grading
description: 'Open 3D Engine (O3DE) HDR Color Grading 组件参考。'
toc: true
---

**HDR Color Grading**组件是一种后期处理效果，可让用户对场景进行调色，以匹配所需的外观和感觉。该组件在幕后使用着色器对每个帧的像素执行色彩分级逻辑。

为减少计算时间，请执行以下操作
1. 点击组件上的 **Generate LUT** 按钮，根据当前调色设置生成查找纹理（LUT）。

    ![HDR Color Grading Generate LUT](/images/user-guide/components/reference/atom/post-processing-modifiers/hdr-color-grading/hdr-color-grading-generate-lut-ui.png)

2. 然后，点击 **Activate LUT** 按钮激活生成的LUT。 

    ![HDR Color Grading Activate LUT](/images/user-guide/components/reference/atom/post-processing-modifiers/hdr-color-grading/hdr-color-grading-activate-lut-ui.png)

激活后，当前实体将添加**Look Modification**组件，随后**HDR Color Grading**组件将被禁用。

## 提供方 ##

[Atom Gem](/docs/user-guide/gems/reference/rendering/atom/atom/)

## 依赖

[PostFX Layer component](/docs/user-guide/components/reference/atom/postfx-layer/)

## 属性
### Color Adjustment ###

![HDR Color Grading Color Adjustment](/images/user-guide/components/reference/atom/post-processing-modifiers/hdr-color-grading/hdr-color-grading-component-color-adjustment-ui.png)

| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **Weight** | 色彩调整属性的强度。| Float: 0.0 - 1.0 | `1.0` |
| **Exposure** | 控制进入相机的光量。 | Float: -Infinity to Infinity | `0.0` |
| **Contrast** | 控制细节的清晰度。 | Float: -100.0 - 100.0 | `0.0` |
| **Pre Saturation** | 在色彩调整过程中控制色彩强度。| Float: -100.0 - 100.0 | `0.0` |
| **Filter Swatch** | 应用滤色器。 | 每通道八位色彩: 0 - 255 | `255,128,128` |
| **Filter Multiply** | 滤色效果的强度。 | Float: 0.0 - 1.0 | `0.0` |
| **Filter Intensity** | 控制滤色片色块的强度。 | Float: -Infinity to Infinity | `1.0` |

### White Balance ###

![HDR Color Grading White Balance](/images/user-guide/components/reference/atom/post-processing-modifiers/hdr-color-grading/hdr-color-grading-component-white-balance-ui.png)

| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **Weight** | 白平衡的强度。 | Float: 0.0 - 1.0 | `0.0` |
| **Temperature** | 控制白平衡的冷暖度。| Float: 1000.0 - 40000.0 | `6600.0` |
| **Tint** | 将白平衡从洋红色转为绿色。 | Float: -100.0 - 100.0 | `0.0` |
| **Luminance Preservation** | 在操作白平衡时保留表观亮度。 | Float: 0.0 - 1.0 | `1.0` |

### Split Toning ###

![HDR Color Grading Split Toning](/images/user-guide/components/reference/atom/post-processing-modifiers/hdr-color-grading/hdr-color-grading-component-split-toning-ui.png)

| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **Weight** | 切分调色特性的强度。 | Float: 0.0 - 1.0 | `0.0` |
| **Balance** | 控制阴影和高光颜色的混合方式。 | Float: -1.0 - 1.0 | `0.0` |
| **Shadows Color** | 控制阴影区域的颜色。 | 每通道八位色彩: 0 - 255 | `255,128,128` |
| **Highlights Color** | 控制高光区域的颜色。 | 每通道八位色彩: 0 - 255 | `26,255,26` |

### Shadows Midtones Highlights ###

![HDR Color Grading Shadows Midtones Highlights](/images/user-guide/components/reference/atom/post-processing-modifiers/hdr-color-grading/hdr-color-grading-component-shadow-midtones-higlights-ui.png)

| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **Weight** | **Shadow Midtones Highlights**属性的强度。 | Float: 0.0 - 1.0 | `0.0` |
| **Shadows Start** | 控制构成阴影的像素亮度值的起始值。 | Float: 0.0 - 16.0 | `0.0` |
| **Shadows End** | 控制构成阴影的像素亮度值的终点。 | Float: 0.0 - 16.0 | `0.3` |
| **Highlights Start** | 控制构成高光的像素亮度值的起始值。 | Float: 0.0 - 16.0 | `0.55` |
| **Highlights End** | 控制构成高光的像素亮度值的终点。 | Float: 0.0 - 16.0 | `1.0` |
| **Shadows Color** | 用于阴影的颜色。 | 每通道八位色彩: 0 - 255 | `255,64,64` |
| **Midtones Color** | 用于中间调的颜色，即介于阴影和高光之间的像素亮度值。 | 每通道八位色彩: 0 - 255 | `26`,`26`,`255` |
| **Highlights Color** | 用于高光的颜色。| 每通道八位色彩: 0 - 255 | `255,0,255` |

### Channel Mixing ###

![HDR Color Grading Channel Mixing](/images/user-guide/components/reference/atom/post-processing-modifiers/hdr-color-grading/hdr-color-grading-component-channel-mixing-ui.png)

| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **Channel Mixing Red** | 将红色通道转换成不同的颜色。 | Vector3: 0.0 to infinity | X:`1`,Y:`0`,Z:`0` |
| **Channel Mixing Green** | 将绿色通道转换成不同的颜色。 | Vector3: 0.0 to infinity | X:`0`,Y:`1`,Z:`0` |
| **CHannel Mixing Blue** | 将蓝色通道转换成不同的颜色。| Vector3: 0.0 to infinity | X:`0`,Y:`0`,Z:`1` |

### Final Adjustment ###

![HDR Color Grading Final Adjustment](/images/user-guide/components/reference/atom/post-processing-modifiers/hdr-color-grading/hdr-color-grading-component-final-adjustment-ui.png)

| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **Weight** | **Final Adjustment**属性的强度。 | Float: 0.0 - 1.0 | `1.0` |
| **Post Saturation** | 控制调色过程结束时应用的色彩强度。 | Float: -100.0 - 100.0 | `0.0` |
| **Hue Shift** | 将色彩转换成不同的色调。  | Float: 0.0 - 1.0 | `0.0` |

### LUT Generation ###

![HDR Color Grading LUT Generation](/images/user-guide/components/reference/atom/post-processing-modifiers/hdr-color-grading/hdr-color-grading-component-lut-generation-ui.png)

| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **LUT Resolution** | 设置生成 LUT 的分辨率。分辨率越高，生成的结果越接近原始色阶设置。 | `16x16x16`, `32x32x32`, or `64x64x64` | `16x16x16` |
| **Shaper Type** | 在 LUT 生成过程中应用整形功能，以匹配显示器的亮度。 | `Linear Custom Range`, `Log2 48 Nits`, `Log2 1000 Nits`, `Log2 2000 Nits`, `log2 4000 Nits`, `Log2 Custom Range`, `PQ (SMPTE ST 2084)` | `None` |
