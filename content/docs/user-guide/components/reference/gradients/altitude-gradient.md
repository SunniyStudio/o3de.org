---
linktitle: Altitude Gradient
title: Altitude Gradient 组件
description: 使用Altitude Gradient组件在Open 3D Engine (O3DE)中从海拔高度范围生成梯度。
---

添加**Altitude Gradient**组件，从高度范围生成归一化梯度。 输出梯度可选择受地表标签限制。

## 提供方

[Gradient Signal Gem](/docs/user-guide/gems/reference/utility/gradient-signal)

## Altitude Gradient 属性

![Altitude Gradient component properties](/images/user-guide/components/reference/gradients/altitude-gradient-component.png)

| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **Preview** | 显示该组件应用所有属性后的输出渐变效果。| | |
| **Pin Preview to Shape** | 设置一个具有兼容形状组件的实体，以便在**Constrain to Shape**为`Enabled`时用作预览的边界。 | EntityId | Current Entity |
| **Preview Position** | 设置预览的世界位置。<br> <br>只有在**Pin Preview to Shape**中未选择实体时，该字段才可用。 | Vector3: -Infinity to Infinity | X:`0.0`, Y:`0.0`, Z:`0.0` |
| **Preview Size** | 设置预览的尺寸。 | Vector3: 0.0 to Infinity | X:`1.0`, Y:`1.0`, Z:`1.0` |
| **Constrain to Shape** | 如果`Enabled`，渐变预览将使用在**Pin Preview to Shape**中选择的实体的边界。<br> <br>只有在**Pin Preview to Shape**中选择了实体，该字段才可用。 | Boolean | `Disabled` |
| **Altitude Min** | 设置生成梯度值的最小高度。 | Float: -Infinity to Infinity | `0.0` |
| **Altitude Max** | 设置产生梯度值的最大高度。 | Float: -Infinity to Infinity | `128.0` |
| **Surface Tags to track** | [曲面标签](/docs/user-guide/gems/reference/environment/surface-data)的可选数组。 该组件只在存在这些地表标签的地方生成梯度。 | Array: Surface Tags | None |

## SurfaceAltitudeGradientRequestBus

将以下请求函数与 `SurfaceAltitudeGradientRequestBus` EBus 接口结合使用，可与游戏中的高度梯度组件进行通信。

| 方法名称 | 说明 | 参数 | 返回值 | 脚本化 |
|-|-|-|-|-|
| `AddTag` | 在**Surface Tags to track**数组中添加一个曲面标记。 | Surface Tag: String | None | Yes |
| `GetAltitudeMax` | 返回 **Altitude Max** 属性的值。 | None | Altitude: Float | Yes |
| `GetAltitudeMin` | 返回 **Altitude Min** 属性的值。 | None | Altitude: Float | Yes |
| `GetNumTags` | 返回要跟踪的**Surface Tags to track**数组中标签的数量。 | None | Count: Integer | Yes |
| `GetShapeEntityId` | 返回 **Pin Preview to Shape** 属性的值。  | None | EntityId | Yes |
| `GetTag` | 返回**Surface Tags to track**数组中指定索引处的表面标签。 | Surface Tag Index: Integer | Surface Tag: String | Yes |
| `RemoveTag` | 删除**Surface Tags to track**数组中指定索引处的表面标签。 | Surface Tag Index: Integer | None | Yes |
| `SetAltitudeMax` | 设置 **Altitude Max** 属性的值。 | Altitude: Float | None | Yes |
| `SetAltitudeMin` | 设置 **Altitude Min** 属性的值。 | Altitude: Float | None | Yes |
| `SetShapeEntityId` | 设置 **Pin Preview to Shape** 属性的值。 | EntityId | None | Yes |
