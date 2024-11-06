---
linkTitle: Vegetation Asset List
title: Vegetation Asset List 组件
description: 在 Open 3D Engine （O3DE） 中使用自定义权重和设置将植被资产组织到可重用的资产列表中。
weight: 150
---

使用 **Vegetation Asset List** 组件将植被资产合并到可重用的资产列表中。 配置 **Vegetation Asset Descriptors**，这是特定于资产的设置，仅在从此组件的植被层生成资产时使用。 在 **资产编辑器** 中创建 Vegetation Descriptor List，或在组件界面中定义它们。

## 提供方

[Vegetation Gem](/docs/user-guide/gems/reference/environment/vegetation/)

## Vegetation Asset List 属性

{{< tabs name="source-type" >}}
{{% tab name="Embedded Source" %}}

![Vegetation Asset List component properties](/images/user-guide/components/reference/vegetation/vegetation-asset-list-component-embedded.png)

| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **Source Type** | 如果设置为 '`Embedded`'，则在此组件的界面中定义植被资产描述符。 如果设置为 '`External`'，则在 Vegetation Descriptor List 资产中定义描述符。 | `Embedded` or `External` | `Embedded` |
| **Embedded Assets** | 植被资产描述符的数组。 |  |  |

### Asset 属性

| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **Instance Spawner** | 设置要生成的资产类型。 | `Dynamic Slice`, `Empty Space`, or `Prefab` | `Dynamic Slice` |
| **Asset** | 选择要生成的源资源。<br> <br>仅当 **Instance Spawner** 设置为 '`Dynamic Slice`' 或 '`Prefab`' 时，此字段才可用。 | Dynamic Slice 或 Prefab Asset | None |
| **Weight** | 设置资产实例的相对密度。 资产列表中权重较大的植被资产将比权重较小的资产更频繁地放置。 | Float: -Infinity to Infinity | `1.0` |
| **Display Per-Item Overrides** | 显示 Vegetation Modifiers 和 Filters 设置的逐资产覆盖。 | Boolean | `Disabled` |

{{% /tab %}}
{{% tab name="External Source" %}}

![Vegetation Asset List component properties](/images/user-guide/components/reference/vegetation/vegetation-asset-list-component-external.png)

| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **Source Type** | 如果设置为 '`Embedded`'，则在此组件的界面中定义植被资产描述符。 如果设置为 '`External`'，则在 Vegetation Descriptor List 资产中定义描述符。 | `Embedded` or `External` | `Embedded` |
| **External Assets** | 选择为此组件提供描述符的 Vegetation Descriptor List 资产。 | Vegetation Descriptor List Asset | None |

{{% /tab %}}
{{< /tabs >}}

## DescriptorListRequestBus

将以下请求函数与 '`DescriptorListRequestBus`' 事件总线接口结合使用，以便与游戏中的 Vegetation Asset List 组件进行通信。

| 方法名称 | 说明 | 参数 | 返回值 | 可脚本化 |
|-|-|-|-|-|
| `AddDescriptor` | 将描述符添加到资产列表。 | Vegetation Descriptor | None | Yes |
| `GetDescriptor` | 从资产列表中返回植被描述符。 | Descriptor Index: Integer | Vegetation Descriptor | Yes |
| `GetDescriptorAssetPath` | 返回资产列表的 **External Assets** 源路径。 | None | Path: String | Yes |
| `GetDescriptorListSource` | 返回资产列表的 **Source Type**。对于 '`Embedded`' 返回 '`0`'，对于 '`External`' 源类型返回 '`1`'。| None | List Source Type: Integer | Yes |
| `GetNumDescriptors` | 返回资产列表中的植被描述符的数量。 | None | Descriptor Index: Integer | Yes |
| `RemoveDescriptor` | 从资产列表中删除植被描述符。| Descriptor Index: Integer | None | Yes |
| `SetDescriptor` | 在特定资产列表索引处设置描述符的配置。 | Descriptor Index: Integer, Vegetation Descriptor | None | Yes |
| `SetDescriptorAssetPath` | 设置资产列表的 **External Assets** 源路径。 | Path: String | None | Yes |
| `SetDescriptorListSource` | 设置资产列表的 **Source Type**。 `0` 用于 `Embedded` ， `1` 用于 `External`源类型。 | List Source Type: Integer | None | Yes |
