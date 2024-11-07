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

| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **Override Preview Settings** | 如果为`Enabled`，则 **Preview Settings** 属性将确定植被图层预览的形状。 如果为`Disabled`，则植被图层预览的边界由在**Vegetation Areas**中选择的植被图层的Shape组件设置。  | Boolean | `Disabled` |
| **Pin Preview to Shape** | 如果 **Constrain to Shape** 为 `Enabled`，则设置具有兼容形状组件的实体，以用作植被图层预览的边界。<br> <br>仅当 **Override Preview Settings** 为`Enabled`时，此字段才可用。 | EntityId | Current Entity |
| **Preview Position** | 设置植被图层预览的世界位置。<br> <br>仅当 **Override Preview Settings** 为`Enabled`， 并且未在 **Pin Preview to Shape** 中选择任何实体时，此字段才可用。* | Vector3: -Infinity to Infinity | X:`0.0`, Y:`0.0`, Z:`0.0` |
| **Preview Size** | 如果 **Constrain to Shape** 为 '`Disabled`'，则设置植被图层预览的尺寸。<br> <br>仅当 **Override Preview Settings** 为 '`Enabled`' 时，他的字段才可用。 | Vector3: 0.0 to Infinity | X:`1.0`, Y:`1.0`, Z:`1.0` |
| **Constrain to Shape** | 如果 '`Enabled`'，则植被图层预览将使用在 **Pin Preview to Shape** 中选择的实体的边界。<br> <br>*仅当 **Pin Preview to Shape** 为  `Enabled`  并且在 **Override Preview Setting** 中选择了实体时，此字段才可用。 | Boolean | `Disabled` |
| **Layer Priority** | 定义应用植被区域的高级别顺序。 | `Background` or `Foreground` | `Foreground` |
| **Sub Priority** | 定义植被区域在图层中的应用顺序。数字越大，优先级越高。 | 0-10000 | `0` |
| **Inherit Behavior** | 允许父实体的形状、修饰符和过滤器影响此植被图层。 | Boolean | `Enabled` |
| **Propagate Behavior** | 允许此植被图层的形状、修饰符和过滤器影响子实体。 | Boolean | `Enabled` |
| **Vegetation Areas** | 植被图层的有序列表。 | Array: EntityId | None |

## AreaBlenderRequestBus

将以下请求函数与“`AreaBlenderRequestBus`”事件总线接口结合使用，以便与游戏中的 Vegetation Layer Blender 组件进行通信。

| 方法名称 | 说明 | 参数 | 返回值 | 可脚本化 |
|-|-|-|-|-|
| `AddAreaEntityId` | 将植被图层添加到 **Vegetation Areas** 数组中。 | EntityId | None | Yes |
| `GetAreaEntityId` | 返回 **Vegetation Areas** 数组中指定索引处条目的 EntityId。 | Vegetation Areas Index: Integer | EntityId | Yes |
| `GetAreaLayer` | 返回图层混合器的 **Layer Priority**。返回 '`Background`' 的 '`0`' 和 '`foreground`' 的 '`1`'。 | None | Layer Priority: Integer | Yes |
| `GetAreaPriority` | 返回图层混合器的 **Sub Priority**。 | None | Sub Priority: Integer | Yes |
| `GetAreaProductCount` | 返回在图层混合器的植被区域中生成的植被实例数。 |None  | Count: Integer | Yes |
| `GetInheritBehavior` | 返回图层混合器的 **Inherit Behavior** 属性的配置。 | None | Boolean | Yes |
| `GetNumAreas` | 返回图层混合器的 **Vegetation Areas** 数组中的条目数。 | None | Count: Integer | Yes |
| `GetPropagateBehavior` | 返回图层混合器的 **Propagate Behavior** 属性的配置。 | None | Boolean | Yes |
| `RemoveAreaEntityId` | 从图层混合器的 **Vegetation Areas** 数组中删除植被图层。 | Vegetation Areas Index: Integer | None | Yes |
| `SetAreaLayer` | 设置图层混合器的 **Layer Priority**。 | Layer Priority: Integer | None | Yes |
| `SetAreaPriority` | 设置图层混合器的 **Sub Priority**。 | Sub Priority: Integer | None | Yes |
| `SetInheritBehavior` | 设置图层混合器的 **Inherit Behavior** 属性。 | Boolean | None | Yes |
| `SetPropagateBehavior` | 设置图层混合器的 **Propagate Behavior** 属性。 | Boolean | None | Yes |
