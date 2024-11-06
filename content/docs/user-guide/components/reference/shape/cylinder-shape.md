---
title: Cylinder Shape 组件
linktitle: Cylinder Shape
description: ' Open 3D Engine (O3DE) Cylinder Shape 组件参考。 '
weight: 100
---



**Cylinder Shape** 组件创建一个以局部 Z 轴为导向的透明圆柱体。圆柱体的尺寸可以使用 **Height** 和 **Radius** 属性进行编辑。Cylinder Shape （圆柱体形状） 组件不是网格，而是一个辅助几何体，可用于定义生成器、形状渐变、音频、植被、PhysX 以及任何可利用形状事件总线的应用程序的体积。有关使用 Shape 组件的更多信息，请参阅 [Shape 组件](/docs/user-guide/components/reference/shape/)。

## 提供者

[O3DE Core (LmbrCentral) Gem](/docs/user-guide/gems/reference/o3de-core)

## Cylinder Shape 属性

![Capsule Shape component properties](/images/user-guide/components/reference/shape/cylinder-shape-component-ui-01.png)

| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **Visible** | 启用此选项可始终在视区中显示形状，即使未选择实体也是如此。禁用可在未选择实体时隐藏形状。 | Boolean | `Enabled` |
| **Game View** | 启用此选项可在游戏模式下显示形状。 | Boolean | `Disabled` |
| **Filled** | 启用此选项可将形状显示为已填充。 禁用以将形状显示为线框。 | Boolean | `Enabled` |
| **Shape Color** | 形状的颜色。 | 每通道颜色 8 位: 0-255 | `255,255,199` |
| **Height** | 形状的高度（以米为单位）。 | 0.0 to Infinity | `1.0` |
| **Radius** | 形状的半径（以米为单位）。 | 0.0 to Infinity | `0.5` |

## CylinderShapeComponentRequestsBus

将以下请求函数与 '`CylinderShapeComponentRequestsBus`' 事件总线接口结合使用，以便与游戏中的圆柱体形状组件进行通信。

| 方法名称 | 说明 | 参数 | 返回值 | 脚本化 |
|-|-|-|-|-|
| `GetCylinderConfiguration` | 返回圆柱体形状的配置。 | None | `CylinderShapeConfig` 对象，其中包含 '`Height`' 和 '`Radius`' 属性。 | Yes |
| `SetHeight` | 设置圆柱体形状的 **Height**。 | Height: Float | None | Yes |
| `SetRadius` | 设置圆柱体形状的 **Radius**。 | Radius: Float | None | Yes |

请参阅 [Shape 组件 Ebus 接口](./#shape-component-ebus-interface) 以获取所有 Shape 组件可用的功能说明。
