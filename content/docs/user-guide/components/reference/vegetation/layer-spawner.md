---
linkTitle: Vegetation Layer Spawner
title: Vegetation Layer Spawner 组件
description: 使用 Vegetation Layer Spawner 组件定义在 Open 3D Engine （O3DE） 中程序化放置动态植被或其他静态网格的区域和规则。
weight: 500
---

使用 **Vegetation Layer Spawner** 组件来定义区域和规则，以便在 **Open 3D Engine （O3DE）** 关卡中程序化地放置动态植被或其他静态网格。

使用 Vegetation Layer Spawner （植被图层生成器） 组件，您可以执行以下操作：
- 在运行时在用户定义的区域内创建植被。
- 配置图层设置以确定应用植被图层的深度或相对顺序。
- 添加 [Vegetation Modifier](./../vegetation-modifiers/) 和 [Vegetation Filter](./../vegetation-filters/) 组件为放置的植被添加变化。
- 使用  [Vegetation Asset Weight Selector](vegetation-asset-weight-selector) 组件来确定要在给定位置放置哪些植被资产。
- 控制附加组件的预览设置。
从父[Vegetation Layer Blender](vegetation-layer-blender) 组件继承行为。

有关如何使用 Vegetation Layer Spawner 的信息，请参阅 [动态植被](/docs/user-guide/gems/reference/environment/vegetation/)。

## 提供者

[Vegetation Gem](/docs/user-guide/gems/reference/environment/vegetation/)

## 依赖

在使用 Vegetation Layer Spawner 组件时，添加以下必需的组件：
- 以下 [Shape](./../shape/) 组件中的一个： [Axis Aligned Box](./../shape/axis-aligned-box-shape), [Box](./../shape/box-shape), [Capsule](./../shape/capsule-shape), [Compound](./../shape/compound-shape), [Cylinder](./../shape/cylinder-shape), [Disk](./../shape/disk-shape), [Polygon Prism](./../shape/polygon-prism-shape), [Quad](./../shape/quad-shape), [Shape Reference](./../shape/shape-reference), [Sphere](./../shape/sphere-shape), 或 [Tube](./../shape/tube-shape),  定义植被的生成区域。
- 一个 [Vegetation Asset List](vegetation-asset-list) 或 [Vegetation Asset List Combiner](vegetation-asset-list-combiner) 组件列出网格资产、材质资产和植被的其他设置。

## Vegetation Layer Spawner 属性

![Vegetation Layer Spawner component properties](/images/user-guide/components/reference/vegetation/vegetation-layer-spawner-component.png)

| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **Override Preview Settings** | 如果 `Enabled`， **Preview Settings** 属性确定植被图层预览的形状。 如果 `Disabled`， 植被图层预览的边界由附加的 Shape 组件设置。  | Boolean | `Disabled` |
| **Pin Preview to Shape** | 如果 **Constrain to Shape** 为 `Enabled`，则使用具有兼容形状组件的实体作为植被图层预览的边界。<br> <br>仅当 **Override Preview Settings** 为 `Enabled` 时，此字段才可用。* | EntityId | Current Entity |
| **Preview Position** | 设置植被图层预览的世界位置。<br> <br>仅当 **Override Preview Settings** 为 `Enabled` 并且未在 **Pin Preview to Shape** 中选择任何实体时，此字段才可用。 | Vector3: -Infinity to Infinity | X:`0.0`, Y:`0.0`, Z:`0.0` |
| **Preview Size** | 如果 **Constrain to Shape** 为 '`Disabled`'，则设置植被图层预览的尺寸。<br> <br>仅当 **Override Preview Settings** 为`Enabled`时，此字段才可用。 | Vector3: 0.0 to Infinity | X:`1.0`, Y:`1.0`, Z:`1.0` |
| **Constrain to Shape** | 如果`Enabled`'，则植被图层预览将使用在 **Pin Preview to Shape** 中选择的实体的边界。<br><br仅当 **Override Preview Settings** 为 `Enabled` 并且在 **Pin Preview to Shape** 中选择了实体时，此字段才可用。 | Boolean | `Disabled` |
| **Layer Priority** | 定义应用植被区域的高级顺序。 | `Background` or `Foreground` | `Foreground` |
| **Sub Priority** | 定义植被区域在图层中的应用顺序。数字越大，优先级越高。 | 0-10000 | `0` |
| **Inherit Behavior** | 允许父实体的形状、修饰符和过滤器影响此植被图层。 | Boolean | `Enabled` |
| **Allow Empty Assets** | 允许未指定的资产占用空间并阻止其他植被。 | Boolean | `Enabled` |
| **Filter Stage** | 定义是在修饰符之前还是之后应用滤镜。 | `PreProcess` or `PostProcess` | `PreProcess` |

## VegetationSpawnerRequestBus

将以下请求函数与 '`VegetationSpawnerRequestBus`' 事件总线接口结合使用，以便与游戏中的 Vegetation Layer Spawner 组件进行通信。

| 方法名称 | 说明 | 参数 | 返回值 | 可脚本化 |
|-|-|-|-|-|
| `GetAllowEmptyMeshes` | 如果 **Allow Empty Assets** 为 '`Enabled`'，则返回 '`True`'。 | None | Boolean | Yes |
| `GetAreaLayer` | 返回植被图层的 **Layer Priority**。 | None | Layer Priority: String | Yes |
| `GetAreaPriority` | 返回植被图层的 **Sub Priority**。 | None | Sub Priority: Integer | Yes |
| `GetAreaProductCount` | 返回在植被图层中生成的植被实例数。 | None | Count: Integer | Yes |
| `GetFilterStage` | 返回植被图层的 **Filter Stage**。 | None | Filter Stage: String | Yes |
| `GetInheritBehavior` | 如果 **Inherit Behavior** 为 '`Enabled`'，则返回 '`True`'。 | None | Boolean | Yes |
| `SetAllowEmptyMeshes` | 设置 **Allow Empty Assets** 属性的值。 | Boolean | None | Yes |
| `SetAreaLayer` | 设置 **Layer Priority** 属性的值。 | Layer Priority: String | None | Yes |
| `SetAreaPriority` | 设置 **Sub Priority** 属性的值。 | Sub Priority: Integer | None | Yes |
| `SetFilterStage` | 设置 **Filter** 属性的值。 | Filter Stage: String | None | Yes |
| `SetInheritBehavior` | 设置 **Inherit Behavior** 属性的值。 | Boolean | None | Yes |
