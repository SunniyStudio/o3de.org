---
linkTitle: Vegetation Asset List Combiner
title: Vegetation Asset List Combiner 组件
description: 将多个植被资产列表与 Open 3D Engine （O3DE） 中的 Vegetation Asset List Combiner 组件组合在一起。
weight: 200
---

使用 **Vegetation Asset List Combiner** 组件对多个植被资产列表进行分组。 此组件允许您使植被资产列表保持较小、集中且可重用;仅合并植被图层所需的资产列表。

## 提供者

[Vegetation Gem](/docs/user-guide/gems/reference/environment/vegetation/)

## Vegetation Asset List Combiner 属性

![Vegetation Asset List Combiner component properties](/images/user-guide/components/reference/vegetation/vegetation-asset-list-combiner-component.png)

| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **Descriptor Providers** | 将由此组件组合的植被资产列表数组。 | Array: Vegetation Asset List | None |

## DescriptorListCombinerRequestBus

将以下请求函数与 '`DescriptorListCombinerRequestBus`' 事件总线接口结合使用，以便与游戏中的 Vegetation Asset List Combiner 组件进行通信。

| 方法名称 | 说明 | 参数 | 返回值 | 可脚本化 |
|-|-|-|-|-|
| `AddDescriptorEntityId` | 将具有 **Vegetation Asset List** 组件的实体添加到 **Descriptor Providers** 数组中。 | EntityId | None | Yes |
| `GetDescriptorEntityId` | 返回指定索引处的 **Descriptor Provider** 的 EntityId。 | Descriptor Providers Index: Integer | EntityId | Yes |
| `GetNumDescriptors` | 返回 **Descriptor Providers** 数组中的条目数。 | None | Count: Integer | Yes |
| `RemoveDescriptorEntityId` | 从 **Descriptor Providers** 数组中删除实体。 | EntityId | None | Yes |
| `SetDescriptorEntityId` | 更新 **Descriptor Providers** 数组中条目的 EntityId。 | Descriptor Providers Index: Integer, EntityId | None | Yes |
