---
title: Vegetation Distribution Filter 组件
linktitle: Vegetation Distribution Filter
description: 在 Open 3D Engine （O3DE） 关卡中使用 Vegetation Distribution Filter 组件的梯度控制植被实例的分布。
weight: 300
---

添加 **Vegetation Distribution Filter** 组件以使用梯度来控制植被或阻碍者实例在植被区域中的生成位置。

## 提供者

[Vegetation Gem](/docs/user-guide/gems/reference/environment/vegetation/)

## 依赖

使用 Vegetation Distribution Filter 组件时，添加以下必需组件之一：
- [Vegetation Layer Blender](./../vegetation/vegetation-layer-blender)
- [Vegetation Layer Blocker](./../vegetation/vegetation-layer-blocker)
- [Vegetation Layer Blocker (Mesh)](./../vegetation/vegetation-layer-blocker-mesh)
- [Vegetation Layer Spawner](./../vegetation/layer-spawner)

## Vegetation Distribution Filter 属性

![Vegetation Distribution Filter component properties](/images/user-guide/components/reference/vegetation-filters/vegetation-distribution-filter-component.png)

| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **Preview** | 显示应用了 **Threshold** 属性的输出渐变。 | | |
| **Filter Stage** | 定义是在修饰符之前还是之后应用滤镜。 | `PreProcess`, `PostProcess`, or `Default` | `Default` |
| **Threshold Min** | 设置植被实例放置的最小梯度值。 | Float 0.0 - 1.0 | `0.1` |
| **Threshold Max** | 设置植被实例放置的最大梯度值。 | Float 0.0 - 1.0 | `1.0` |
| **Gradient** | 请参阅下面的 [Gradient 属性](#gradient-properties)。 |  |  |

### Gradient 属性

![Gradient properties](/images/user-guide/components/reference/vegetation-modifiers/gradient-properties.png)

| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **Gradient Entity Id** | 设置具有活动 **Gradient** 组件的实体。 | Entity | None |
| **Opacity** | 设置输入渐变的不透明度。 | Float: 0.0 - 1.0 | `1.0` |
| **Invert Input** | 反转输入渐变的值。 | Boolean | `Disabled` |
| **Preview (Inbound)** | 显示 **Gradient Entity Id** 中指定的实体提供的输入渐变。 |  |  |
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

## DistributionFilterRequestBus

将以下请求函数与 '`DistributionFilterRequestBus`' 事件总线接口结合使用，以便与游戏中的 Vegetation Distribution Filter 组件进行通信。

| 方法名称 | 说明 | 参数 | 返回值 | 可脚本化 |
|-|-|-|-|-|
| `GetGradientSampler` | 返回分布滤波器的 gradient sampler 对象。 | None | Gradient Sampler | Yes |
| `GetThresholdMax` | 返回 **Threshold Max** 属性的值。 | None | Float | Yes |
| `GetThresholdMin` | 返回 **Threshold Min** 属性的值。 | None | Float | Yes |
| `SetThresholdMax` | 设置 **Threshold Max** 属性的值。 | Float | None | Yes |
| `SetThresholdMin` | 设置 **Threshold Min** 属性的值。 | Float | None | Yes |
