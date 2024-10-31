---
title: PostFX Gradient Weight Modifier 组件
linktitle: PostFX Gradient Weight Modifier
description: 'Open 3D Engine (O3DE) PostFX Gradient Weight Modifier 组件参考。'
toc: true
---

**PostFX Gradient Weight Modifier** 组件通过使用另一个实体的梯度信号作为遮蔽操作，来修改后处理特效（PostFX）的混合权重。


## 提供方

[Atom Gem](/docs/user-guide/gems/reference/rendering/atom/atom/)


## 依赖

[PostFX Layer 组件](/docs/user-guide/components/reference/atom/postfx-layer/)


## Gradient Sampler 属性

![PostFX Gradient Weight Modifier base properties](/images/user-guide/components/reference/atom/post-processing-modifiers/postfx-gradient-weight-modifier.png)
1
| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **Gradient Entity Id** | 对提供梯度的独立实体的引用。 | EntityId | None |
| **Opacity** | 设置输入渐变的不透明度。 | Float: 0.0 - 1.0 | `1.0` |
| **Invert Input** | 反转输入梯度的值。 | Boolean | `Disabled` |
| **Preview (Inbound)** | 显示由 **Gradient Entity Id** 中设置的实体提供的渐变。 |  |  |
| **Enable Transform** | 如果`Enabled`，则可以修改输入梯度的平移、缩放和旋转。 | Boolean | `Disabled` |
| **Translate** | 设置输入梯度的平移。 | Vector3: -Infinity to Infinity | X:`0.0`, Y:`0.0`, Z:`0.0` |
| **Scale** | 设置输入梯度的比例。 | Vector3: 0.0001 to Infinity | X:`1.0`, Y:`1.0`, Z:`1.0` |
| **Rotate** | 设置输入梯度的旋转角度。 | Vector3: -Infinity to Infinity | X:`0.0`, Y:`0.0`, Z:`0.0` |
| **Enable Levels** | 如果 `Enabled`，输入梯度的输入值和输出值可能会被修改。 | Boolean | `Disabled` |
| **Input Mid** | 设置输入梯度的中值。 | Float: 0.0 - 1.0 | `1.0` |
| **Input Min** | 设置输入梯度的最小值。| Float: 0.0 - 1.0 | `0.0` |
| **Input Max** | 设置输入梯度的最大值。 | Float: 0.0 - 1.0 | `1.0` |
| **Output Min** | 设置输出梯度的最小值。 | Float: 0.0 - 1.0 | `0.0` |
| **Output Max** | 设置输出梯度的最大值。 | Float: 0.0 - 1.0 | `1.0` |

## 用法

本例中有两个实体： ShaderBall 和 GradientSignalProvider。ShaderBall 实体的 PostFX 梯度权重修改器组件在其 `Gradient Entity Id` 属性中引用了 GradientSignalProvider 实体。PostFX 的权重取决于 GradientSignalProvider 输入的渐变。

有关梯度信号提供者的更多信息，请参阅 [Gradient 组件](/docs/user-guide/components/reference/#gradients)列表 和 [Gradient Signal Gem](/docs/user-guide/gems/reference/utility/gradient-signal/)。

![Using PostFX Gradient Weight Modifier example](/images/user-guide/components/reference/atom/post-processing-modifiers/postfx-gradient-weight-modifier-example-1.png)

![Using PostFX Gradient Weight Modifier example](/images/user-guide/components/reference/atom/post-processing-modifiers/postfx-gradient-weight-modifier-example-2.png)
