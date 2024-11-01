---
linktitle: Gradient Mixer
title: Gradient Mixer 组件
description: 使用Gradient Mixer组件将渐变与Open 3D Engine (O3DE)中的操作结合起来。
---

**Gradient Mixer** 组件通过层混合操作混合多个输入梯度层，生成新的梯度。

## 提供方

[Gradient Signal Gem](/docs/user-guide/gems/reference/utility/gradient-signal)

## Gradient Mixer 属性

![Gradient Mixer component properties](/images/user-guide/components/reference/gradient-modifiers/gradient-mixer-component.png)

| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **Preview** | 在世界空间中显示该组件应用所有属性后的输出渐变效果。选择 **Show Larger Preview**按钮，可查看一个单独的可停靠窗口，其中包含更大的预览渐变效果。 | | |
| **Pin Preview to Shape** | 设置一个具有兼容Shape组件的实体，作为预览的位置和边界框。 | EntityId | Current Entity |
| **Preview Position** | 设置预览的世界位置。<br> <br>只有在**Pin Preview to Shape**中没有选择实体时，此字段才可用。 | Vector3: -Infinity to Infinity | X:`0.0`, Y:`0.0`, Z:`0.0` |
| **Preview Size** | 设置预览的尺寸。 | Vector3: 0.0 to Infinity | X:`1.0`, Y:`1.0`, Z:`1.0` |
| **Constrain to Shape** | 如果`Enabled`，渐变预览只会显示在**Pin Preview to Shape**中所选实体形状内的渐变。<br> <br>此字段仅在**Pin Preview to Shape**中选择了实体时可用。 | Boolean | `Disabled` |
| **Layers** | 一个包含梯度和梯度运算的数组。 梯度运算的应用顺序与该数组相同。 |  |  |
| **Layers - Enabled** | 切换该渐变层的影响力。 | Boolean | `Enabled` |
| **Layers - Operation** | 设置用于将此梯度图层与之前所有图层的结果混合的函数。更多信息请参考后面的 [梯度混合操作](#gradient-mixing-operations)部分。  | `Initialize`, `Multiply`, `Linear Dodge (Add)`, `Subtract`, `Darken (Min)`, `Lighten (Max)`, `Average`, `Normal`, or `Overlay`. | `Initialize` |
| **Layers - Gradient** | 请参阅下面的 [Gradient 属性](#gradient-properties) 。 | | |

### Gradient 属性

![Gradient properties](/images/user-guide/components/reference/vegetation-modifiers/gradient-properties.png)

| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **Gradient Entity Id** | 设置具有活动 **Gradient** 或 **Gradient Modifier** 组件的实体。 | EntityId | None |
| **Opacity** | 设置输入渐变的不透明度。 | Float: 0.0 - 1.0 | `1.0` |
| **Invert Input** | 反转输入梯度的值。 | Boolean | `Disabled` |
| **Preview (Input)** | 显示由 **Gradient Entity Id** 中设置的实体提供的渐变，并应用以下渐变属性。 |  |  |
| **Enable Transform** | 如果`Enabled`，则可以修改输入梯度的平移、缩放和旋转。 | Boolean | `Disabled` |
| **Translate** | 设置输入梯度的平移。 | Vector3: -Infinity to Infinity | X:`0.0`, Y:`0.0`, Z:`0.0` |
| **Scale** | 设置输入梯度的比例。 | Vector3: 0.0001 to Infinity | X:`1.0`, Y:`1.0`, Z:`1.0` |
| **Rotate** | 设置输入梯度的旋转角度。 | Vector3: -Infinity to Infinity | X:`0.0`, Y:`0.0`, Z:`0.0` |
  | **Enable Levels** | 如果`Enabled`，则输入梯度的输入值和输出值可通过以下属性进行修改。 | Boolean | `Disabled` |
| **Input Mid** | 重映射输入梯度中大于`0`和小于`1`值的中点。**Input Mid**值为 `0.5`，则输出渐变效果较暗。 如果**Input Mid**值为`2.0`，则输出渐变效果较浅。| Float: 0.0 - 10.0 | `1.0` |
| **Input Min** | 重映射输入梯度，使其位于**Input Min**和**Input Max**之间。 所有小于或等于**Input Min**的输出梯度值都会设置为`0`。 | Float: 0.0 - 1.0 | `0.0` |
| **Input Max** | 重映射输入梯度，使其位于**Input Min**和**Input Max**之间。 所有大于或等于**Input Max**的输出梯度值都会设置为`1`。 | Float: 0.0 - 1.0 | `1.0` |
| **Output Min** | 设置输出梯度的最小值。重映射输入梯度，使其位于**Output Min**和**Output Max**之间。 | Float: 0.0 - 1.0 | `0.0` |
| **Output Max** | 设置输出梯度的最大值。重映射输入梯度，使其位于**Output Min**和**Output Max**之间。 | Float: 0.0 - 1.0 | `1.0` |

## 梯度混合操作

下面的示例混合了这些梯度：

| 第1个 Gradient | 第2个 Gradient |
| - | - |
| ![Linear ramp gradient](/images/user-guide/components/reference/gradient-modifiers/linear-ramp-gradient.png) | ![Noise gradient](/images/user-guide/components/reference/gradient-modifiers/noise-gradient.png) |

{{< note >}}
渐变的**Opacity**属性在其**Operation**之后应用。
{{< /note >}}

| 混合运算 | 说明 | 混合后的 Gradient |
| :- | :- | :- |
| `Initialize` | 将混合渐变的值初始化为当前渐变层的值。与`Normal`操作不同的是，当**Opacity**小于 `1`时，当前渐变层不会与之前渐变层的结果混合。  | ![Example gradients mixed with the initialize operation](/images/user-guide/components/reference/gradient-modifiers/initialize-operation.png) |
| `Multiply` | 将当前梯度值与前几层的结果相乘。 | ![Example gradients mixed with the multiply operation](/images/user-guide/components/reference/gradient-modifiers/multiply-operation.png) |
| `Linear Dodge (Add)` | 将当前梯度值与前几层的结果相加。| ![Example gradients mixed with the linear dodge-add operation](/images/user-guide/components/reference/gradient-modifiers/linear-dodge-operation.png) |
| `Subtract` | 将当前梯度值与前几层的结果相减。 | ![Example gradients mixed with the subtract operation](/images/user-guide/components/reference/gradient-modifiers/subtract-operation.png) |
| `Darken (Min)`  | 从当前梯度和前几层的结果中选择最小值。 | ![Example gradients mixed with the darken-min operation](/images/user-guide/components/reference/gradient-modifiers/darken-operation.png) |
| `Lighten (Max)` | 从当前梯度和前几层的结果中选择最大值。 | ![Example gradients mixed with the lighten-max operation](/images/user-guide/components/reference/gradient-modifiers/lighten-operation.png) |
| `Average` | 求当前梯度值和前几层结果的平均值。 | ![Example gradients mixed with the average operation](/images/user-guide/components/reference/gradient-modifiers/average-operation.png) |
| `Normal` | 使用 **Opacity** 的值来平均当前渐变值和之前图层的结果。如果**Opacity**等于`1`，则选择当前梯度的值。 | ![Example gradients mixed with the normal operation](/images/user-guide/components/reference/gradient-modifiers/normal-operation.png) |
| `Overlay` | 增强混合渐变中高低值的对比度。 如果前几层的结果值小于`.5`，该操作将使混合渐变变暗。 如果数值大于 `.5`，混合渐变将变亮。  | ![Example gradients mixed with the overlay operation](/images/user-guide/components/reference/gradient-modifiers/overlay-operation.png) |

## MixedGradientRequestBus

使用以下带有 `MixedGradientRequestBus` EBus 接口的请求函数，与游戏中的渐变混合器组件进行通信。

| 方法名称 | 说明 | 参数 | 返回值 | 可脚本化 |
|-|-|-|-|-|
| `AddLayer` | 将图层添加到 **Layers** 数组的末尾。 | Mixed Gradient Layer | None | Yes |
| `GetLayer` | 返回指定索引处的混合梯度层。 | Layer Index: Integer | Mixed Gradient Layer | Yes |
| `GetNumLayers` | 返回混合梯度层的数量。| None | Count: Integer | Yes |
| `RemoveLayer` | 删除指定索引处的混合梯度图层。 | Layer Index: Integer | None | Yes |
