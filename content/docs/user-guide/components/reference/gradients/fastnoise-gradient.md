---
linktitle: FastNoise Gradient
title: FastNoise Gradient 组件
description: 使用FastNoise Gradient组件，利用Open 3D Engine (O3DE)的 FastNoise 噪点生成库生成梯度。
---

添加**FastNoise Gradient**组件，使用[FastNoise](https://github.com/Auburn/FastNoiseLite) 库中的一种噪声生成算法来生成梯度。 噪声生成算法在组件的**Noise Type**属性中设置。

## 提供方

[Fast Noise Gem](/docs/user-guide/gems/reference/utility/fast-noise)

## 依赖 ##

- [Gradient Transform Modifier](/docs/user-guide/components/reference/gradient-modifiers/gradient-transform-modifier)
- 以下 [Shape](./../shape/) 组件之一: [Axis Aligned Box](./../shape/axis-aligned-box-shape), [Box](./../shape/box-shape), [Capsule](./../shape/capsule-shape), [Compound](./../shape/compound-shape), [Cylinder](./../shape/cylinder-shape), [Disk](./../shape/disk-shape), [Polygon Prism](./../shape/polygon-prism-shape), [Quad](./../shape/quad-shape), [Shape Reference](./../shape/shape-reference), [Sphere](./../shape/sphere-shape), or [Tube](./../shape/tube-shape),  定义 **Gradient Transform Modifier** 的区域。

## Noise 类型

| Noise 类型 | 说明 | 灰阶示例 |
| - | - | - |
| `Value` | 根据 XYZ 坐标的内插值生成`White Noise`梯度。 | ![FastNoise value noise type example](/images/user-guide/components/reference/gradients/fastnoise-value.png) |
| `Value Fractal` | `Value`算法的结果通过分形函数运行。 | ![FastNoise value fractal noise type example](/images/user-guide/components/reference/gradients/fastnoise-value-fractal.png) |
| `Perlin` | 从 Perlin 噪声算法中生成数值，这是一种视觉特征大小相似的噪声算法。 | ![FastNoise Perlin noise type example](/images/user-guide/components/reference/gradients/fastnoise-perlin.png) |
| `Perlin Fractal` | 通过分形函数运行`Perlin`噪声算法的结果。 | ![FastNoise Perlin fractal noise type example](/images/user-guide/components/reference/gradients/fastnoise-perlin-fractal.png) |
| `Simplex` | 根据 Simplex 噪声算法生成数值，这是 Perlin 噪声的一种变体，具有较少的方向伪差。 | ![FastNoise simplex noise type example](/images/user-guide/components/reference/gradients/fastnoise-simplex.png) |
| `Simplex Fractal` | 通过分形函数运行`Simplex`噪声算法的结果。 | ![FastNoise simplex fractal noise type example](/images/user-guide/components/reference/gradients/fastnoise-simplex-fractal.png) |
| `Cellular` | 通过蜂窝噪声算法生成数值，该算法根据随机分布的特征点分配数值；每个世界位置都会分配最接近特征点的数值。 | ![FastNoise cellular noise type example](/images/user-guide/components/reference/gradients/fastnoise-cellular.png) |
| `White Noise` |	根据 XYZ 坐标生成数值，相邻样本的数值差异极大。 | ![FastNoise white noise noise type example](/images/user-guide/components/reference/gradients/fastnoise-white-noise.png) |
| `Cubic` | 直接从 XYZ 坐标生成数值，然后与邻近数值进行三次插值。其结果类似于`Perlin`噪声，但方向伪影较少，极值出现率较高。 | ![FastNoise cubic noise type example](/images/user-guide/components/reference/gradients/fastnoise-cubic.png) |
| `Cubic Fractal` | 通过分形函数运行`Cubic`噪声算法的结果。 | ![FastNoise cubic fractal noise type example](/images/user-guide/components/reference/gradients/fastnoise-cubic-fractal.png) |

{{< note >}}
并非所有**Noise Type**和噪音属性设置的组合都具有相同的性能特征。
{{< /note >}}

## FastNoise Gradient 属性

{{< tabs name="fastnoise-gradient-component-ui" >}}
{{% tab name="Value" %}}

![FastNoise value gradient properties](/images/user-guide/components/reference/gradients/fastnoise-gradient-component-value.png)

| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **Preview** | 显示该组件应用所有属性后的输出渐变效果。点击预览图片顶部的预览器图标，可在停靠窗口中显示更大的渐变预览。 | | |
| **Pin Preview to Shape** | 设置一个具有兼容形状组件的实体，以便在**Constrain to Shape**为`Enabled`时用作预览的边界。 | EntityId | Current Entity |
| **Preview Position** | 设置预览的世界位置。<br> <br>只有在**Pin Preview to Shape**中没有选择实体时，此字段才可用。 | Vector3: -Infinity to Infinity | X:`0.0`, Y:`0.0`, Z:`0.0` |
| **Preview Size** | 设置预览的尺寸。 | Vector3: 0.0 to Infinity | X:`1.0`, Y:`1.0`, Z:`1.0` |
| **Constrain to Shape** | 如果`Enabled`，渐变预览将使用在**Pin Preview to Shape**中选择的实体的边界。<br> <br>此字段仅在**Pin Preview to Shape**中选择了实体时可用。 | Boolean | `Disabled` |
| **Noise Type** | 设置用于生成梯度的噪声生成算法。 | `Value`, `Value Fractal`, `Perlin`, `Perlin Fractal`, `Simplex`, `Simplex Fractal`, `Cellular`, `White Noise`, `Cubic`, 或 `Cubic Fractal` | `Perlin Fractal` |
| **Random Seed** | 设置伪随机噪音生成算法的初始化值。每个值都会产生不同的噪声模式。 | Integer: 1 to Infinity | `1` |
| **Frequency** | 设置产生噪音的频率。数值越小，产生的噪音越大；数值越大，产生的噪音越小。  | Float: 0.0001 - Infinity | `1.0` |
| **FastNoise Advanced Settings - Interpolation** | 设置用于平滑梯度值之间的函数。有关 [插值类型说明和示例](#value-interpolation-type-examples) 请参阅以下部分。 | `Linear`, `Hermite`, or `Quintic` | `Quintic` |
| **Generate Random Seed** | 设置 **Random Seed** 属性为随机。 | | |

### Value Interpolation 类型示例

| 插值类型 | 示例 | 灰阶示例 |
|-|-|-|
| `Linear` | `Linear` 插值会产生角度假象。 | ![Linear interpolation example gradient](/images/user-guide/components/reference/gradients/interpolation-linear.png) |
| `Hermite` | `Hermite` 插值产生平滑的模糊值。 | ![Hermite interpolation example gradient](/images/user-guide/components/reference/gradients/interpolation-hermite.png) |
| `Quintic` | `Quintic`插值法比`Hermite`插值法产生更清晰的边缘，而不会产生 `Linear` 插值法的角度误差。 | ![Quintic interpolation example gradient](/images/user-guide/components/reference/gradients/interpolation-quintic.png) |

{{% /tab %}}
{{% tab name="Value Fractal" %}}

![FastNoise value fractal gradient properties](/images/user-guide/components/reference/gradients/fastnoise-gradient-component-value-fractal.png)

| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **Preview** | 显示该组件应用所有属性后的输出渐变效果。点击预览图片顶部的预览器图标，可在停靠窗口中显示更大的渐变预览。 | | |
| **Pin Preview to Shape** | 设置一个具有兼容形状组件的实体，以便在**Constrain to Shape**为 `Enabled` 时用作预览的边界。 | EntityId | Current Entity |
| **Preview Position** | 设置预览的世界位置。<br> <br>只有在**Pin Preview to Shape**中没有选择实体时，此字段才可用。 | Vector3: -Infinity to Infinity | X:`0.0`, Y:`0.0`, Z:`0.0` |
| **Preview Size** | 设置预览的尺寸。 | Vector3: 0.0 to Infinity | X:`1.0`, Y:`1.0`, Z:`1.0` |
| **Constrain to Shape** | 如果`Enabled`，渐变预览将使用在**Pin Preview to Shape**中选择的实体的边界。<br> <br>此字段仅在**Pin Preview to Shape**中选择了实体时可用。 | Boolean | `Disabled` |
| **Noise Type** | 设置用于生成梯度的噪声生成算法。 | `Value`, `Value Fractal`, `Perlin`, `Perlin Fractal`, `Simplex`, `Simplex Fractal`, `Cellular`, `White Noise`, `Cubic`, 或 `Cubic Fractal` | `Perlin Fractal` |
| **Random Seed** | 设置伪随机噪音生成算法的初始化值。每个值都会产生不同的噪声模式。 | Integer: 1 to Infinity | `1` |
| **Frequency** | 设置产生噪音的频率。数值越小，产生的噪音越大；数值越大，产生的噪音越小。 | Float: 0.0001 - Infinity | `1.0` |
| **Octaves** | 设置模式生成的递归次数。数值越大，细节越精细。大于`4`的值可能无法察觉。 | Integer: 0 - 8 | `4` |
| **Lacunarity** | 设置应用于**Octaves**倍频程的频率乘数。 | Float 0.0 to Infinity | `2.0` |
| **Gain** | 设置应用于 **Octaves**的相对强度乘数。 | Float: 0.0 to Infinity | `0.5` |
| **FastNoise Advanced Settings - Interpolation** | 设置用于平滑梯度值之间的函数。有关 [插值类型说明和示例](#value-fractal-interpolation-type-examples) 请参阅以下部分。 | `Linear`, `Hermite`, 或 `Quintic` | `Quintic` |
| **FastNoise Advanced Settings - Fractal Type** | 设置分形组合的方法。有关 [分形类型说明和示例](#value-fractal-type-examples) 请参阅以下部分。| `FBM`, `Billow`, 或  `Rigid Multi` | `FBM` |
| **Generate Random Seed** | 设置 **Random Seed** 属性为随机值。 | | |

### Value Fractal Interpolation 类型示例

| Interpolation Type | 说明 | 灰阶示例 |
|-|-|-|
| `Linear` | `Linear` 插值会产生角度假象。 | ![Linear interpolation example gradient](/images/user-guide/components/reference/gradients/interpolation-linear.png) |
| `Hermite` | `Hermite` 插值产生平滑的模糊值。 | ![Hermite interpolation example gradient](/images/user-guide/components/reference/gradients/interpolation-hermite.png) |
| `Quintic` | `Quintic`插值法比 `Hermite` 插值法产生更清晰的边缘，而不会产生`Linear`插值法的角度误差。 | ![Quintic interpolation example gradient](/images/user-guide/components/reference/gradients/interpolation-quintic.png) |

### Value Fractal Type 示例

| Fractal Type | 说明 | 灰阶示例 |
|-|-|-|
| `FBM` | `FBM` 或 分数布朗运动 将噪声信号的多个频率和振幅加在一起. | ![FBM fractal type example gradient](/images/user-guide/components/reference/gradients/fractal-type-fbm.png) |
| `Billow` | `FBM`的一种变体。`Billow`将噪声信号的多个频率和振幅的绝对值相加。这会产生梯度值的极低值。 | ![Billow fractal type example gradient](/images/user-guide/components/reference/gradients/fractal-type-billow.png) |
| `Rigid Multi` | `FBM`的变体。`Rigid Multi` 是将噪声信号的多个频率和振幅的绝对值的倒数相减。这会产生梯度值的极高值。 | ![Rigid Multi fractal type example gradient](/images/user-guide/components/reference/gradients/fractal-type-rigid-multi.png) |
{{% /tab %}}
{{% tab name="Perlin" %}}

![FastNoise perlin gradient properties](/images/user-guide/components/reference/gradients/fastnoise-gradient-component-perlin.png)

| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **Preview** | 显示该组件应用所有属性后的输出渐变效果。点击预览图片顶部的预览器图标，可在停靠窗口中显示更大的渐变预览。 | | |
| **Pin Preview to Shape** | 设置一个具有兼容Shape组件的实体，以便在**Constrain to Shape**为`Enabled`时用作预览的边界。 | EntityId | Current Entity |
| **Preview Position** | 设置预览的世界位置。<br> <br>只有在**Pin Preview to Shape**中没有选择实体时，此字段才可用。 | Vector3: -Infinity to Infinity | X:`0.0`, Y:`0.0`, Z:`0.0` |
| **Preview Size** | 设置预览的尺寸。 | Vector3: 0.0 to Infinity | X:`1.0`, Y:`1.0`, Z:`1.0` |
| **Constrain to Shape** | 如果 `Enabled`，渐变预览将使用在**Pin Preview to Shape**中选择的实体的边界。<br> <br>只有在**Pin Preview to Shape**中选择了实体，该字段才可用。 | Boolean | `Disabled` |
| **Noise Type** | 设置用于生成梯度的噪声生成算法。 | `Value`, `Value Fractal`, `Perlin`, `Perlin Fractal`, `Simplex`, `Simplex Fractal`, `Cellular`, `White Noise`, `Cubic`, 或 `Cubic Fractal` | `Perlin Fractal` |
| **Random Seed** | 设置伪随机噪音生成算法的初始化值。每个值都会产生不同的噪声模式。 | Integer: 1 to Infinity | `1` |
| **Frequency** | 设置产生噪音的频率。数值越小，产生的噪音越大；数值越大，产生的噪音越小。  | Float: 0.0001 - Infinity | `1.0` |
| **FastNoise Advanced Settings - Interpolation** | 设置用于平滑梯度值之间的函数。有关 [插值类型说明和示例](#perlin-interpolation-type-examples) 请参阅以下部分。 | `Linear`, `Hermite`, 或 `Quintic` | `Quintic` |
| **Generate Random Seed** | 设置 **Random Seed** 属性为随机值。 | | |

### Perlin Interpolation类型示例

| Interpolation Type | 说明 | 灰阶示例 |
|-|-|-|
| `Linear` | `Linear` 插值会产生角度假象。 | ![Linear interpolation example gradient](/images/user-guide/components/reference/gradients/interpolation-linear.png) |
| `Hermite` | `Hermite` 插值产生平滑的模糊值。| ![Hermite interpolation example gradient](/images/user-guide/components/reference/gradients/interpolation-hermite.png) |
| `Quintic` | `Quintic`“插值法比`Hermite`插值法产生更清晰的边缘，而不会产生`Linear`插值法的角度误差。 | ![Quintic interpolation example gradient](/images/user-guide/components/reference/gradients/interpolation-quintic.png) |

{{% /tab %}}
{{% tab name="Perlin Fractal" %}}

![FastNoise perlin fractal gradient properties](/images/user-guide/components/reference/gradients/fastnoise-gradient-component-perlin-fractal.png)

| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **Preview** | 显示该组件应用所有属性后的输出渐变效果。点击预览图片顶部的预览器图标，可在停靠窗口中显示更大的渐变预览。 | | |
| **Pin Preview to Shape** | 设置一个具有兼容形状组件的实体，以便在**Constrain to Shape**为`Enabled`时用作预览的边界。 | EntityId | Current Entity |
| **Preview Position** | 设置预览的世界位置。<br> <br>只有在**Pin Preview to Shape**中未选择实体时，该字段才可用。 | Vector3: -Infinity to Infinity | X:`0.0`, Y:`0.0`, Z:`0.0` |
| **Preview Size** | 设置预览的尺寸。 | Vector3: 0.0 to Infinity | X:`1.0`, Y:`1.0`, Z:`1.0` |
| **Constrain to Shape** | 如果`Enabled`，渐变预览将使用在**Pin Preview to Shape**中选择的实体的边界。<br> <br>此字段仅在**Pin Preview to Shape**中选择了实体时可用。 | Boolean | `Disabled` |
| **Noise Type** | 设置用于生成梯度的噪声生成算法。 | `Value`, `Value Fractal`, `Perlin`, `Perlin Fractal`, `Simplex`, `Simplex Fractal`, `Cellular`, `White Noise`, `Cubic`, 或 `Cubic Fractal` | `Perlin Fractal` |
| **Random Seed** | 设置伪随机噪音生成算法的初始化值。每个值都会产生不同的噪声模式。 | Integer: 1 to Infinity | `1` |
| **Frequency** | 设置产生噪音的频率。数值越小，产生的噪音越大；数值越大，产生的噪音越小。 | Float: 0.0001 - Infinity | `1.0` |
| **Octaves** | 设置模式生成的递归次数。数值越大，细节越精细。大于 `4` 的值可能无法察觉。 | Integer: 0 - 8 | `4` |
| **Lacunarity** | 设置应用于连续**Octaves**的频率乘数。 | Float 0.0 to Infinity | `2.0` |
| **Gain** | 设置应用于连续**Octaves**的相对强度乘数。 | Float: 0.0 to Infinity | `0.5` |
| **FastNoise Advanced Settings - Interpolation** | 设置用于平滑梯度值之间的函数。有关 [插值类型说明和示例](#perlin-fractal-interpolation-type-examples)请参阅以下部分。 | `Linear`, `Hermite`, 或 `Quintic` | `Quintic` |
| **FastNoise Advanced Settings - Fractal Type** | 设置分形组合的方法。有关 [分形类型说明和示例](#perlin-fractal-type-examples) 请参阅以下部分。 | `FBM`, `Billow`, or  `Rigid Multi` | `FBM` |
| **Generate Random Seed** | 设置 **Random Seed** 属性为随机值。 | | |

### Perlin Fractal Interpolation 类型示例

| Interpolation Type | 说明 | 灰阶示例 |
|-|-|-|
| `Linear` | `Linear` 插值会产生角度假象。 | ![Linear interpolation example gradient](/images/user-guide/components/reference/gradients/interpolation-linear.png) |
| `Hermite` | `Hermite` 插值产生平滑的模糊值。 | ![Hermite interpolation example gradient](/images/user-guide/components/reference/gradients/interpolation-hermite.png) |
| `Quintic` | `Quintic`插值法比`Hermite`插值法产生更清晰的边缘，而不会产生`Linear`插值法的角度误差。 | ![Quintic interpolation example gradient](/images/user-guide/components/reference/gradients/interpolation-quintic.png) |

### Perlin Fractal Type 示例

| Fractal Type | 说明 | 灰阶示例 |
|-|-|-|
| `FBM` | `FBM` 或**分数布朗运动**将噪声信号的多个频率和振幅加在一起。 | ![FBM fractal type example gradient](/images/user-guide/components/reference/gradients/fractal-type-fbm.png) |
| `Billow` | `FBM`的一种变体。`Billow`"将噪声信号的多个频率和振幅的绝对值相加。这会产生梯度值的极低值。 | ![Billow fractal type example gradient](/images/user-guide/components/reference/gradients/fractal-type-billow.png) |
| `Rigid Multi` | `FBM` 的变体。`Rigid Multi`是将噪声信号的多个频率和振幅的绝对值的倒数相减。这会产生梯度值的极高值。 | ![Rigid Multi fractal type example gradient](/images/user-guide/components/reference/gradients/fractal-type-rigid-multi.png) |

{{% /tab %}}
{{% tab name="Simplex" %}}

![FastNoise simplex gradient properties](/images/user-guide/components/reference/gradients/fastnoise-gradient-component-simplex.png)

| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **Preview** | 显示该组件应用所有属性后的输出渐变效果。点击预览图片顶部的预览器图标，可在停靠窗口中显示更大的渐变预览。 | | |
| **Pin Preview to Shape** | 设置一个具有兼容形状组件的实体，以便在**Constrain to Shape**为 `Enabled` 时用作预览的边界。 | EntityId | Current Entity |
| **Preview Position** | 设置预览的世界位置。<br> <br>只有在**Pin Preview to Shape**中未选择实体时，该字段才可用。 | Vector3: -Infinity to Infinity | X:`0.0`, Y:`0.0`, Z:`0.0` |
| **Preview Size** | 设置预览的尺寸。 | Vector3: 0.0 to Infinity | X:`1.0`, Y:`1.0`, Z:`1.0` |
| **Constrain to Shape** | 如果`Enabled`，渐变预览将使用在**Pin Preview to Shape**中选择的实体的边界。 <br> <br>此字段仅在**Pin Preview to Shape**中选择了实体时可用。 | Boolean | `Disabled` |
| **Noise Type** | 设置用于生成梯度的噪声生成算法。 | `Value`, `Value Fractal`, `Perlin`, `Perlin Fractal`, `Simplex`, `Simplex Fractal`, `Cellular`, `White Noise`, `Cubic`, 或 `Cubic Fractal` | `Perlin Fractal` |
| **Random Seed** | 设置伪随机噪音生成算法的初始化值。每个值都会产生不同的噪声模式。 | Integer: 1 to Infinity | `1` |
| **Frequency** | 设置产生噪音的频率。数值越小，产生的噪音越大；数值越大，产生的噪音越小。  | Float: 0.0001 - Infinity | `1.0` |
| **Generate Random Seed** | 设置**Random Seed** 属性为随机值。 | | |

{{% /tab %}}
{{% tab name="Simplex Fractal" %}}

![FastNoise simplex fractal gradient properties](/images/user-guide/components/reference/gradients/fastnoise-gradient-component-simplex-fractal.png)

| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **Preview** | 显示该组件应用所有属性后的输出渐变效果。点击预览图片顶部的预览器图标，可在停靠窗口中显示更大的渐变预览。 | | |
| **Pin Preview to Shape** | 设置一个具有兼容形状组件的实体，以便在**Constrain to Shape**为`Enabled`时用作预览的边界。 | EntityId | Current Entity |
| **Preview Position** | 设置预览的世界位置。<br> <br>只有在**Pin Preview to Shape**中没有选择实体时，此字段才可用。 | Vector3: -Infinity to Infinity | X:`0.0`, Y:`0.0`, Z:`0.0` |
| **Preview Size** | 设置预览的尺寸。 | Vector3: 0.0 to Infinity | X:`1.0`, Y:`1.0`, Z:`1.0` |
| **Constrain to Shape** | 如果`Enabled`，渐变预览将使用在**Pin Preview to Shape**中选择的实体的边界。 <br> <br>只有在**Pin Preview to Shape**中选择了实体，该字段才可用。| Boolean | `Disabled` |
| **Noise Type** | 设置用于生成梯度的噪声生成算法。 | `Value`, `Value Fractal`, `Perlin`, `Perlin Fractal`, `Simplex`, `Simplex Fractal`, `Cellular`, `White Noise`, `Cubic`, 或 `Cubic Fractal` | `Perlin Fractal` |
| **Random Seed** | 设置伪随机噪音生成算法的初始化值。每个值都会产生不同的噪声模式。 | Integer: 1 to Infinity | `1` |
| **Frequency** | 设置产生噪音的频率。数值越小，产生的噪音越大；数值越大，产生的噪音越小。  | Float: 0.0001 - Infinity | `1.0` |
| **Octaves** | 设置模式生成的递归次数。数值越大，细节越精细。大于 `4` 的值可能无法察觉。 | Integer: 0 - 8 | `4` |
| **Lacunarity** | 设置应用于连续 **Octaves** 的频率乘数。 | Float 0.0 to Infinity | `2.0` |
| **Gain** | 设置应用于连续**Octaves** 的相对强度乘数。 | Float: 0.0 to Infinity | `0.5` |
| **FastNoise Advanced Settings - Fractal Type** | 设置分形组合的方法。有关 [分形类型说明和示例](#simplex-fractal-type-examples) 请参阅以下部分。 | `FBM`, `Billow`, or  `Rigid Multi` | `FBM` |
| **Generate Random Seed** | 设置 **Random Seed** 属性为随机值。 | | |

### Simplex Fractal Type 示例

| Fractal Type | 说明 | 灰阶示例 |
|-|-|-|
| `FBM` | `FBM`或 分数布朗运动 将噪声信号的多个频率和振幅加在一起。 | ![FBM fractal type example gradient](/images/user-guide/components/reference/gradients/fractal-type-fbm.png) |
| `Billow` | `FBM`的一种变体。`Billow`将噪声信号的多个频率和振幅的绝对值相加。这会产生梯度值的极低值。 | ![Billow fractal type example gradient](/images/user-guide/components/reference/gradients/fractal-type-billow.png) |
| `Rigid Multi` | `FBM`的变体。`Rigid Multi`是将噪声信号的多个频率和振幅的绝对值的倒数相减。这会产生梯度值的极高值。 | ![Rigid Multi fractal type example gradient](/images/user-guide/components/reference/gradients/fractal-type-rigid-multi.png) |

{{% /tab %}}
{{% tab name="Cellular" %}}

![FastNoise cellular gradient properties](/images/user-guide/components/reference/gradients/fastnoise-gradient-component-cellular.png)

| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **Preview** | 显示该组件应用所有属性后的输出渐变效果。点击预览图片顶部的预览器图标，可在停靠窗口中显示更大的渐变预览。 | | |
| **Pin Preview to Shape** | 设置一个具有兼容形状组件的实体，以便在**Constrain to Shape**为 `Enabled`时用作预览的边界。 | EntityId | Current Entity |
| **Preview Position** | 设置预览的世界位置。<br> <br>只有在**Pin Preview to Shape**中没有选择实体时，此字段才可用。 | Vector3: -Infinity to Infinity | X:`0.0`, Y:`0.0`, Z:`0.0` |
| **Preview Size** | 设置预览的尺寸。 | Vector3: 0.0 to Infinity | X:`1.0`, Y:`1.0`, Z:`1.0` |
| **Constrain to Shape** | 如果`Enabled`，渐变预览将使用在**Pin Preview to Shape**中选择的实体的边界。<br> <br>此字段仅在**Pin Preview to Shape**中选择了实体时可用。 | Boolean | `Disabled` |
| **Noise Type** | 设置用于生成梯度的噪声生成算法。 | `Value`, `Value Fractal`, `Perlin`, `Perlin Fractal`, `Simplex`, `Simplex Fractal`, `Cellular`, `White Noise`, `Cubic`, 或 `Cubic Fractal` | `Perlin Fractal` |
| **Random Seed** | 设置伪随机噪音生成算法的初始化值。每个值都会产生不同的噪声模式。 | Integer: 1 to Infinity | `1` |
| **Frequency** | 设置产生噪音的频率。数值越小，产生的噪音越大；数值越大，产生的噪音越小。  | Float: 0.0001 - Infinity | `1.0` |
| **Distance Function** | 设置用于计算指定点的单元格值的距离函数。距离函数会产生不同的单元格形状。有关 [距离函数示例和说明](#distance-function-examples) 请参阅以下部分。 | `Euclidean`, `Manhattan`, 或 `Natural` | `Euclidean` |
| **Return Type** | 设置单元函数返回值的类型。有关 [返回类型示例](#return-type-examples) 请参阅以下部分。 | `CellValue`, `Distance`, `Distance2`, `Distance2Add`, `Distance2Sub`, `Distance2Mul`, or `Distance2Div`  | `CellValue` |
| **Jitter** | 设置单元点从原始位置移动的最大距离。大于 `1.0` 的值可能会产生箝位乘法的假象。 | Float: 0.0 to Infinity | `0.45` |
| **Generate Random Seed** | 设置 **Random Seed** 属性为随机值。 | | |

### Distance Function 示例

| Distance Function | 灰阶示例 |
|-|-|
| `Euclidean` | ![Euclidean distance function example gradient](/images/user-guide/components/reference/gradients/distance-function-euclidean.png) |
| `Manhattan` | ![Manhattan distance function example gradient](/images/user-guide/components/reference/gradients/distance-function-manhattan.png) |
| `Natural` | ![Natural distance function example gradient](/images/user-guide/components/reference/gradients/distance-function-natural.png) |

### Return Type 示例

| Return Type | 说明 | 灰阶示例 |
|-|-|-|
| `CellValue` | 返回任何给定世界位置上最近特征点的值。 | ![CellValue return type example gradient](/images/user-guide/components/reference/gradients/return-type-cellvalue.png) |
| `Distance` | 	返回任何给定世界位置上最近地物点的距离。 | ![Distance return type example gradient](/images/user-guide/components/reference/gradients/return-type-distance.png) |
| `Distance2` | 返回任何给定世界位置上距离第二最近特征点的距离。 | ![Distance2 return type example gradient](/images/user-guide/components/reference/gradients/return-type-distance2.png) |
| `Distance2Add` | 返回两个最近特征点相加的距离。 | ![Distance2Add return type example gradient](/images/user-guide/components/reference/gradients/return-type-distance2add.png) |
| `Distance2Sub` | 返回两个最近的特征点相减后的距离。| ![Distance2Sub return type example gradient](/images/user-guide/components/reference/gradients/return-type-distance2sub.png) |
| `Distance2Mul` | 返回两个最近特征点相乘的距离。 | ![Distance2Mul return type example gradient](/images/user-guide/components/reference/gradients/return-type-distance2mul.png) |
| `Distance2Div` | 返回最近特征点的距离除以第二近特征点的距离。 | ![Distance2Div return type example gradient](/images/user-guide/components/reference/gradients/return-type-distance2div.png) |

{{% /tab %}}
{{% tab name="White Noise" %}}

![FastNoise white noise gradient properties](/images/user-guide/components/reference/gradients/fastnoise-gradient-component-white-noise.png)

| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **Preview** | 显示该组件应用所有属性后的输出渐变效果。点击预览图片顶部的预览器图标，可在停靠窗口中显示更大的渐变预览。 | | |
| **Pin Preview to Shape** | 设置一个具有兼容形状组件的实体，以便在**Constrain to Shape**为`Enabled`时用作预览的边界。 | EntityId | Current Entity |
| **Preview Position** | 设置预览的世界位置。<br> <br>只有在**Pin Preview to Shape**中没有选择实体时，此字段才可用。 | Vector3: -Infinity to Infinity | X:`0.0`, Y:`0.0`, Z:`0.0` |
| **Preview Size** | 设置预览的尺寸。 | Vector3: 0.0 to Infinity | X:`1.0`, Y:`1.0`, Z:`1.0` |
| **Constrain to Shape** | 如果 `Enabled`，渐变预览将使用在**Pin Preview to Shape**中选择的实体的边界。<br> <br>只有在**Pin Preview to Shape**中选择了实体，该字段才可用。 | Boolean | `Disabled` |
| **Noise Type** | 设置用于生成梯度的噪声生成算法。 | `Value`, `Value Fractal`, `Perlin`, `Perlin Fractal`, `Simplex`, `Simplex Fractal`, `Cellular`, `White Noise`, `Cubic`, 或 `Cubic Fractal` | `Perlin Fractal` |
| **Random Seed** | 设置伪随机噪音生成算法的初始化值。每个值都会产生不同的噪声模式。 | Integer: 1 to Infinity | `1` |
| **Generate Random Seed** | 设置 **Random Seed** 属性为随机值。 | | |

{{% /tab %}}
{{% tab name="Cubic" %}}

![FastNoise cubic gradient properties](/images/user-guide/components/reference/gradients/fastnoise-gradient-component-cubic.png)

| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **Preview** | 显示该组件应用所有属性后的输出渐变效果。点击预览图片顶部的预览器图标，可在停靠窗口中显示更大的渐变预览。 | | |
| **Pin Preview to Shape** | 设置一个具有兼容形状组件的实体，以便在**Constrain to Shape**为`Enabled`时用作预览的边界。 | EntityId | Current Entity |
| **Preview Position** | 设置预览的世界位置。<br> <br>只有在**Pin Preview to Shape**中未选择实体时，该字段才可用。 | Vector3: -Infinity to Infinity | X:`0.0`, Y:`0.0`, Z:`0.0` |
| **Preview Size** | 设置预览的尺寸。 | Vector3: 0.0 to Infinity | X:`1.0`, Y:`1.0`, Z:`1.0` |
| **Constrain to Shape** | 如果`Enabled`，渐变预览将使用在**Pin Preview to Shape**中选择的实体的边界。 <br> <br>此字段仅在**Pin Preview to Shape**中选择了实体时可用。 | Boolean | `Disabled` |
| **Noise Type** | 设置用于生成梯度的噪声生成算法。 | `Value`, `Value Fractal`, `Perlin`, `Perlin Fractal`, `Simplex`, `Simplex Fractal`, `Cellular`, `White Noise`, `Cubic`, 或 `Cubic Fractal` | `Perlin Fractal` |
| **Random Seed** | 设置伪随机噪音生成算法的初始化值。每个值都会产生不同的噪声模式。 | Integer: 1 to Infinity | `1` |
| **Frequency** | 设置产生噪音的频率。数值越小，产生的噪音越大；数值越大，产生的噪音越小。  | Float: 0.0001 - Infinity | `1.0` |
| **Generate Random Seed** | 设置 **Random Seed** 属性为随机值。 | | |

{{% /tab %}}
{{% tab name="Cubic Fractal" %}}

![FastNoise cubic fractal gradient properties](/images/user-guide/components/reference/gradients/fastnoise-gradient-component-cubic-fractal.png)

| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **Preview** | 显示该组件应用所有属性后的输出渐变效果。点击预览图片顶部的预览器图标，可在停靠窗口中显示更大的渐变预览。 | | |
| **Pin Preview to Shape** | 设置一个具有兼容形状组件的实体，以便在**Constrain to Shape**为`Enabled`时用作预览的边界。 | EntityId | Current Entity |
| **Preview Position** | 设置预览的世界位置。<br> <br>只有在**Pin Preview to Shape**中没有选择实体时，此字段才可用。 | Vector3: -Infinity to Infinity | X:`0.0`, Y:`0.0`, Z:`0.0` |
| **Preview Size** | 设置预览的尺寸。 | Vector3: 0.0 to Infinity | X:`1.0`, Y:`1.0`, Z:`1.0` |
| **Constrain to Shape** | 如果`Enabled`，渐变预览将使用在**Pin Preview to Shape**中选择的实体的边界。<br> <br>只有在**Pin Preview to Shape**中选择了实体，该字段才可用。 | Boolean | `Disabled` |
| **Noise Type** | Sets the noise generation algorithm used to generate the gradient. | `Value`, `Value Fractal`, `Perlin`, `Perlin Fractal`, `Simplex`, `Simplex Fractal`, `Cellular`, `White Noise`, `Cubic`, or `Cubic Fractal` | `Perlin Fractal` |
| **Random Seed** | 设置伪随机噪音生成算法的初始化值。每个值都会产生不同的噪声模式。 | Integer: 1 to Infinity | `1` |
| **Frequency** | 设置产生噪音的频率。数值越小，产生的噪音越大；数值越大，产生的噪音越小。  | Float: 0.0001 - Infinity | `1.0` |
| **Octaves** | 设置模式生成的递归次数。数值越大，细节越精细。大于 `4` 的值可能无法察觉。 | Integer: 0 - 8 | `4` |
| **Lacunarity** | 设置应用于连续**Octaves**的频率乘数。 | Float 0.0 to Infinity | `2.0` |
| **Gain** | 设置应用于连续**Octaves**的相对强度乘数。 | Float: 0.0 to Infinity | `0.5` |
| **FastNoise Advanced Settings - Fractal Type** | 设置分形组合的方法。有关 [分形类型说明和示例](#cubic-fractal-type-examples) 请参阅以下部分。| `FBM`, `Billow`, or  `Rigid Multi` | `FBM` |
| **Generate Random Seed** | Sets the **Random Seed**属性为随机值。 | | |

### Cubic Fractal Type 示例

| Fractal Type | 说明 | 灰阶示例 |
|-|-|-|
| `FBM` | `FBM` 或 分数布朗运动 将噪声信号的多个频率和振幅加在一起。 | ![FBM fractal type example gradient](/images/user-guide/components/reference/gradients/fractal-type-fbm.png) |
| `Billow` | `FBM` 的一种变体。`Billow` 将噪声信号的多个频率和振幅的绝对值相加。这会产生梯度值的极低值。 | ![Billow fractal type example gradient](/images/user-guide/components/reference/gradients/fractal-type-billow.png) |
| `Rigid Multi` | `FBM` 的变体。`Rigid Multi` 是将噪声信号的多个频率和振幅的绝对值的倒数相减。这会产生梯度值的极高值。 | ![Rigid Multi fractal type example gradient](/images/user-guide/components/reference/gradients/fractal-type-rigid-multi.png) |

{{% /tab %}}
{{< /tabs >}}

## FastNoiseGradientRequestBus

使用下列请求函数和`FastNoiseGradientRequestBus` EBus 接口与游戏中的 FastNoise Gradient 组件通信。

| 方法名称 | 说明 | 参数 | 返回值 | 脚本化 |
|-|-|-|-|-|
| `GetFractalType` | 返回 **FastNoise Advanced Settings - Fractal Type** 属性的值。 | None | Fractal Type Index: Integer | Yes |
| `GetFrequency` | 返回 **Frequency** 属性的值。| None | Float | Yes |
| `GetGain` | 返回 **Gain** property. | None | Float | Yes |
| `GetInterpolation` | 返回 **FastNoise Advanced Settings - Interpolation** 属性的值。 | None | Interpolation Index: Integer | Yes |
| `GetLacunarity` | 返回 **Lacunarity** 属性的值。 | None | Float | Yes |
| `GetNoiseType` | 返回 **Noise Type** 属性的值。 | None | Noise Type Index: Integer | Yes |
| `GetOctaves` | 返回 **Octaves** 属性的值。 | None | Octave Count: Integer | Yes |
| `GetRandomSeed` | 返回 **Random Seed** 属性的值。 | None | Seed: Integer | Yes |
| `SetFractalType` | 设置 **FastNoise Advanced Settings - Fractal Type** 属性的值。| Fractal Type Index: Integer | None | Yes |
| `SetFrequency` | 设置 **Frequency** 属性的值。 | Float | None | Yes |
| `SetGain` | 设置 **Gain** 属性的值。 | Float | None | Yes |
| `SetInterpolation` | 设置 **FastNoise Advanced Settings - Interpolation** 属性的值。 | Interpolation Index: Integer | None | Yes |
| `SetLacunarity` | 设置 **Lacunarity** 属性的值。 | Float | None | Yes |
| `SetNoiseType` | 设置 **Noise Type** 属性的值。 | Noise Type Index: Integer | None | Yes |
| `SetOctaves` | 设置 **Octaves** 属性的值。 | Octave Count: Integer | None | Yes |
| `SetRandomSeed` | 设置 **Random Seed** 属性的值。 | Seed: Integer | None | Yes |
