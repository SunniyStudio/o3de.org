---
linktitle: Levels Gradient Modifier
title: Levels Gradient Modifier 组件
description: 使用Levels Gradient Modifier组件可修改Open 3D Engine (O3DE)中的输入和输出梯度值。
---

**Levels Gradient Modifier** 组件修改输入梯度的高、中、低值，并设置输出梯度的最小值和最大值。

## 提供方

[Gradient Signal Gem](/docs/user-guide/gems/reference/utility/gradient-signal)

## Levels Gradient Modifier 属性

![Levels Gradient Modifier component properties](/images/user-guide/components/reference/gradient-modifiers/levels-gradient-modifier-component.png)

| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **Preview** | 显示该组件应用所有属性后的输出渐变效果。 | | |
| **Pin Preview to Shape** | 设置一个具有兼容形状组件的实体，以便在**Constrain to Shape**为`Enabled`时用作预览的边界。 | EntityId | Current Entity |
| **Preview Position** | 设置预览的世界位置。<br> <br>只有在**Pin Preview to Shape**中未选择实体时，该字段才可用。 | Vector3: -Infinity to Infinity | X:`0.0`, Y:`0.0`, Z:`0.0` |
| **Preview Size** | 设置预览的尺寸。 | Vector3: 0.0 to Infinity | X:`1.0`, Y:`1.0`, Z:`1.0` |
| **Constrain to Shape** | 如果`Enabled`，渐变预览将使用在**Pin Preview to Shape**中选择的实体的边界。<br> <br>此字段仅在**Pin Preview to Shape**中选择了实体时可用。 | Boolean | `Disabled` |
| **Input Mid** | 设置输入梯度的中值。 | Float: 0.0 - 1.0 | `1.0` |
| **Input Min** | 设置输入梯度的最小值。 | Float: 0.0 - 1.0 | `0.0` |
| **Input Max** | 设置输入梯度的最大值。 | Float: 0.0 - 1.0 | `1.0` |
| **Output Min** | 设置输出梯度的最小值。 | Float: 0.0 - 1.0 | `0.0` |
| **Output Max** | 设置输出梯度的最大值。. | Float: 0.0 - 1.0 | `1.0` |
| **Gradient** | 请参阅下方的 [Gradient 属性](#gradient-properties)。 | | |

### Gradient properties

![Gradient properties](/images/user-guide/components/reference/vegetation-modifiers/gradient-properties.png)

| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **Gradient Entity Id** | 设置具有活动 **Gradient** 组件的实体。 | EntityId | None |
| **Opacity** | Sets the opacity of the input gradient. | Float: 0.0 - 1.0 | `1.0` |
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

## LevelsGradientRequestBus

使用以下带有`LevelsGradientRequestBus` EBus 接口的请求函数与游戏中的 Levels Gradient Modifier组件进行通信。

| 方法名称 | 说明 | 参数 | 返回值 | 可脚本化 |
|-|-|-|-|-|
| `GetGradientSampler` | 返回反转梯度的梯度采样器对象。 | None | Gradient Sampler | Yes |
| `GetInputMax` | 返回 **Input Max**的值。 | None | Max: Float | Yes |
| `GetInputMid` | 返回 **Input Mid**的值。 | None | Mid: Float | Yes |
| `GetInputMin` | 返回 **Input Min**的值。 | None | Min: Float | Yes |
| `GetOutputMax` | 返回 **Output Max**的值。 | None | Max: Float | Yes |
| `GetOutputMin` | 返回 **Output Min**的值。 | None | Min: Float | Yes |
| `SetInputMax` | 设置 **Input Max**的值。 | Max: Float | None | Yes |
| `SetInputMid` | 设置 **Input Mid**的值。 | Mid: Float | None | Yes |
| `SetInputMin` | 设置 **Input Min**的值。 | Min: Float | None | Yes |
| `SetOutputMax` | 设置 **Output Max**的值。 | Max: Float | None | Yes |
| `SetOutputMin` | 设置 **Output Min**的值。 | Min: Float | None | Yes |
