---
title: Entity Reference 组件
linktitle: Entity Reference
description: 'Open 3D Engine (O3DE) Entity Reference 组件参考。'
toc: true
---

**Entity Reference** 组件允许您跟踪一个或多个实体 ID，您可以在编辑器模式下通过脚本或 C++ 代码方便地访问这些 ID。

## 提供方 ##

[Atom Gem](/docs/user-guide/gems/reference/rendering/atom/atom/)


## 属性

![Entity Reference Component UI](/images/user-guide/components/reference/atom/entity-reference-ui.png)

| 属性 | 说明 |
| - | - |
| **EntityIdReferences** | 包含 ID 将被跟踪的实体列表。 |

## 用法 ##

在编辑器模式下，调用`EntityReferenceRequestBus`来检索组件引用的实体列表。
本例使用 O3DE 的 Python 控制台来检索实体 ID 列表。
```python
# entityId contains the Entity Reference component.
entityIdReferences = azlmbr.entity.EntityReferenceRequestBus(azlmbr.bus.Event, 'GetEntityReferences', entityId)
 
for id in entityIdReferences:
    print(id.ToString())
 
 
# Expected Output: Entity IDs of the referenced entities.
# [3664549435643349363]
# [17340279414445478303]
```
