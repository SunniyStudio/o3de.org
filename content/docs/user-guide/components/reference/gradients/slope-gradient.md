---
linktitle: Slope Gradient
title: Slope Gradient 组件
description: 在Open 3D Engine (O3DE)中使用Slope Gradient组件从一系列曲面坡度中生成梯度。
---

添加**Slope Gradient**组件，从坡度范围生成归一化梯度。 输出的梯度可选择受曲面标记的限制。

## 提供者

[Gradient Signal Gem](/docs/user-guide/gems/reference/utility/gradient-signal)

## Slope Gradient 属性

![Slope Gradient component properties](/images/user-guide/components/reference/gradients/slope-gradient-component.png)

| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **Preview** | 显示该组件应用所有属性后的输出渐变效果。 | | |
| **Pin Preview to Shape** | 如果**Constrain to Shape**为 `Enabled`，则设置一个具有兼容形状组件的实体作为预览的边界。 | EntityId | Current Entity |
| **Preview Position** | 设置预览的世界位置。<br> <br>只有在**Pin Preview to Shape**中未选择实体时，该字段才可用。 | Vector3: -Infinity to Infinity | X:`0.0`, Y:`0.0`, Z:`0.0` |
| **Preview Size** | 设置预览的尺寸。 | Vector3: 0.0 to Infinity | X:`1.0`, Y:`1.0`, Z:`1.0` |
| **Constrain to Shape** | 如果`Enabled`，渐变预览将使用在**Pin Preview to Shape**中选择的实体的边界。 <br> <br>此字段仅在**Pin Preview to Shape**中选择了实体时可用。 | Boolean | `Disabled` |
| **Surface Tags to track** | [surface tags](/docs/user-guide/gems/reference/environment/surface-data)的可选数组。 该组件只在存在这些表面标记的地方生成梯度。 | Array: Surface Tags | None |
| **Slope Min** | 设置产生梯度值的最小表面坡度角。 | Float: 0.0 - 90.0 | `0.0` |
| **Slope Max** | 设置产生梯度值的最大表面坡度角。 | Float: 0.0 - 90.0 | `20.0` |
| **Ramp Type** | 设置斜率范围之间的内插函数。 | `Linear Ramp Down`, `Linear Ramp Up`, 或 `Smooth Step` | `Linear Ramp Down` |
| **Smooth Step Settings** | 请参阅下面的 [Smooth Step Settings 属性](#smooth-step-settings-properties) 。| 

### Smooth Step Settings 属性
Smooth Step Settings属性仅适用于**Ramp Type** `Smooth Step`。

| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **Falloff Midpoint** | 设置衰减值的中点。 | Float: 0.0 - 1.0 | `0.5` |
| **Falloff Range** | 设置衰减范围。| Float: 0.0 - 1.0 | `0.5` |
| **Falloff Softness** | 设置应用于衰减值的平滑量。 | Float: 0.0 - 1.0 | `0.25` |

## SurfaceSlopeGradientRequestBus

将以下请求函数与`SurfaceSlopeGradientRequestBus` EBus 接口结合使用，可与游戏中的斜率渐变组件进行通信。

| 方法名称 | 说明 | 参数 | 返回值 | 脚本化 |
|-|-|-|-|-|
| `AddTag` | 在**Surface Tags to track**数组中添加一个曲面标记。 | Surface Tag: String | None | Yes |
| `GetNumTags` | 返回**Surface Tags to track**数组中标签的数量。  | None | Count: Integer | Yes |
| `GetRampType` | 返回 **Ramp Type** 属性的值。| None | Ramp Type Index: Integer | Yes |
| `GetSlopeMax` | 返回 **Slope Max** 属性的值。| None | Float | Yes |
| `GetSlopeMin` | 返回 **Slope Min** 属性的值。| None | Float | Yes |
| `GetTag` | 返回**Surface Tags to track**数组中指定索引处的表面标签。  | Surface Tag Index: Integer | Surface Tag: String | Yes |
| `RemoveTag` | 删除**Surface Tags to track**数组中指定索引处的表面标签。 | Surface Tag Index: Integer | None | Yes |
| `SetRampType` | 设置 **Ramp Type** 属性的值。| Ramp Type Index: Integer | None | Yes |
| `SetSlopeMax` | 设置 **Slope Max** 属性的值。| Float | None | Yes |
| `SetSlopeMin` | 设置 **Slope Min** 属性的值。| Float | None | Yes |
