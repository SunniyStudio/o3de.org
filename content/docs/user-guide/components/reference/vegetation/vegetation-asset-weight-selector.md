---
linkTitle: Vegetation Asset Weight Selector
title: Vegetation Asset Weight Selector 组件
description: 使用 Open 3D Engine （O3DE） 中的 Vegetation Asset Weight Selector 组件，使用渐变创建基于权重的资产选择。
weight: 250
---

**Vegetation Asset Weight Selector** 组件使用梯度的值从植被资产列表中选择资产。 在选择之前，可以按权重对资产进行排序，并且可以修改输入梯度以影响植被资产的数量和分布。

## 提供者

[Vegetation Gem](/docs/user-guide/gems/reference/environment/vegetation/)

## Vegetation Asset Weight Selector 属性

![Vegetation Asset Weight Selector component properties](/images/user-guide/components/reference/vegetation/vegetation-asset-weight-selector-component.png)

| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **Sort By Weight** | 确定在使用渐变选择资产之前是否按权重对资产描述符进行排序。 | `Unsorted`, `Ascending`, 或 `Descending` | `Unsorted` |
| **Gradient** | 请参阅下面的 [Gradient properties](#gradient-properties)。 |  |  |

### Gradient 属性

![Gradient properties](/images/user-guide/components/reference/vegetation-modifiers/gradient-properties.png)
2
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

## DescriptorWeightSelectorRequestBus

将以下请求函数与 '`DescriptorWeightSelectorRequestBus`' 事件总线接口结合使用，以便与游戏中的植被资产权重选择器组件进行通信。

| 方法名称 | 说明 | 参数 | 返回值 | 可脚本化 |
|-|-|-|-|-|
| `GetGradientSampler` | 返回权重选择器的 gradient sampler 对象。 | None | Gradient Sampler | Yes |
| `GetSortBehavior` | 设置 **Sort By Weight** 属性的配置。返回 '`0`' 表示 '`unsorted`'，返回 '`1`' 表示 '`Ascending`'，返回 '`2`' 表示 '`descending`'。 | None | Sort Behavior: Integer | Yes |
| `SetSortBehavior` | 设置 **Sort By Weight** 属性的配置。“`0`”代表“`Unsorted`”，“`1`”代表“`Ascending`”，“`2`”代表“`Descending`”。 | Sort Behavior: Integer | None | Yes |
