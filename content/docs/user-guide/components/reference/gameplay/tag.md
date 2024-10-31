---
linkTitle: Tag
title: Tag 组件
description: 使用Tag组件可将标签应用到Open 3D Engine (O3DE)中的实体。
---

使用 **Tag** 组件可将一个或多个标签或 *tags* 应用于实体。使用这些标签可查找或过滤具有特定标签的实体。

## 提供方

[O3DE Core (LmbrCentral) Gem](/docs/user-guide/gems/reference/o3de-core)

## Tag 属性

![Tag component properties](/images/user-guide/components/reference/gameplay/tag-component.png)

| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **Tags** | 添加到实体中的标签数组。 | Array: Tag | None |

## TagHelper

| 请求名称 | 说明 | 参数 | 返回值 | 可脚本化 |
|-|-|-|-|-|
| `GetEntitiesbyTag` | 返回具有特定标记的实体数组。 | Tag: Crc32 | Array: EntityIds | Yes |

## TagComponentRequestBus

| 请求名称 | 说明 | 参数 | 返回值 | 可脚本化 |
|-|-|-|-|-|
| `AddTag` | 为实体添加一个标签。 | Tag: Crc32 | None | Yes |
| `HasTag` | 如果实体有特定标签，则返回 `True`。 | Tag: Crc32 | Boolean | Yes |
| `RemoveTag` | 从实体中删除特定标签。 | Tag: Crc32 | None | Yes |

## TagComponentNotificationsBus

| 请求名称 | 说明 | 参数 | 返回值 | 可脚本化 |
|-|-|-|-|-|
| `OnTagAdded` | 在特定实体添加任何标记时通知侦听器。 | Entity: EntityId | Tag: Crc32 | Yes |
| `OnTagRemoved` | 当从特定实体移除任何标记时通知侦听器。 | Entity: EntityId | Tag: Crc32 | Yes |

## TagGlobalRequestBus

| 请求名称 | 说明 | 参数 | 返回值 | 可脚本化 |
|-|-|-|-|-|
| `RequestTaggedEntities` | 返回第一个响应特定标记的实体。 | Tag: Crc32 | Tagged Entity: EntityId | Yes |

## TagGlobalNotificationBus
1
| 请求名称 | 说明 | 参数 | 返回值 | 可脚本化 |
|-|-|-|-|-|
| `OnEntityTagAdded` | 当特定标记被添加到任何实体时通知侦听器。 | Tag: Crc32 | Tagged Entity: EntityId | Yes |
| `OnEntityTagRemoved` | 当特定标记从任何实体中移除时，通知侦听器。 | Tag: Crc32 | Untagged Entity: EntityId | Yes |

更多信息，请参阅 [使用事件总线 (EBus) 系统](/docs/user-guide/programming/messaging/ebus/)。
