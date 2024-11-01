---
linktitle: Dither Gradient Modifier
title: Dither Gradient Modifier 组件
description: 在Open 3D Engine (O3DE)中，使用Dither Gradient Modifier组件对渐变应用抖动效果。
---

**Dither Gradient Modifier**  组件将[有序抖动](https://en.wikipedia.org/wiki/Ordered_dithering) 算法应用于梯度。

## 提供方

[Gradient Signal Gem](/docs/user-guide/gems/reference/utility/gradient-signal)

## Dither Gradient Modifier 属性

![Dither Gradient Modifier component properties](/images/user-guide/components/reference/gradient-modifiers/dither-gradient-modifier-component.png)

| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **Preview** | 显示该组件应用所有属性后的输出渐变效果。 | | |
| **Pin Preview to Shape** | 设置一个具有兼容Shape组件的实体，以便在**Constrain to Shape**为`Enabled`时用作预览的边界。 | EntityId | Current Entity |
| **Preview Position** | 设置预览的世界位置。<br> <br>只有在**Pin Preview to Shape**中没有选择实体时，此字段才可用。 | Vector3: -Infinity to Infinity | X:`0.0`, Y:`0.0`, Z:`0.0` |
| **Preview Size** | 设置预览的尺寸。 | Vector3: 0.0 to Infinity | X:`1.0`, Y:`1.0`, Z:`1.0` |
| **Constrain to Shape** | 如果`Enabled`，渐变预览将使用在**Pin Preview to Shape**中选择的实体的边界。<br> <br>此字段仅在**Pin Preview to Shape**中选择了实体时可用。 | Boolean | `Disabled` |
| **Pattern Offset** | 移动模式的查找索引。 | Vector3: -Infinity to Infinity | X:`0.0`, Y:`0.0`, Z:`0.0` |
| **Pattern Type** | 设置抖动效果的模式。 | `4x4` or `8x8` | `4x4` |
| **Sample Settings - Use System Points Per Unit** |  如果`Enabled`，**Points Per Unit** 会自动设置为扇区密度除以扇区大小。 | Booelan | `Enabled` |
| **Sample Settings - Points Per Unit** | 设置梯度采样前缩放输入位置的值。| 0.001 to Infinity | `1.0` |
| **Gradient** | 请参阅下面的 [Gradient 属性](#gradient-properties)。 | | |

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
| **Enable Levels** | 如果 `Enabled`，则可以修改输入梯度的输入值和输出值。 | Boolean | `Disabled` |
| **Input Mid** | 设置输入梯度的中值。 | Float: 0.0 - 1.0 | `1.0` |
| **Input Min** | 设置输入梯度的最小值。 | Float: 0.0 - 1.0 | `0.0` |
| **Input Max** | 设置输入梯度的最大值。 | Float: 0.0 - 1.0 | `1.0` |
| **Output Min** | 设置输出梯度的最小值。 | Float: 0.0 - 1.0 | `0.0` |
| **Output Max** | 设置输出梯度的最大值。 | Float: 0.0 - 1.0 | `1.0` |

## DitherGradientRequestBus

使用以下带有 `DitherGradientRequestBus` EBus 接口的请求函数与游戏中的 Dither Gradient Modifier 组件进行通信。

| 方法名称 | 说明 | 参数 | 返回值 | 脚本化 |
|-|-|-|-|-|
| `GetGradientSampler` | 返回抖动梯度的梯度采样器对象。 | None | Gradient Sampler | Yes |
| `GetPatternOffset` | 返回**Pattern Offset**的值。 | None | Offset: Vector3 | Yes |
| `GetPatternType` | 返回**Pattern Type**的值。 | None | Pattern Type Index: Integer | Yes |
| `GetPointsPerUnit` | 返回**Points Per Unit**的值。 | None | Points: Float | Yes |
| `GetUseSystemPointsPerUnit` | 返回 **Use System Points Per Unit**的值。 | None | Boolean | Yes |
| `SetPatternOffset` | 设置**Pattern Offset**的值。 | Offset: Vector3 | None | Yes |
| `SetPatternType` | 设置**Pattern Type**的值。 | Pattern Type Index: Integer | None | Yes |
| `SetPointsPerUnit` | 设置**Points Per Unit**的值。 | Points: Float | None | Yes |
| `SetUseSystemPointsPerUnit` | 设置**Use System Points Per Unit**的值。 | Boolean | None | Yes |
