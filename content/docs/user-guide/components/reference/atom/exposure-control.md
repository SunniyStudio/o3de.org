---
title: Exposure Control 组件
linktitle: Exposure Control
description: 'Open 3D Engine (O3DE) Exposure Control 组件参考。'
toc: true
---

**Exposure Control** 组件调整相机从场景中接收的光量，从而控制场景的亮度。您可以手动设置曝光，或使用**Eye Adaptation**模式根据场景亮度自动调整相机曝光。眼睛适应模式会将场景调整为中性的中灰亮度。您可以通过增加或减少**Manual Compensation**属性来进一步调整自动曝光。


## 提供方 ##

[Atom Gem](/docs/user-guide/gems/reference/rendering/atom/atom/)


## 依赖

[PostFX Layer](/docs/user-guide/components/reference/atom/postfx-layer)


## 属性

![Exposure Control interface](/images/user-guide/components/reference/atom/exposure-control/exposure-control-component-ui.png)

### Base 属性

| 属性 | 说明 | 值 | 默认值 |
| - | - | - | - |
| **Enable** | 启用曝光控制属性。 | Boolean | `Enabled`  |
| **Overrides - Enabled Override** | 如果启用，您可以覆盖Exposure Control设置，从而对后期特效的表现和外观拥有更多控制权。 | Boolean | `Enabled` |
| **Control Type** | 指定如何控制曝光值。 <br><br>当**Control Type**为`Manual Only`时，只有**Manual Compensation**值以恒定的量调整场景的曝光值。<br><br>当**Control Type**为`Eye Adaptation`时，场景的曝光会自动调整为中性中灰亮度。然后，根据**Manual Compensation**进一步调整曝光。 | `Manual Only`, `Eye Adaptation` | `Manual Only` |
| **Manual Compensation** | 设置以曝光值档为单位的曝光补偿值。数值越小，曝光量越少，场景越暗。数值越大，场景越亮。 | Float: -16.0 - 16.0 | `0.0` |
| **Eye Adaptation** | 查看 [Eye Adaptation 属性](#eye-adaptation-properties).  |   |   |


### Eye Adaptation 属性

当 **Control Type** 设置为`Eye Adaptation`时，以下属性将激活。

| 属性 | 说明 | 值 | 默认值 |
| - | - | - | - |
| **Minimum Exposure** | 自动调整曝光的最小值。 | Float: -16.0 - 16.0 | `-16.0` |
| **Maximum Exposure** | 自动调整曝光的最大值。 | Float: -16.0 - 16.0 | `16.0` |
| **Speed Up** |自动曝光适应较亮场景的速度。数值越大，速度越快。 | Float: 0.01 - 10.0 | `3.0` |
| **Speed Down** | 自动曝光适应较暗场景的速度。数值越大，速度越快。 | Float: 0.01 - 10.0 |  `1.0` |
| **Enable Heatmap** | 显示摄像机在场景中捕捉到的曝光值直方图。它显示在**Viewport**的顶部。您可以移动摄像机查看其他区域的热图。低于**Minimum Exposure**值的区域用蓝色突出显示，高于**Maximum Exposure**值的区域用红色突出显示。热图上的数字代表曝光值档位。 | None | `Disabled` |
