---
linkTitle: Bloom
title: Bloom 组件
description: Bloom 组件可模拟真实世界中的光渗入（发光），由Open 3D Engine (O3DE).中的 Atom Gem 提供。
toc: true
---

**Bloom** 组件创建了**bloom**，这是一种后期处理效果，可以模拟现实世界中的光渗入或发光。


## 提供方

[Atom Gem](/docs/user-guide/gems/reference/rendering/atom/atom)


## 依赖

[PostFX Layer 组件](./postfx-layer)


## 属性

![Bloom component interface](/images/user-guide/components/reference/atom/bloom-component-ui.png)


### Bloom 属性

| 属性 | 说明 | 值 | 默认值 |
| - | - | - | - |
| **Enable Bloom** | 如果启用，则激活Bloom效果。| Boolean | `Disabled` |
| **Overrides - Enabled Override** | 如果启用，所有 Bloom 组件属性都将设置为**Overrides**属性组中指定的值。 | Boolean | `Enabled` |
| **Threshold** | Bloom 效果仅适用于亮度值大于此阈值的像素。 | Float: 0.0 to Infinity  | `1.0` |
| **Knee** | 在阈值以下和阈值以上的像素之间创建渐变过渡。这样可以柔化和扩散Bloom效果。  | Float: 0.0 - 1.0 | `0.5` |
| **Intensity** | 调整Bloom效果的强度。 | Float: 0.0 - 10000.0 | `0.5` |
| **Enable Bicubic** | 如果启用，则应用双三次滤波。这有助于减少可能出现的不良伪影。 | Boolean |  `Disabled` |
| **Kernel Size Scale** | 缩放内核的大小。 | Float: 0.0 - 2.0 | `1.0` |
| **Kernel Size 0 to 4** | 按渲染目标宽度的百分比平滑内核大小。例如，`0.04`的内核大小相当于在 100 x 100 像素的图像上使用 4 x 4 的内核，在 1000 x 1000 像素的图像上使用 40 x 40 的内核。数值越大，散射越多，计算成本越高。最大内核大小为 128。  | Float: 0.0 - 1.0 | 内核大小<br><ul><li>0: `0.04`</li><li>1: `0.08`</li><li>2: `0.16`</li><li>3: `0.32`</li><li>4: `0.64`</li></ul> |
| **Tint 0 to 4** | 为每个Bloom阶段添加色调。Bloom阶段通过相加混合产生最终效果。| Vector3: 0 - 255 | X: `255`. Y: `255`, Z: `255` |

