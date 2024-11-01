---
linktitle: Constant Gradient
title: Constant Gradient 组件
description: 使用Constant Gradient组件在Open 3D Engine (O3DE)生成具有相同值的梯度。
---

添加**Constant Gradient**组件，以创建具有相同值的输出渐变。

## 提供方

[Gradient Signal Gem](/docs/user-guide/gems/reference/utility/gradient-signal)

## Constant Gradient 属性

![Constant Gradient component properties](/images/user-guide/components/reference/gradients/constant-gradient-component.png)

| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **Preview** | 显示该组件应用所有属性后的输出渐变效果。 | | |
| **Pin Preview to Shape** | 设置一个具有兼容形状组件的实体，以便在**Constrain to Shape**为`Enabled`时用作预览的边界。 | EntityId | Current Entity |
| **Preview Position** | 设置预览的世界位置。<br> <br>只有在**Pin Preview to Shape**中没有选择实体时，此字段才可用。 | Vector3: -Infinity to Infinity | X:`0.0`, Y:`0.0`, Z:`0.0` |
| **Preview Size** | 设置预览的尺寸。 | Vector3: 0.0 to Infinity | X:`1.0`, Y:`1.0`, Z:`1.0` |
| **Constrain to Shape** | 如果 `Enabled`，渐变预览将使用在**Pin Preview to Shape**中选择的实体的边界。<br> <br>只有在**Pin Preview to Shape**中选择了实体，该字段才可用。 | Boolean | `Disabled` |
| **Value** | 设置该梯度返回的单一值。 | Float: 0.0 - 1.0 | `1.0` |

## ConstantGradientRequestBus

使用以下带有 `ConstantGradientRequestBus` EBus 接口的请求函数，可与游戏中的恒定渐变组件进行通信。

| 方法名称 | 说明 | 参数 | 返回值 | 脚本化 |
|-|-|-|-|-|
| `GetConstantValue` | 返回 **Value** 属性的值。 | None | Constant Value: Float | Yes |
| `SetConstantValue` | 设置 **Value** 属性的值。 | Constant Value: Float | None | Yes |
