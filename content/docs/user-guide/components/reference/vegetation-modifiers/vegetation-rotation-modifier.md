---
title: Vegetation Rotation Modifier 组件
linktitle: Vegetation Rotation Modifier
description: 使用 Open 3D Engine （O3DE） 中的 Vegetation Rotation Modifier 组件向植被实例添加变化。
weight: 300
---

使用 **Vegetation Rotation Modifier** 组件向关卡中的植被实例添加变化。 使用渐变来控制植被或阻碍者实例在 X、Y 或 Z 轴上单独旋转的方式。默认情况下，此组件配置为在 Z 轴上向任一方向将植被实例旋转最多 180 度。

## 提供者

[Vegetation Gem](/docs/user-guide/gems/reference/environment/vegetation/)

## 依赖

使用 Vegetation Rotation Modifier 组件时，添加以下必需组件之一：
- [Vegetation Layer Blender](./../vegetation/vegetation-layer-blender)
- [Vegetation Layer Blocker](./../vegetation/vegetation-layer-blocker)
- [Vegetation Layer Blocker (Mesh)](./../vegetation/vegetation-layer-blocker-mesh)
- [Vegetation Layer Spawner](./../vegetation/layer-spawner)

## Vegetation Rotation Modifier 属性

![Vegetation Rotation Modifier component properties](/images/user-guide/components/reference/vegetation-modifiers/vegetation-rotation-modifier-component.png)

| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **Allow Per-Item Overrides** | 如果为 '`Enabled`'，则启用的植被描述符属性可以覆盖此组件的属性。 | Boolean | `Disabled` |
| **Rotation X - Range Min** | 设置 Vegetation 实例在 X 轴上的最小修改旋转。 | Float: -Infinity to Infinity | `0.0` |
| **Rotation X - Range Max** | 设置 Vegetation 实例在 X 轴上的最大修改旋转。 | Float: -Infinity to Infinity | `0.0` |
| **Rotation X - Gradient** | 请参阅下面的[Gradient 属性](#gradient-properties)。 |  |  |
| **Rotation Y - Range Min** | 设置 Vegetation 实例在 Y 轴上的最小修改旋转。 | Float: -Infinity to Infinity | `0.0` |
| **Rotation Y - Range Max** | 设置 Vegetation 实例在 Y 轴上的最大修改旋转。 | Float: -Infinity to Infinity | `0.0` |
| **Rotation Y - Gradient** | 请参阅下面的[Gradient 属性](#gradient-properties)。 |  |  |
| **Rotation Z - Range Min** | 设置 Vegetation 实例在 Z 轴上的最小修改旋转。 | Float: -Infinity to Infinity | `-180.0` |
| **Rotation Z - Range Max** | 设置 Z轴上植被实例的最大修改旋转。 | Float: -Infinity to Infinity | `180.0` |
| **Rotation Z - Gradient** | 请参阅下面的[Gradient 属性](#gradient-properties)。 |  |  |

### Gradient 属性

![Gradient properties](/images/user-guide/components/reference/vegetation-modifiers/gradient-properties.png)

| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **Gradient Entity Id** | 设置具有活动 **Gradient** 组件的实体。 | Entity | None |
| **Opacity** | 设置输入渐变的不透明度。 | Float: 0.0 - 1.0 | `1.0` |
| **Invert Input** | 反转输入渐变的值。 | Boolean | `Disabled` |
| **Preview (Inbound)** | 显示 **Gradient Entity Id** 中的实体集提供的渐变。 |  |  |
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

## RotationModifierRequestBus

将以下请求函数与 '`RotationModifierRequestBus`' 事件总线接口结合使用，以便与游戏中的 Vegetation Rotation Modifier 组件进行通信。

| 方法名称 | 说明 | 参数 | 返回值 | 可脚本化 |
|-|-|-|-|-|
| `GetAllowOverrides` | 返回 **Allow Per-Item Overrides** 属性的配置。 | None | Boolean | Yes |
| `GetGradientSamplerX` | 返回 **Rotation X** 组属性的渐变采样器对象。 | None | Gradient Sampler | Yes |
| `GetGradientSamplerY` | 返回 **Rotation Y** 组属性的渐变采样器对象。 | None | Gradient Sampler | Yes |
| `GetGradientSamplerZ` | 返回 **Rotation Z** 组属性的渐变采样器对象。 | None | Gradient Sampler | Yes |
| `GetRangeMax` | 返回 **Range Max** 属性的 Vector3。 | None | Vector3: (**Rotation X - Range Max**, **Rotation Y - Range Max**, **Rotation Z - Range Max**) | Yes |
| `GetRangeMin` | 返回 **Range Min** 属性的 Vector3。 | None | Vector3: (**Rotation X - Range Min**, **Rotation Y - Range Min**, **Rotation Z - Range Min**) | Yes |
| `SetAllowOverrides` | 设置 **Allow Per-Item Overrides** 属性的配置。 | Boolean | None | Yes |
| `SetRangeMax` | 设置 X、Y 和 Z **Range Max** 属性。 | Vector3: (**Rotation X - Range Max**, **Rotation Y - Range Max**, **Rotation Z - Range Max**) | None | Yes |
| `SetRangeMin` | 设置 X、Y 和 Z **Range Min** 属性。 | Vector3: (**Rotation X - Range Min**, **Rotation Y - Range Min**, **Rotation Z - Range Min**) | None | Yes |
