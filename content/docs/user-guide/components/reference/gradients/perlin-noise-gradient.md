---
linktitle: Perlin Noise Gradient
title: Perlin Noise Gradient 组件
description: 在Open 3D Engine (O3DE)中使用Perlin Noise Gradient组件，通过珀林噪声算法生成梯度。
---

添加 **Perlin Noise Gradient** 组件使用珀林噪声算法生成梯度。

## 提供方

[Gradient Signal Gem](/docs/user-guide/gems/reference/utility/gradient-signal)

## 依赖 ##

[Gradient Transform Modifier](/docs/user-guide/components/reference/gradient-modifiers/gradient-transform-modifier)

## Perlin Noise Gradient 属性

![Perlin Noise Gradient properties](/images/user-guide/components/reference/gradients/perlin-noise-gradient-component.png)

| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **Generate Random Seed** | 将下面的**Random Seed**属性设置为随机值。 | | |
| **Preview** | 显示该组件应用所有属性后的输出渐变效果。 | | |
| **Pin Preview to Shape** | 设置一个具有兼容形状组件的实体，以便在**Constrain to Shape**为`Enabled`时用作预览的边界。 | EntityId | Current Entity |
| **Preview Position** | 设置预览的世界位置。<br> <br>只有在**Pin Preview to Shape**中没有选择实体时，此字段才可用。 | Vector3: -Infinity to Infinity | X:`0.0`, Y:`0.0`, Z:`0.0` |
| **Preview Size** | 设置预览的尺寸。 | Vector3: 0.0 to Infinity | X:`1.0`, Y:`1.0`, Z:`1.0` |
| **Constrain to Shape** | 如果 `Enabled`，渐变预览将使用在**Pin Preview to Shape**中选择的实体的边界。<br> <br>此字段仅在**Pin Preview to Shape**中选择了实体时可用。 | Boolean | `Disabled` |
| **Random Seed** | 设置噪音生成算法的种子值。每个值都会产生不同的噪音模式。 | Integer: 1 to Infinity | `1` |
| **Octaves** | 设置模式生成的递归次数。 | Integer: 0 - 16 | `1` |
| **Amplitude** | 增加高梯度值和低梯度值之间的对比度。 | Float: 0.0 to Infinity | `1.0` |
| **Frequency** | 调整梯度坐标。数值越大，噪音越粗。  | Float: 0.0001 - Infinity | `1.0` |

## PerlinGradientRequestBus

使用以下带有 `PerlinGradientRequestBus` EBus 接口的请求函数与游戏中的 Perlin Noise Gradient 组件进行通信。

| 方法名称 | 说明 | 参数 | 返回值 | 脚本化 |
|-|-|-|-|-|
| `GetAmplitude` | 返回**Amplitude** 属性的值。 | None | Float | Yes |
| `GetFrequency` | 返回**Frequency** 属性的值。 | None | Float | Yes |
| `GetOctaves` | 返回**Octaves** 属性的值。 | None | Octave Count: Integer | Yes |
| `GetRandomSeed` | 返回**Random Seed** 属性的值。 | None | Seed: Integer | Yes |
| `SetAmplitude` | 设置**Amplitude** 属性的值。 | Float | None | Yes |
| `SetFrequency` | 设置**Frequency** 属性的值。 | Float | None | Yes |
| `SetOctaves` | 设置**Octaves** 属性的值。 | Octave Count: Integer | None | Yes |
| `SetRandomSeed` | 设置**Random Seed** 属性的值。 | Seed: Integer | None | Yes |
