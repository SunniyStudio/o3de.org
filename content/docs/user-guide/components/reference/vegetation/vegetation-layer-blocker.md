---
linkTitle: Vegetation Layer Blocker
title: Vegetation Layer Blocker 组件
description: 使用 Open 3D Engine （O3DE） 中的 Vegetation Layer Blocker 组件定义动态植被无法生成的区域。
weight: 350
---

使用 **Vegetation Layer Blocker** 组件创建动态植被无法生成的区域。

## 提供者

[Vegetation Gem](/docs/user-guide/gems/reference/environment/vegetation/)

## 依赖

使用 Vegetation Layer Blocker 组件时，添加以下必需组件：
- 以下[Shape](./../shape/) 组件之一: [Axis Aligned Box](./../shape/axis-aligned-box-shape), [Box](./../shape/box-shape), [Capsule](./../shape/capsule-shape), [Compound](./../shape/compound-shape), [Cylinder](./../shape/cylinder-shape), [Disk](./../shape/disk-shape), [Polygon Prism](./../shape/polygon-prism-shape), [Quad](./../shape/quad-shape), [Shape Reference](./../shape/shape-reference), [Sphere](./../shape/sphere-shape), 或 [Tube](./../shape/tube-shape),  以定义阻止体化生成的区域。

## Vegetation Layer Blocker 属性

![Vegetation Layer Blocker component properties](/images/user-guide/components/reference/vegetation/vegetation-layer-blocker-mesh-component.png)

| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **Override Preview Settings** | 如果 `Enabled`, **Preview Settings** 属性确定阻止体的形状。 如果 `Disabled`, 阻止体的边界由附加的 Shape 组件设置。  | Boolean | `Disabled` |
| **Pin Preview to Shape** | 如果 **Constrain to Shape** 为 '`Enabled`'，则设置具有兼容形状组件的实体，以用作阻止体的边界。<br> <br>仅当 **Override Preview Settings** 为`Enabled` 时，此字段才可用。 | EntityId | Current Entity |
| **Preview Position** | 设置阻止体的世界位置。<br> <br>仅当 **Override Preview Settings** 为 '已启用' 并且未在 **Pin Preview to Shape** 中选择任何实体时，此字段才可用。 | Vector3: -Infinity to Infinity | X:`0.0`, Y:`0.0`, Z:`0.0` |
| **Preview Size** | 如果 **Constrain to Shape** 为 '`Disabled`'，则设置阻止器的尺寸。<br> <br>仅当 **Override Preview Settings** 为 `Enabled` 时，此字段才可用。 | Vector3: 0.0 to Infinity | X:`1.0`, Y:`1.0`, Z:`1.0` |
| **Constrain to Shape** | 如果为 '`Enabled`'，则阻止体将使用在 **Pin Preview to Shape** 中选择的实体的边界。<br> <br>仅当 **Override Preview Settings** 为 `Enabled` 并且在 **Pin Preview to Shape** 中选择了实体时，此字段才可用。 | Boolean | `Disabled` |
| **Layer Priority** | 定义应用植被区域和阻碍因素的高级顺序。 | `Background` or `Foreground` | `Foreground` |
| **Sub Priority** | 定义植被区域或阻止体在图层中的应用顺序。数字越大，优先级越高。 | 0-10000 | `10000` |
| **Inherit Behavior** | 允许父实体的形状、修饰符和过滤器影响此植被图层。 | Boolean | `Enabled` |

## BlockerRequestBus

将以下请求函数与 '`BlockerRequestBus`' EBus 接口结合使用，以便与游戏中的 Vegetation Layer Blocker 组件进行通信。

| 方法名称 | 说明 | 参数 | 返回值 | 可脚本化 |
|-|-|-|-|-|
| `GetAreaLayer` | 返回阻止体的 **Layer Priority**。返回 '`Background`' 的 '`0`' 和 '`foreground`' 的 '`1`'。 | None | Layer Priority: Integer | Yes |
| `GetAreaPriority` | 返回阻止体的 **Sub Priority**。 | None | Sub Priority: Integer | Yes |
| `GetAreaProductCount` | 返回在 Blocker 的植被区域中生成的植被实例数。 | None  | Count: Integer | Yes |
| `GetInheritBehavior` | 返回阻止体的 **Inherit Behavior** 属性的配置。 | None | Boolean | Yes |
| `SetAreaLayer` | 设置阻止体的 **Layer Priority**。返回 '`Background`' 的 '`0`' 和 '`foreground`' 的 '`1`'。 | Layer Priority: Integer | None | Yes |
| `SetAreaPriority` | 设置阻止体的 **Sub Priority**。 | Sub Priority: Integer | None | Yes |
| `SetInheritBehavior` | 设置阻止体的 **Inherit Behavior** 属性的配置。 | Boolean | None | Yes |
