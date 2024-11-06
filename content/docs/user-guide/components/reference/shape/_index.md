---
title: Shape 组件
linktitle: Shape
description: ' 在Open 3D Engine (O3DE)中使用Shape组件。 '
---



**Shape**组件生成辅助几何体，这些几何体可用于定义区域光和形状渐变的形状，以及 AI、音频、雾、生成器、植被和 PhysX 的体积和区域。

## 可用的形状

| Shape 组件 | 形状 | 用于 |
|-|-|-|
| [Axis Aligned Box Shape](axis-aligned-box-shape) | 轴对齐的长方体几何体 | Terrain, Volume, Triggers |
| [Box Shape](box-shape) | 箱体几何体 | Volumes, Triggers |
| [Capsule Shape](capsule-shape) | 胶囊体（末端有半球体的圆柱体）几何体 | Volumes, Triggers |
| [Compound Shape](compound-shape) | 聚合由其他 Shape 组件定义的几何体 | Volumes, Triggers |
| [Cylinder Shape](cylinder-shape) | 圆柱体几何体 | Volumes, Triggers |
| [Disk Shape](disk-shape) | 圆盘几何 | Areas, Triggers |
| [Polygon Prism Shape](polygon-prism-shape) | N 边棱柱几何形状 | Volumes, Triggers |
| [Quad Shape](quad-shape) | 四平面几何| Areas, Triggers |
| [Sphere Shape](sphere-shape) | 球体几何体 | Volumes, Triggers |
| [Spline](spline) | 直线和曲线 | Paths |
| [Tube Shape](tube-shape) | 管材几何形状 | Volumes, Triggers |

{{< note >}}
**White Box**组件与其他 Shape 组件不共享相同的功能。 以下各节不适用于 White Box 组件。 有关更多信息，请参阅 [White Box](white-box)文档。
{{< /note >}}

## 使用 Shape 组件

一个实体只能有一个 Shape 组件。

{{< important >}}
始终使用 Shape 的组件属性来缩放形状，例如 **Dimensions**、**Height** 和 **Radius** 属性。请勿使用实体的 Transform （变换） 组件来缩放 Shape （形状） 组件。
{{< /important >}}

包含 Shape 组件的实体应具有统一且标准化的缩放;也就是说，Transform 组件的 **Scale** 属性应为 **X：** `1.0`， **Y：** `1.0`， **Z：** `1.0`。如果形状组件的缩放不均匀，则渲染和交集测试将使用 Transform 组件的 **Scale** 属性的最大向量，从而产生不需要的结果。

默认情况下，形状在 **Open 3D Engine （O3DE） 编辑器**中始终可见。您可以通过禁用形状组件中的 **Visible** 属性来隐藏未选定实体上的形状。

要在游戏模式下显示 Shape 组件以进行调试，请启用 **Game View** 属性。

每个 Shape 组件都提供了一个通用的 `ShapeService`，它公开了所有形状的通用功能。每个形状还提供更具体的服务，例如 `BoxShapeService` 或 `SphereShapeService`。

## Shape组件 EBus 接口

所有 Shape 组件都提供对两个单独请求总线的访问。第一条总线是 '`ShapeComponentRequestsBus`'，它返回有关 Shape 组件的一般信息。第二个总线是 '`ShapeNameComponentRequestsBus`'，它返回特定形状的属性配置。有关`ShapeNameComponentRequestsBus`的信息，可以在每个 Shape 组件的参考页上找到。

所有形状共享一个名为 '`ShapeComponentNotificationsBus`' 的通知总线。

您可以将以下函数与事件总线接口结合使用，以便与游戏中的其他组件进行通信。

## ShapeComponentRequestsBus

| 方法名称 | 说明 | 参数 | 返回值 | 脚本化 |
|-|-|-|-|-|
| `DistanceFromPoint` | 返回指定点与形状之间的最小距离。 | Point: Vector3 点来计算距离。  | Float - 从点到形状的距离。 | Yes |
| `DistanceSquaredFromPoint` | 返回指定点与形状之间的最小平方距离。| Point: Vector3 点，用于计算平方距离。 | Float: 从点到形状的平方距离。 | Yes |
| `GetEncompassingAabb` | 返回包含整个形状的 AABB（轴对齐边界框）。 | None | 包住形状的`AZ::Aabb`。  | Yes |
| `GetShapeType` | 返回组件的指定 **Shape Type**。 | None | `AZ::Crc32(<shape_type_name>)`: 例如。 `AZ::Crc32("Sphere")` | Yes |
| `IsPointInside` | 检查指定的点是否在形状内部。  | Point: Vector3 指向进行检查。 | Boolean: 如果指定的点位于形状内部，则返回 '`True`'。如果点不在形状内部，则返回 '`False`'。 | Yes |

## ShapeComponentNotificationsBus

| 通知名称 | 说明 | 参数 | 返回值 | 脚本化 |
|-|-|-|-|-|
| `OnShapeChanged` | 通知侦听器形状已更新。 | `ShapeChangeReasons`: 指示形状是由转换更改还是形状属性更改更新。 | Void | Yes |
