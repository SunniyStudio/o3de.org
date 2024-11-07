---
title: Vegetation Position Modifier 组件
linktitle: Vegetation Position Modifier
description: 使用 Open 3D Engine （O3DE） 中的 Vegetation Position Modifier 组件为植被实例的放置添加变化。
weight: 200
---

使用 **Vegetation Position Modifier** 组件为关卡中植被实例的放置添加变化。 使用渐变来控制植被或阻碍者实例在 X、Y 或 Z 轴上的单独偏移方式。 默认情况下，此组件配置为在正轴或负 X 轴和 Y 轴上将植被实例的位置最多偏移 0.3 米。

## 提供者

[Vegetation Gem](/docs/user-guide/gems/reference/environment/vegetation/)

## 依赖

使用 Vegetation Position Modifier （植被位置修饰符） 组件时，添加以下必需组件之一：
- [Vegetation Layer Blender](./../vegetation/vegetation-layer-blender)
- [Vegetation Layer Blocker](./../vegetation/vegetation-layer-blocker)
- [Vegetation Layer Blocker (Mesh)](./../vegetation/vegetation-layer-blocker-mesh)
- [Vegetation Layer Spawner](./../vegetation/layer-spawner)

## Vegetation Position Modifier 属性

![Vegetation Position Modifier component properties](/images/user-guide/components/reference/vegetation-modifiers/vegetation-position-modifier-component.png)

| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **Allow Per-Item Overrides** | 如果为 '`Enabled`'，则启用的植被描述符属性可以覆盖此组件的属性。 | Boolean | `Disabled` |
| **Auto Snap to Surface** | 如果为 '`Enabled`'，则自动将修改后的植被实例位置对齐到最近的有效表面标签。 有效的表面标签包括在 **Surface Tags To Snap To** 属性中选择的标签，以及已与植被实例关联的任何表面标签。 | Boolean | `Enabled` |
| **Surface Tags To Snap To** | 用于将植被实例与表面对齐的[surface tags](/docs/user-guide/gems/reference/environment/surface-data)数组。 | Array: Surface Tags | None |
| **Position X - Range Min** | 设置 Vegetation 实例在 X 轴上的最小修改位置偏移。 | Float: -Infinity to Infinity | `-0.3` |
| **Position X - Range Max** | 设置 Vegetation 实例在 X 轴上的最大修改位置偏移。 | Float: -Infinity to Infinity | `0.3` |
| **Position X - Gradient** | 请参阅下面的[Gradient 属性](#gradient-properties)。 |  |  |
| **Position Y - Range Min** | 设置 Vegetation 实例在 Y 轴上的最小修改位置偏移。 | Float: -Infinity to Infinity | `-0.3` |
| **Position Y - Range Max** | 设置 Vegetation 实例在 Y 轴上的最大修改位置偏移。 | Float: -Infinity to Infinity | `0.3` |
| **Position Y - Gradient** | 请参阅下面的[Gradient 属性](#gradient-properties)。 |  |  |
| **Position Z - Range Min** | 设置 Z 轴上植被实例的最小修改位置偏移。 | Float: -Infinity to Infinity | `0.0` |
| **Position Z - Range Max** | 设置 Vegetation 实例在 Z 轴上的最大修改位置偏移。 | Float: -Infinity to Infinity | `0.0` |
| **Position Z - Gradient** | 请参阅下面的[Gradient 属性](#gradient-properties)。|  |  |

### Gradient 属性

![Gradient properties](/images/user-guide/components/reference/vegetation-modifiers/gradient-properties.png)

| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **Gradient Entity Id** | 设置具有活动 **Gradient** 组件的实体。 | Entity | None |
| **Opacity** | 设置输入渐变的不透明度。 | Float: 0.0 - 1.0 | `1.0` |
| **Invert Input** | 反转输入渐变的值。 | Boolean | `Disabled` |
| **Preview (Inbound)** | 显示 **Gradient Entity Id** 中的实体集提供的输入梯度。 |  |  |
| **Enable Transform** | 如果为 '`Enabled`'，则可以修改输入渐变的平移、缩放和旋转。 | Boolean | `Disabled` |
| **Translate** | 设置输入渐变的平移。 | Vector3: -Infinity to Infinity | X:`0.0`, Y:`0.0`, Z:`0.0` |
| **Scale** | 设置输入渐变的比例。 | Vector3: 0.0 to Infinity | X:`1.0`, Y:`1.0`, Z:`1.0` |
| **Rotate** | 设置输入渐变的旋转。 | Vector3: -Infinity to Infinity | X:`0.0`, Y:`0.0`, Z:`0.0` |
| **Enable Levels** | 如果为 '`Enabled`'，则可以修改渐变的输入和输出值。 | Boolean | `Disabled` |
| **Input Mid** | 设置输入渐变的中值。 | Float: 0.0 - 1.0 | `1.0` |
| **Input Min** | 设置输入渐变的最小值。 | Float: 0.0 - 1.0 | `0.0` |
| **Input Max** | 设置输入渐变的最大值。 | Float: 0.0 - 1.0 | `1.0` |
| **Output Min** | 设置输出渐变的最小值。 | Float: 0.0 - 1.0 | `0.0` |
| **Output Max** | 设置输出渐变的最大值。 | Float: 0.0 - 1.0 | `1.0` |

## PositionModifierRequestBus

将以下请求函数与 '`PositionModifierRequestBus`' 事件总线接口结合使用，以便与游戏中的 Vegetation Position Modifier 组件进行通信。

| 方法名称 | 说明 | 参数 | 返回值 | 可脚本化 |
|-|-|-|-|-|
| `AddTag` | 将曲面标记添加到 **Surface Tags To Snap To** 数组中。 | Surface Tag: String | None | Yes |
| `GetAllowOverrides` | 返回 **Allow Per-Item Overrides** 属性的配置。| None | Boolean | Yes |
| `GetGradientSamplerX` | 返回 **Position X** 组属性的渐变采样器对象。 | None | Gradient Sampler | Yes |
| `GetGradientSamplerY` | 返回 **Position Y** 组属性的渐变采样器对象。 | None | Gradient Sampler | Yes |
| `GetGradientSamplerZ` | 返回 **Position Z** 组属性的渐变采样器对象。 | None | Gradient Sampler | Yes |
| `GetNumTags` | 返回 **Surface Tags To Snap To**数组中的表面标记数。 | None | Count: Integer | Yes |
| `GetRangeMax` | 返回 **Range Max** 属性的 Vector3。 | None | Vector3: (**Position X - Range Max**, **Position Y - Range Max**, **Position Z - Range Max**) | Yes |
| `GetRangeMin` | 返回 **Range Min** 属性的 Vector3。 | None | Vector3: (**Position X - Range Min**, **Position Y - Range Min**, **Position Z - Range Min**) | Yes |
| `GetTag` | 返回 **Surface Tags To Snap To** 数组的指定索引处的 surface 标签。 | Surface Tag Index: Integer | Surface Tag: String | Yes |
| `RemoveTag` | 删除 **Surface Tags To Snap To** 数组的指定索引处的表面标签。 | Surface Tag Index: Integer | None | Yes |
| `SetAllowOverrides` | 设置 **Allow Per-Item Overrides** 属性的配置。 | Boolean | None | Yes |
| `SetRangeMax` | 设置 X、Y 和 Z **Range Max** 属性。 | Vector3: (**Position X - Range Max**, **Position Y - Range Max**, **Position Z - Range Max**) | None | Yes |
| `SetRangeMin` | 设置 X、Y 和 Z **Range Min**属性。 | Vector3: (**Position X - Range Min**, **Position Y - Range Min**, **Position Z - Range Min**) | None | Yes |
