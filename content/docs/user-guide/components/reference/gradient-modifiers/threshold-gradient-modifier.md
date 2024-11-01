---
linktitle: Threshold Gradient Modifier
title: Threshold Gradient Modifier 组件
description: 使用Threshold Gradient Modifier组件，可根据Open 3D Engine (O3DE)中的阈值将梯度值转换为`0` 或 `1`。
---

**Threshold Gradient Modifier** 组件对输入梯度应用阈值，以生成只有两个值的输出梯度。高于阈值的输入梯度值设为 1，低于阈值的输入值设为 0。

## 提供方

[Gradient Signal Gem](/docs/user-guide/gems/reference/utility/gradient-signal)

## Threshold Gradient Modifier 属性

![Threshold Gradient Modifier component properties](/images/user-guide/components/reference/gradient-modifiers/threshold-gradient-modifier-component.png)

| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **Preview** | 显示该组件应用所有属性后的输出渐变效果。 | | |
| **Pin Preview to Shape** | 设置一个具有兼容形状组件的实体，以便在**Constrain to Shape**为`Enabled`时用作预览的边界。 | EntityId | Current Entity |
| **Preview Position** | 设置预览的世界位置。<br> <br>只有在**Pin Preview to Shape**中没有选择实体时，此字段才可用。 | Vector3: -Infinity to Infinity | X:`0.0`, Y:`0.0`, Z:`0.0` |
| **Preview Size** | 设置预览的尺寸。 | Vector3: 0.0 to Infinity | X:`1.0`, Y:`1.0`, Z:`1.0` |
| **Constrain to Shape** | 如果`Enabled`，渐变预览将使用在**Pin Preview to Shape**中选择的实体的边界。<br> <br>此字段仅在**Pin Preview to Shape**中选择了实体时可用。| Boolean | `Disabled` |
| **Threshold** | 设置阈值。如果输入梯度的值小于或等于阈值，则设置为  `0.0`。 高于阈值的输入梯度值会被评估为`1.0`。 | Float: 0.0 - 1.0 | `0.5` |
| **Gradient** | 请参阅下面的 [Gradient 属性](#gradient-properties) 。 | | |

### Gradient 属性

![Gradient properties](/images/user-guide/components/reference/vegetation-modifiers/gradient-properties.png)

| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **Gradient Entity Id** | 设置具有活动 **Gradient** 组件的实体。 | EntityId | None |
| **Opacity** | 设置输入渐变的不透明度。 | Float: 0.0 - 1.0 | `1.0` |
| **Invert Input** | 反转输入梯度的值。 | Boolean | `Disabled` |
| **Preview (Input)** | 显示由 **Gradient Entity Id** 中设置的实体提供的渐变。 |  |  |
| **Enable Transform** | 如果`Enabled`，则可以修改输入梯度的平移、缩放和旋转。 | Boolean | `Disabled` |
| **Translate** | 设置输入梯度的平移。 | Vector3: -Infinity to Infinity | X:`0.0`, Y:`0.0`, Z:`0.0` |
| **Scale** | 设置输入梯度的比例。 | Vector3: 0.0001 to Infinity | X:`1.0`, Y:`1.0`, Z:`1.0` |
| **Rotate** | 设置输入梯度的旋转角度。 | Vector3: -Infinity to Infinity | X:`0.0`, Y:`0.0`, Z:`0.0` |
| **Enable Levels** | 如果 `Enabled`, 可以修改输入梯度的输入值和输出值。 | Boolean | `Disabled` |
| **Input Mid** | 设置输入梯度的中值。 | Float: 0.0 - 1.0 | `1.0` |
| **Input Min** | 设置输入梯度的最小值。 | Float: 0.0 - 1.0 | `0.0` |
| **Input Max** | 设置输入梯度的最大值。 | Float: 0.0 - 1.0 | `1.0` |
| **Output Min** | 设置输出梯度的最小值。 | Float: 0.0 - 1.0 | `0.0` |
| **Output Max** | 设置输出梯度的最大值。 | Float: 0.0 - 1.0 | `1.0` |

## ThresholdGradientRequestBus

使用以下带有 `ThresholdGradientRequestBus` EBus 接口的请求函数与游戏中的阈值渐变修改器组件进行通信。

| 方法名称 | 说明 | 参数 | 返回值 | 可脚本化 |
|-|-|-|-|-|
| `GetGradientSampler` | 返回阈值梯度的梯度采样器对象。 | None | Gradient Sampler | Yes |
| `GetThreshold` | 返回**Threshold**的值。 | None | Threshold: Float | Yes |
| `SetThreshold` | 设置**Threshold**的值。 | Threshold: Float | None | Yes |
