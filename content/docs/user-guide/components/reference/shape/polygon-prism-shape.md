---
title: Polygon Prism Shape 组件
linktitle: Polygon Prism Shape
description: ' Open 3D Engine (O3DE) Polygon Prism Shape 组件参考。 '
weight: 100
---



**Polygon Prism Shape** 组件可创建透明的长方体体积。棱柱形状由位于局部 XY 平面上的点定义。点位置将创建相同的平行平面，这些平面由高度值分隔，并由直角边连接。多边形棱柱形状可以使用组件的 **Height** 属性和 **Edit** 功能来定义。多边形棱柱形状可以有三条或更多不自相交的边。Polygon Prism Shape （多边形棱柱形状） 组件不是网格，而是一个辅助几何体，可用于定义区域光、AI、形状渐变、音频、植被、PhysX 以及任何可利用 Shape EBus 的应用程序的体积。有关使用 Shape 组件的更多信息，请参阅 [Shape 组件](/docs/user-guide/components/reference/shape/)。

## 提供者

[O3DE Core (LmbrCentral) Gem](/docs/user-guide/gems/reference/o3de-core)

## Polygon Prism Shape 属性

![Polygon Prism Shape component properties](/images/user-guide/components/reference/shape/polygon-prism-shape-component-ui-01.png)
2
| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **Visible** | 启用此选项可始终在视区中显示形状，即使未选择实体也是如此。禁用可在未选择实体时隐藏形状。 | Boolean | `Enabled` |
| **Game View** | 启用此选项可在游戏模式下显示形状。 | Boolean | `Disabled` |
| **Filled** | 启用此选项可将形状显示为已填充。 禁用以将形状显示为线框。 | Boolean | `Enabled` |
| **Shape Color** | 形状的颜色。 | 每通道 8 位颜色： 0-255 | `255,255,199` |
| **Height** | 形状的高度（以米为单位）。 | 0.0 to Infinity | `1.0` |
| **Edit** | 选择 **Edit** 按钮进入 Edit 模式。在编辑模式下，您可以使用下面的 [编辑模式操作](#edit-mode-actions) 中概述的方法在视区中修改形状的尺寸。在 Edit 模式下，菜单栏中的 Edit （编辑） 菜单显示可用的操作和热键。要退出 Edit 模式，请在组件界面中选择 **Done**。 |  |  |

## 编辑模式操作

* **选择顶点** - **左键单击** 任何顶点。
* **添加到选择项** - 按住 **Ctrl** 并 **左键单击** 未选中的顶点。
* **从选择中删除** - 按住 **Ctrl** 并 **左键单击** 选定的顶点。
* **选择多个** - 按住 Shift** 和 **左键单击** 并在多个顶点上拖动。
* **移动顶点** - **左键单击** 并拖动所选顶点的红色和绿色变换操纵器。顶部和底部棱柱平面必须平行，因此顶点移动仅限于 XY 平面。
* **添加顶点** - 按住 **Ctrl** 并 **左键单击** 现有顶点之间的边缘。
* **删除顶点** - 按住 **Alt** 并 **左键单击** 顶点。
* **删除多个顶点** - 按 **删除** 可删除所有选定的顶点。
* **修改高度** - **单击鼠标左键** 并拖动多边形棱柱形状顶部中心的蓝色变换操纵器。
* **将顶点对齐到位置** - 在视口中按住 **Ctrl + Shift** 和 **左键单击** 以将所选顶点对齐到该位置。
* **将顶点对齐到网格** - 如果在编辑模式下启用了 **对齐网格** 工具，则顶点将对齐到构造平面上的位置。

## PolygonPrismShapeComponentRequestBus

将以下请求函数与 '`PolygonPrismShapeComponentRequestBus`' 事件总线接口结合使用，以便与游戏中的 Polygon Prism Shape 组件进行通信。Polygon Prism Shape 组件也使用 '`VertexContainer`' 函数。有关更多信息，请参阅 [顶点容器](/docs/user-guide/components/reference/shape/vertex-container/) 。
| 方法名称 | 说明 | 参数 | 返回值 | 脚本化 |
|-|-|-|-|-|
| `AddVertex` | 将顶点添加到多边形棱柱形状。 | Vertex: Vector2 | None | Yes |
| `ClearVertices` | 从多边形棱柱形状中删除所有顶点。  | None | None | Yes |
| `GetPolygonPrism` | 返回一个常量指针，该指针指向包含属性 '`Height`' 和 '`vertexContainer`' 的基础多边形棱柱形状对象。 | None | `AZ::ConstPolygonPrismPtr` | Yes |
| `InsertVertex` | 在指定的索引位置插入顶点。 | Index: Integer, Vertex: Vector2 | None | Yes |
| `RemoveVertex` | 删除指定索引位置的顶点。| Index: Integer | None | Yes |
| `SetHeight` | 设置多边形棱柱形状的 **Height**。 | Height: Float | None | Yes |
| `UpdateVertex` | 修改指定索引位置的顶点。 | Index: Integer, Vertex: Vector2 | None | Yes |

请参阅 [Shape 组件 Ebus 接口](./#shape-component-ebus-interface) 以获取所有 Shape 组件可用的功能说明。
