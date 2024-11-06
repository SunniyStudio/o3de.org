---
linktitle: Shape Falloff Gradient
title: Shape Falloff Gradient 组件
description: 使用Shape Falloff Gradient组件在Open 3D Engine (O3DE)中生成带有衰减的形状渐变。
---

添加**Shape Falloff Gradient**组件，生成一个由可配置的落差包围的形状渐变。

## 提供者

[Gradient Signal Gem](/docs/user-guide/gems/reference/utility/gradient-signal)

## Shape Falloff Gradient 属性

![Shape Falloff Gradient component properties](/images/user-guide/components/reference/gradients/shape-falloff-gradient-component.png)

| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **Preview** | 显示该组件应用所有属性后的输出渐变效果。 | | |
| **Pin Preview to Shape** | 设置一个具有兼容形状组件的实体，以便在**Constrain to Shape**为`Enabled`时用作预览的边界。 | EntityId | Current Entity |
| **Preview Position** | 设置预览的世界位置。<br> <br>*只有在**Pin Preview to Shape**中没有选择实体时，此字段才可用。  | Vector3: -Infinity to Infinity | X:`0.0`, Y:`0.0`, Z:`0.0` |
| **Preview Size** | 设置预览的尺寸。 | Vector3: 0.0 to Infinity | X:`1.0`, Y:`1.0`, Z:`1.0` |
| **Constrain to Shape** | 如果`Enabled`，渐变预览将使用在**Pin Preview to Shape**中选择的实体的边界。<br> <br>此字段仅在**Pin Preview to Shape**中选择了实体时可用。 | Boolean | `Disabled` |
| **Shape Entity Id** | 设置该组件生成渐变的形状。 | EntityId | None |
| **Falloff Width** | 设置落差的最大距离（以米为单位）。 | Float: 0.0 - 100.0 | `1.0` |

## ShapeAreaFalloffGradientRequestBus

将以下请求函数与`ShapeAreaFalloffGradientRequestBus`EBus 接口结合使用，可与游戏中的Shape Falloff Gradient组件进行通信。

| 方法名称 | 说明 | 参数 | 返回值 | 脚本化 |
|-|-|-|-|-|
| `GetFalloffType` | 返回 **Falloff Type** 属性的值。 | None | Falloff Type Index: Integer | Yes |
| `GetFalloffWidth` | 返回 **Falloff Width** 属性的值。 | None | Float | Yes |
| `GetShapeEntityId` | 返回 **Pin Preview to Shape** 属性的值。 | None | EntityId | Yes |
| `SetFalloffType` | 设置**Falloff Type** 属性的值。 | Falloff Type Index: Integer | None | Yes |
| `SetFalloffWidth` | 设置**Falloff Width** 属性的值。 | Float | None | Yes |
| `SetShapeEntityId` | 设置**Pin Preview to Shape** 属性的值。 | EntityId | None | Yes |
