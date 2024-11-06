---
linktitle: Random Noise Gradient
title: Random Noise Gradient 组件
description: 使用Random Noise Gradient组件，在Open 3D Engine (O3DE)中通过随机噪音算法生成梯度。
---

添加**Random Noise Gradient** 组件使用随机噪点生产灰阶。

## 提供方

[Gradient Signal Gem](/docs/user-guide/gems/reference/utility/gradient-signal)

## 依赖 ##

[Gradient Transform Modifier](/docs/user-guide/components/reference/gradient-modifiers/gradient-transform-modifier)

## Random Noise Gradient 属性

![Random Noise Gradient properties](/images/user-guide/components/reference/gradients/random-noise-gradient-component.png)

| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **Generate Random Seed** | 将下面的**Random Seed**属性设置为随机值。 | | |
| **Preview** | 显示该组件应用所有属性后的输出渐变效果。 | | |
| **Pin Preview to Shape** | 设置一个具有兼容形状组件的实体，以便在**Constrain to Shape**为`Enabled`时用作预览的边界。 | EntityId | Current Entity |
| **Preview Position** | 设置预览的世界位置。<br> <br>只有在**Pin Preview to Shape**中没有选择实体时，此字段才可用。 | Vector3: -Infinity to Infinity | X:`0.0`, Y:`0.0`, Z:`0.0` |
| **Preview Size** | 设置预览的尺寸。 | Vector3: 0.0 to Infinity | X:`1.0`, Y:`1.0`, Z:`1.0` |
| **Constrain to Shape** | 如果`Enabled`，渐变预览将使用在**Pin Preview to Shape**中选择的实体的边界。<br> <br>此字段仅在**Pin Preview to Shape**中选择了实体时可用。 | Boolean | `Disabled` |
| **Random Seed** | 设置噪音生成算法的种子值。每个值都会产生不同的噪音模式。 | Integer: 1 to Infinity | `1` |

## RandomGradientRequestBus

使用以下带有 `RandomGradientRequestBus` EBus 接口的请求函数与游戏中的随机噪音渐变组件进行通信。

| 方法名称 | 说明 | 参数 | 返回值 | 脚本化 |
|-|-|-|-|-|
| `GetRandomSeed` | 返回 **Random Seed** 属性的值。 | None | Seed: Integer | Yes |
| `SetRandomSeed` | 设置**Random Seed**属性的值。 | Seed: Integer | None | Yes |
