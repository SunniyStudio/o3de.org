---
linkTitle: Vegetation Layer Blocker (Mesh)
title: Vegetation Layer Blocker (Mesh) 组件
description: 使用 Open 3D Engine （O3DE） 中的 Vegetation Layer Blocker （Mesh） 组件定义网格或角色周围无法生成动态植被的区域。
weight: 400
---

使用 **Vegetation Layer Blocker （Mesh）** 组件在网格或角色周围创建一个无法生成动态植被的区域。

## 提供者

[Vegetation Gem](/docs/user-guide/gems/reference/environment/vegetation/)

## 依赖

使用 Vegetation Layer Blocker （Mesh） 组件时，添加以下必需组件之一：
- [Mesh](./../atom/mesh/) 组件或 [Actor](./../animation/actor/) 组件，定义阻止体的区域。

## Vegetation Layer Blocker (Mesh) 属性

![Vegetation Layer Blocker (Mesh) component properties](/images/user-guide/components/reference/vegetation/vegetation-layer-blocker-mesh-component.png)

| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **Override Preview Settings** | 如果为 '`Enabled`'，则 **Preview Settings** 属性将确定阻止体的形状。 如果为“`Disabled`”，则阻止体的边界由附加的 Mesh 或 Actor 组件设置。 | Boolean | `Disabled` |
| **Pin Preview to Shape** | 如果 **Constrain to Shape** 为 `Enabled`，则设置具有兼容形状组件的实体，以用作阻止体的边界。<br> <br>仅当 **Override Preview Settings** 为 `Enabled` 时，此字段才可用。| EntityId | Current Entity |
| **Preview Position** | 设置阻止体的世界位置。<br> <br>仅当 **Override Preview Settings** 为`Enabled`， 并且未在 **Pin Preview to Shape** 中选择任何实体时，此字段才可用。 | Vector3: -Infinity to Infinity | X:`0.0`, Y:`0.0`, Z:`0.0` |
| **Preview Size** | 如果 **Constrain to Shape** 为 '`Disabled`'，则设置阻止器的尺寸。<br> <br>仅当 **覆盖Override Preview Settings预览设置** 为`Enabled` 时，此字段才可用。 | Vector3: 0.0 to Infinity | X:`1.0`, Y:`1.0`, Z:`1.0` |
| **Constrain to Shape** | 如果为 '`Enabled`'，则阻止体将使用在 **Pin Preview to Shape** 中选择的实体的边界。<br> <br>*仅当 **Override Preview Settings** 为 `Enabled` 并且在 **Pin Preview to Shape** 中选择了实体时，此字段才可用。 | Boolean | `Disabled` |
| **Layer Priority** | 定义应用植被区域和阻碍因素的高级顺序。 | `Background` or `Foreground` | `Foreground` |
| **Sub Priority** | 定义植被区域或阻止体在图层中的应用顺序。数字越大，优先级越高。 | 0-10000 | `10000` |
| **Inherit Behavior** | 允许父实体的形状、修饰符和过滤器影响此植被图层。 | Boolean | `Enabled` |
| **Mesh Height Percent Min** | 设置网格高度的百分比（从下往上）用于确定相交测试的下限。 | Float: 0.0 - 1.0 | `0.0` |
| **Mesh Height Percent Max** | 设置网格高度的百分比（从下往上）用于确定相交测试的上限。 | Float: 0.0 - 1.0 | `1.0` |
| **Block When Invisible** | 如果为 '`Disabled`'，则仅当 Mesh 或 Actor 组件可见时，阻止器才会阻止植被。 | Boolean | `Enabled` |
| **Draw Debug Bounds** | 绘制网格高度边界。 | Boolean | `Disabled` |

## MeshBlockerRequestBus

将以下请求函数与“`MeshBlockerRequestBus`”事件总线接口结合使用，以便与游戏中的植被图层阻止体 （Mesh） 组件进行通信。

| 方法名称 | 说明 | 参数 | 返回值 | 可脚本化 |
|-|-|-|-|-|
| `GetAreaLayer` | 返回网格阻止体的 **Layer Priority**。返回 'Background' 的 '0' 和 'foreground' 的 '1'。 | None | Layer Priority: Integer | Yes |
| `GetAreaPriority` | 返回网格阻止体的 **Sub Priority**。 | None | Sub Priority: Integer | Yes |
| `GetAreaProductCount` | 返回在网格阻塞器的植被区域中生成的植被实例的数量。 | None  | Count: Integer | Yes |
| `GetBlockWhenInvisible` | 返回网格阻止体的 **不可见时阻止（Block When Invisible）** 属性的配置。| None | Boolean | Yes |
| `GetInheritBehavior` | 返回网格阻止体的 **Inherit Behavior** 属性的配置。 | None | Boolean | Yes |
| `GetMeshHeightPercentMax` | 返回网格阻止体的 **Mesh Height Percent Max** 属性。 | None | Height Ratio: Float | Yes |
| `GetMeshHeightPercentMin` | 返回网格阻止体的 **Mesh Height Percent Min** 属性。| None | Height Ratio: Float | Yes |
| `SetAreaLayer` | 设置网格阻止体的 **Layer Priority**。返回 '`Background`' 的 '`0`' 和 '`foreground`' 的 '`1`'。 | Layer Priority: Integer | None | Yes |
| `SetAreaPriority` | 设置网格阻止体的 **Sub Priority**。 | Sub Priority: Integer | None | Yes |
| `SetBlockWhenInvisible` | 设置网格阻止体的 **Block When Invisible** 属性的配置。 | Boolean | None | Yes |
| `SetInheritBehavior` | 设置网格阻塞程序的 **Inherit Behavior** 属性的配置。 | Boolean | None | Yes |
| `SetMeshHeightPercentMax` | 设置网格阻止体的 **Mesh Height Percent Max** 属性。| Height Ratio: Float | None | Yes |
| `SetMeshHeightPercentMin` | 设置网格块体的 **Mesh Height Percent Min** 属性。 | Height Ratio: Float | None | Yes |
