---
linktitle: Surface Mask Gradient
title: Surface Mask Gradient 组件
description: 在Open 3D Engine (O3DE)中使用Surface Mask Gradient组件从曲面掩模权重生成渐变。
---

添加**Surface Mask Gradient**组件，从曲面标记列表中生成归一化渐变。 相应的曲面掩膜权重用于生成输出梯度。

## 提供者

[Gradient Signal Gem](/docs/user-guide/gems/reference/utility/gradient-signal)

## Surface Mask Gradient 属性

![Surface Mask Gradient component properties](/images/user-guide/components/reference/gradients/surface-mask-gradient-component.png)

| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **Preview** | 显示该组件应用所有属性后的输出渐变效果。 | | |
| **Pin Preview to Shape** | 设置一个具有兼容形状组件的实体，以便在**Constrain to Shape**为`Enabled`时用作预览的边界。 | EntityId | Current Entity |
| **Preview Position** | 设置预览的世界位置。<br> <br>只有在**Pin Preview to Shape**中没有选择实体时，此字段才可用。 | Vector3: -Infinity to Infinity | X:`0.0`, Y:`0.0`, Z:`0.0` |
| **Preview Size** | 设置预览的尺寸。 | Vector3: 0.0 to Infinity | X:`1.0`, Y:`1.0`, Z:`1.0` |
| **Constrain to Shape** | 如果`Enabled`，渐变预览将使用在**Pin Preview to Shape**中选择的实体的边界。<br> <br>此字段仅在**Pin Preview to Shape**中选择了实体时可用。 | Boolean | `Disabled` |
| **Surface Tag List** | 用于生成梯度的 [表面标签](/docs/user-guide/gems/reference/environment/surface-data) 数组。 | Array: Surface Tags | None |

## SurfaceMaskGradientRequestBus

将以下请求函数与`SurfaceMaskGradientRequestBus` EBus 接口结合使用，可与游戏中的Surface Mask Gradient组件进行通信。

| 方法名称 | 说明 | 参数 | 返回值 | 脚本化 |
|-|-|-|-|-|
| `AddTag` | 将曲面标记添加到**Surface Tag List**数组。 | Surface Tag: String | None | Yes |
| `GetNumTags` | 返回**Surface Tag List**数组中标签的数量。 | None | Count: Integer | Yes |
| `GetTag` | 返回**Surface Tag List**数组中指定索引处的表面标签。  | Surface Tag Index: Integer | Surface Tag: String | Yes |
| `RemoveTag` | 删除**Surface Tag List**数组中指定索引处的表面标记。 | Surface Tag Index: Integer | None | Yes |
