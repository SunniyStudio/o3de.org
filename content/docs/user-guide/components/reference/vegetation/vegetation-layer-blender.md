---
linkTitle: Vegetation Layer Blender
title: Vegetation Layer Blender 组件
description: 将植被图层与 Open 3D Engine （O3DE） 中的 Vegetation Layer Blender 组件组合在一起。
weight: 300
---

将植被图层与 **Vegetation Layer Blender** 组件组合在一起。

## Provider

[Vegetation Gem](/docs/user-guide/gems/reference/environment/vegetation/)

## Vegetation Layer Blender 属性

![Vegetation Layer Blender component properties](/images/user-guide/components/reference/vegetation/vegetation-layer-blender-component.png)
2
| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **Override Preview Settings** | 如果为`Enabled`，则 **Preview Settings** 属性将确定植被图层预览的形状。 如果为`Disabled`，则植被图层预览的边界由在**Vegetation Areas**中选择的植被图层的Shape组件设置。  | Boolean | `Disabled` |
| **Pin Preview to Shape** | 如果 **Constrain to Shape** 为 `Enabled`，则设置具有兼容形状组件的实体，以用作植被图层预览的边界。<br> <br>仅当 **Override Preview Settings** 为`Enabled`时，此字段才可用。 | EntityId | Current Entity |
| **Preview Position** | 设置植被图层预览的世界位置。<br> <br>仅当 **Override Preview Settings** 为`Enabled`， 并且未在 **Pin Preview to Shape** 中选择任何实体时，此字段才可用。* | Vector3: -Infinity to Infinity | X:`0.0`, Y:`0.0`, Z:`0.0` |
| **Preview Size** | Sets the dimensions of the vegetation layer preview if **Constrain to Shape** is `Disabled`.<br> <br>*This field is available only if **Override Preview Settings** is `Enabled`.* | Vector3: 0.0 to Infinity | X:`1.0`, Y:`1.0`, Z:`1.0` |
| **Constrain to Shape** | If `Enabled`, the vegetation layer preview will use the bounds of the entity selected in **Pin Preview to Shape**.<br> <br>*This field is available only if **Override Preview Settings** is `Enabled` and an entity is selected in **Pin Preview to Shape**.* | Boolean | `Disabled` |
| **Layer Priority** | Defines a high level order that vegetation areas are applied. | `Background` or `Foreground` | `Foreground` |
| **Sub Priority** | Defines the order that vegetation areas are applied within a layer. Larger numbers have a higher priority. | 0-10000 | `0` |
| **Inherit Behavior** | Allow shapes, modifiers and filters of a parent entity to affect this vegetation layer. | Boolean | `Enabled` |
| **Propagate Behavior** | Allow shapes, modifiers and filters of this vegetation layer to affect child entities. | Boolean | `Enabled` |
| **Vegetation Areas** | An ordered list of vegetation layers. | Array: EntityId | None |


## AreaBlenderRequestBus

Use the following request functions with the `AreaBlenderRequestBus` EBus interface to communicate with Vegetation Layer Blender components in your game.

| Method Name | Description | Parameter | Return | Scriptable |
|-|-|-|-|-|
| `AddAreaEntityId` | Adds a vegetation layer to a **Vegetation Areas** array. | EntityId | None | Yes |
| `GetAreaEntityId` | Returns the EntityId of an entry in a **Vegetation Areas** array at the specified index. | Vegetation Areas Index: Integer | EntityId | Yes |
| `GetAreaLayer` | Returns the **Layer Priority** of a layer blender. Returns `0` for `Background` and `1` for `Foreground`. | None | Layer Priority: Integer | Yes |
| `GetAreaPriority` | Returns the **Sub Priority** of a layer blender. | None | Sub Priority: Integer | Yes |
| `GetAreaProductCount` | Returns the number of vegetation instances spawned in a layer blender's vegetation area. |None  | Count: Integer | Yes |
| `GetInheritBehavior` | Returns the configuration of a layer blender's **Inherit Behavior** property. | None | Boolean | Yes |
| `GetNumAreas` | Returns the number of entries in a layer blender's **Vegetation Areas** array. | None | Count: Integer | Yes |
| `GetPropagateBehavior` | Returns the configuration of a layer blender's **Propagate Behavior** property. | None | Boolean | Yes |
| `RemoveAreaEntityId` | Removes a vegetation layer from the **Vegetation Areas** array of a layer blender. | Vegetation Areas Index: Integer | None | Yes |
| `SetAreaLayer` | Sets the **Layer Priority** of layer blender. | Layer Priority: Integer | None | Yes |
| `SetAreaPriority` | Sets the **Sub Priority** of a layer blender. | Sub Priority: Integer | None | Yes |
| `SetInheritBehavior` | Sets the **Inherit Behavior** property of a layer blender. | Boolean | None | Yes |
| `SetPropagateBehavior` | Sets the **Propagate Behavior** property of a layer blender. | Boolean | None | Yes |
