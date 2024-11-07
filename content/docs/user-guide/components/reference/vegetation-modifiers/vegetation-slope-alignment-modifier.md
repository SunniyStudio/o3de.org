---
title: Vegetation Slope Alignment Modifier 组件
linktitle: Vegetation Slope Alignment Modifier
description: 使用 Slope Alignment Modifier 组件将植被实例的方向与 Open 3D Engine （O3DE） 中的底层地形对齐。
weight: 500
---

使用 **Slope Alignment Modifier** 组件将植被或障碍物实例的方向与底层地形对齐。 使用梯度来控制和改变植被对齐。默认情况下，此组件配置为始终将植被实例与 terrain 的坡度完全对齐。 通过将对齐系数设置为小于 '`1`' 的值，实例将仅与 terrain 部分对齐。

## 提供者

[Vegetation Gem](/docs/user-guide/gems/reference/environment/vegetation/)

## 依赖

使用 Vegetation Slope Alignment Modifier 组件时，添加以下必需组件之一：
- [Vegetation Layer Blender](./../vegetation/vegetation-layer-blender)
- [Vegetation Layer Blocker](./../vegetation/vegetation-layer-blocker)
- [Vegetation Layer Blocker (Mesh)](./../vegetation/vegetation-layer-blocker-mesh)
- [Vegetation Layer Spawner](./../vegetation/layer-spawner)

## Vegetation Slope Alignment Modifier 属性

![Vegetation Slope Alignment Modifier component properties](/images/user-guide/components/reference/vegetation-modifiers/vegetation-slope-alignment-modifier-component.png)

| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **Allow Per-Item Overrides** | 如果为 '`Enabled`'，则启用的植被描述符属性可以覆盖此组件的属性。 | Boolean | `Disabled` |
| **Alignment Coefficient Min** | 设置最小坡度对齐系数。 | Float: 0.0 - 1 | `1.0` |
| **Alignment Coefficient Max** | 设置最大坡度对齐系数。 | Float: 0.0 - 1 | `1.0` |
| **Gradient** | 请参阅下面的[Gradient 属性](#gradient-properties)。 |  |  |

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

## SlopeAlignmentModifierRequestBus

将以下请求函数与 '`SlopeAlignmentModifierRequestBus`' 事件总线接口结合使用，以便与游戏中的 Vegetation Slope Alignment Modifier 组件进行通信。

| 方法名称 | 说明 | 参数 | 返回值 | 可脚本化 |
|-|-|-|-|-|
| `GetAllowOverrides` | 返回 **Allow Per-Item Overrides** 属性的配置。 | None | Boolean | Yes |
| `GetGradientSampler` | 返回缩放修饰符的渐变采样器对象。| None | Gradient Sampler | Yes |
| `GetRangeMax` | 返回 **Alignment Coefficient Max** 属性的值。 | None | Float | Yes |
| `GetRangeMin` | 返回 **Alignment Coefficient Min** 属性的值。 | None | Float | Yes |
| `SetAllowOverrides` | 设置 **Allow Per-Item Overrides** 属性的配置。 | Boolean | None | Yes |
| `SetRangeMax` | 设置 **Alignment Coefficient Max** 属性。 | Float | None | Yes |
| `SetRangeMin` | 设置 **Alignment Coefficient Min** 属性。 | Float | None | Yes |
