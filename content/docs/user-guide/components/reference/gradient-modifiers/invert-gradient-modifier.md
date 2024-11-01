---
linktitle: Invert Gradient Modifier
title: Invert Gradient Modifier 组件
description: 在Open 3D Engine (O3DE)中使用Invert Gradient Modifier 组件反转梯度。
---

**Invert Gradient Modifier** 分量反转梯度值。输出值等于 1 减去输入值。

## 提供方

[Gradient Signal Gem](/docs/user-guide/gems/reference/utility/gradient-signal)

## Invert Gradient Modifier 属性

![Invert Gradient Modifier component properties](/images/user-guide/components/reference/gradient-modifiers/invert-gradient-modifier-component.png)

| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **Preview** | 显示该组件应用所有属性后的输出渐变效果。 | | |
| **Pin Preview to Shape** | 设置一个具有兼容形状组件的实体，以便在**Constrain to Shape**为 `Enabled`时用作预览的边界。 | EntityId | Current Entity |
| **Preview Position** | 设置预览的世界位置。<br> <br>只有在**Pin Preview to Shape**中没有选择实体时，此字段才可用。 | Vector3: -Infinity to Infinity | X:`0.0`, Y:`0.0`, Z:`0.0` |
| **Preview Size** | 设置预览的尺寸。 | Vector3: 0.0 to Infinity | X:`1.0`, Y:`1.0`, Z:`1.0` |
| **Constrain to Shape** | 如果 `Enabled`，渐变预览将使用在**Pin Preview to Shape**中选择的实体的边界。<br> <br>此字段仅在**Pin Preview to Shape**中选择了实体时可用。| Boolean | `Disabled` |
| **Gradient** | 请参阅 [Gradient 属性](#gradient-properties)。 | | |

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
| **Enable Levels** | 如果`Enabled`，则可以修改输入梯度的输入值和输出值。 | Boolean | `Disabled` |
| **Input Mid** | 设置输入梯度的中值。 | Float: 0.0 - 1.0 | `1.0` |
| **Input Min** | 设置输入梯度的最小值。 | Float: 0.0 - 1.0 | `0.0` |
| **Input Max** | 设置输入梯度的最大值。 | Float: 0.0 - 1.0 | `1.0` |
| **Output Min** | 设置输出梯度的最小值。 | Float: 0.0 - 1.0 | `0.0` |
| **Output Max** | 设置输出梯度的最大值。 | Float: 0.0 - 1.0 | `1.0` |

## InvertGradientRequestBus

使用以下带有 `InvertGradientRequestBus` EBus 接口的请求函数与游戏中的反转渐变修改器组件进行通信。

| 方法名 | 说明 | 参数 | 返回值 | 可脚本化 |
|-|-|-|-|-|
| `GetGradientSampler` | 返回反转梯度的梯度采样器对象。 | None | Gradient Sampler | Yes |
