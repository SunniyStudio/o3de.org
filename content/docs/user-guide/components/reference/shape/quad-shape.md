---
title: Quad Shape 组件
linktitle: Quad Shape
description: ' Open 3D Engine (O3DE) Quad Shape组件参考。 '
weight: 100
---



**Quad Shape** 组件创建一个以局部 Z 轴为导向的透明方形平面。可以使用 **Width** 和 **Height** 属性编辑四边形的尺寸。Quad Shape （四边形） 组件不是网格，而是一个辅助几何体，可用于定义区域光源、生成器、形状渐变、音频、植被、PhysX 以及任何可利用 Shape EBus （形状事件总线） 的应用程序的区域。有关使用 Shape 组件的更多信息，请参阅 [Shape 组件](/docs/user-guide/components/reference/shape/)。

## 提供者

[O3DE Core (LmbrCentral) Gem](/docs/user-guide/gems/reference/o3de-core)

## Quad Shape 属性

![Quad Shape component properties](/images/user-guide/components/reference/shape/quad-shape-component-ui-01.png)

| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **Visible** | 启用此选项可始终在视区中显示形状，即使未选择实体也是如此。禁用可在未选择实体时隐藏形状。 | Boolean | `Enabled` |
| **Game View** | 启用此选项可在游戏模式下显示形状。 | Boolean | `Disabled` |
| **Shape Color** | 形状的颜色。 | 每通道 8 位颜色： 0-255 | `255,255,199` |
| **Width** | 形状在局部 X 轴上的宽度（以米为单位）。 | 0.0 to Infinity | `1.0` |
| **Height** | 形状在局部 Y 轴上的高度（以米为单位）。 | 0.0 to Infinity | `1.0` |

## QuadShapeComponentRequestsBus

将以下请求函数与 '`QuadShapeComponentRequestsBus`' 事件总线接口结合使用，以便与游戏中的四边形组件进行通信。

| 方法名称 | 说明 | 参数 | 返回值 | 脚本化 |
|-|-|-|-|-|
| `GetQuadConfiguration` | 返回四边形的配置。 | None | '`LmbrCentral_QuadShapeConfig`' 对象，其中包含属性 '`Height`' 和 '`Width`'。 | Yes |
|`GetQuadHeight`| 返回四边形的 **Height**。 | None | Height: Float | Yes |
|`GetQuadOrientation`| 返回四边形的方向。 | None | Orientation: Quaternion | Yes |
|`GetQuadWidth`| 返回四边形的 **Width**。 | None | Width: Float | Yes |
|`SetQuadHeight`| 设置四边形的 **Height**。 | Height: Float | None | Yes |
|`SetQuadWidth`| 设置四边形的 **Width**。 | Width: Float | None | Yes |

请参阅 [Shape 组件 Ebus 接口](./#shape-component-ebus-interface) 以获取所有 Shape 组件可用的功能说明。
