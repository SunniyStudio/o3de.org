---
title: Stars 组件
linktitle: Stars
description: 'Open 3D Engine (O3DE) Stars组件参考。'
toc: true
---

**Stars** 组件提供基于物理的动画，与分辨率无关。

恒星以广告牌的形式呈现，广告牌中心为白色，边缘逐渐变为恒星的颜色。广告牌被投射到远平面上，每颗星星的亮度会随时间变化，从而产生 “闪烁 ”效果。整个星域的方向由包含**Stars**组件的实体的**Transform**组件控制，因此您可以旋转星域来模拟地球的自转。

![An example of the star field that this component generates](/images/user-guide/components/reference/atom/stars/stars.png)

## 提供方

[Stars Gem](/docs/user-guide/gems/reference/rendering/stars/)

## 属性

![stars-component-base-properties](/images/user-guide/components/reference/atom/stars/stars-base-properties-ui.png)

| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **Exposure** | 指定渲染场景中星星时使用的曝光量，以便控制星星的亮度。 | `0.0` - `32.0` | `1.0` |
| **Radius Factor** | 指定每个星星的宽度和高度乘以的系数，以便设置星星的大小。 | `0.0` - `64.0`  | `7.0`  |
| **Stars Asset** | 恒星二进制数据文件，用于每颗恒星的位置、颜色和亮度。 |  | `default.stars` |
| **Twinkle Rate** | 指定星星闪烁的频率。 | `0.0` - `10.0` | `0.5` |

## .stars 二进制数据文件格式

引擎包含一个 `default.stars` 资产，您可以用它来创建星域。 要创建自己的自定义星域资产，需要生成一个包含以下数据的二进制`.stars`文件：

| 字段 | 说明 | 类型 | 值 |
| - | - | - | - |
| File Type Tag | .stars “文件中的第一个条目必须是 ”STAR "标记  | `uint32_t` | `0x52415453`|
| File Version | 目前只支持一种版本 | `uint32_t` | `0x00010001` |
| Number of stars | 文件中星星的数量 | `uint32_t` |  |
| Stars | 每颗星的数据结构（见下文） | `Star` |  |

`Star` 二进制类型是一个格式如下的结构：

| 字段 | 说明 | 类型 |
| - | - | - | 
| Ascension | 赤经，度 (0.0 - 24.0) | `float` | 
| Declination | 倾角，度 (-90.0 - 90.0)| `float` | 
| Red | 红色量 | `uint8_t` | 
| Green | 绿色量 | `uint8_t` | 
| Blue | 蓝色量 | `uint8_t` | 
| Magnitude | 星等（亮度）量 | `uint8_t` | 
