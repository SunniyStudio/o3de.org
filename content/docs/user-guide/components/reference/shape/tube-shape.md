---
title: Tube Shape 组件
linktitle: Tube Shape
description: ' Open 3D Engine (O3DE) Tube Shape组件参考。 '
weight: 100
---



**Tube Shape** 组件创建一个透明的封闭圆柱形体积，该体积适合 [Spline 组件](/docs/user-guide/components/reference/shape/spline/)。可以使用 **Radius** 和 **Variable Radius** 属性以及编辑 [Spline 组件](/docs/user-guide/components/reference/shape/spline/) 来编辑管子的尺寸。Tube Shape （管状形状） 组件不是网格，而是一个辅助几何体，可用于定义形状渐变、音频、植被、PhysX 以及任何可利用 Shape EBus （形状事件总线） 的应用程序的体积。有关使用形状组件的更多信息，请参阅 [Spline 组件](/docs/user-guide/components/reference/shape/spline/)。

## Provider

[O3DE Core (LmbrCentral) Gem](/docs/user-guide/gems/reference/o3de-core)

## 依赖

[Spline component](/docs/user-guide/components/reference/shape/spline/)

## Tube Shape 属性

![Tube Shape component properties](/images/user-guide/components/reference/shape/tube-shape-component-ui-01.png)
2
| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **Visible** | 启用此选项可始终在视区中显示形状，即使未选择实体也是如此。禁用可在未选择实体时隐藏形状。 | Boolean | `Enabled` |
| **Game View** | 启用此选项可在游戏模式下显示形状。 | Boolean | `Disabled` |
| **Filled** | 启用此选项可将形状显示为已填充。 禁用以将形状显示为线框。 | Boolean | `Enabled` |
| **Shape Color** | 形状的颜色。 | 每通道 8 位颜色： 0-255 | `255,255,199` |
| **Tube Shape - Radius** | 定义 Tube Shape 宽度的统一半径。 | 0.1 to Infinity | `1.0` |
| **Tube Shape - Variable Radius** | Variable Radius 属性包含一个列表，其中样条主干的每个点对应一个项目。列表值将添加到样条曲线上相应点的统一 Radius 值中，以在每个点处创建具有可变半径的管。  | 0.0 to Infinity | `0.0` |
| **Edit** | 选择 **Edit** 按钮进入 Edit 模式。在编辑模式下，您可以使用下面的 [编辑模式操作](#edit-mode-actions) 中概述的方法在视区中修改形状的尺寸。在 Edit 模式下，菜单栏中的 Edit （编辑） 菜单显示可用的操作和热键。要退出 Edit 模式，请在组件界面中选择 **Done**。 |  |  |

## 编辑模式操作

* **左键单击** 并拖动视口中每个样条点处的黑色手柄，以修改 Tube Shape 每个部分的 **Variable Radius**。

## TubeShapeComponentRequestsBus

将以下请求函数与 '`TubeShapeComponentRequestsBus`' 事件总线接口结合使用，以便与游戏中的 Tube Shape 组件进行通信。

| 方法名称 | 说明 | 参数 | 返回值 | 脚本化 |
|-|-|-|-|-|
| `GetRadius` | 返回管的 **Radius**。 | None | Radius: Float | Yes |
| `GetTotalRadius` | 返回管子的总插值半径。这是 radius 和变量 radius 之和。 | Index: Integer | Total Radius: Float | Yes |
| `GetVariableRadius` | 返回沿样条曲线的 **Variable Radius**。 | Index: Integer | Variable Radius: Float | Yes |
| `SetRadius` | 设置管的 **Radius**。 | Radius: Float | None | Yes |
| `SetVariableRadius` | 在样条曲线点处设置管的 **Variable Radius**。 | Index: Integer, Radius: Float | None | Yes |

请参阅 [Shape 组件 Ebus 接口](./#shape-component-ebus-interface) 以获取所有 Shape 组件可用的功能说明。
