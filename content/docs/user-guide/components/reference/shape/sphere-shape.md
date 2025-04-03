---
title: Sphere Shape 组件
linktitle: Sphere Shape
description: ' Open 3D Engine (O3DE) Sphere Shape组件参考。 '
weight: 100
---



**Sphere Shape** 组件可创建一个透明球体。球体的尺寸可以使用 **Radius** 属性进行编辑。Sphere Shape （球体形状） 组件不是网格，而是一个辅助几何体，可用于定义区域光、形状渐变、音频、植被、PhysX 以及任何可利用 Shape EBus 的应用程序的体积。有关使用 Shape 组件的更多信息，请参阅 [Shape 组件](/docs/user-guide/components/reference/shape/)。

## 提供者

[O3DE Core (LmbrCentral) Gem](/docs/user-guide/gems/reference/o3de-core)

## Sphere Shape 属性

![Sphere Shape component properties](/images/user-guide/components/reference/shape/sphere-shape-component-ui-01.png)

| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **Visible** | 启用此选项可始终在视区中显示形状，即使未选择实体也是如此。禁用可在未选择实体时隐藏形状。 | Boolean | `Enabled` |
| **Game View** | 启用此选项可在游戏模式下显示形状。 | Boolean | `Disabled` |
| **Filled** | 启用此选项可将形状显示为已填充。 禁用以将形状显示为线框。 | Boolean | `Enabled` |
| **Shape Color** | 形状的颜色。 | 每通道 8 位颜色： 0-255 | `255,255,199` |
| **Radius** | 形状的半径（以米为单位）。 | 0.0 to Infinity | `0.5` |
| **Translation Offset** | 形状相对于其实体的平移偏移。 | Vector3: -Infinity to Infinity | X:`0.0`, Y:`0.0`, Z:`0.0` |
| **Edit** | 选择 **Edit** 按钮进入 Edit 模式。在编辑模式下，您可以使用下面的 [编辑模式操作](#edit-mode-actions) 中概述的方法在视区中修改形状的尺寸。在 Edit 模式下，菜单栏中的 Edit （编辑） 菜单显示可用的操作和热键。要退出 Edit 模式，请在组件界面中选择 **Done**。 |  |  |

## 编辑模式操作

编辑模式提供两个子模式。您可以使用 **编辑器** 左上角的 **Viewport UI Cluster** 中的按钮、使用下面列出的键盘热键或使用 **Ctrl + Mousewheel Up/Down** 在子模式之间切换。

| 模式 | 图标 | 快捷键 | 说明 |
| - | - | - | - |
| **Dimensions** | ![Shape component mode dimensions submode icon](/images/user-guide/components/reference/shape/shape-component-mode-submode-dimensions.svg) | **1** | **左键单击**并拖动 Sphere Shape （球体形状） 表面上的黑色手柄以编辑 **Radius**（半径）。 |
| **Translation Offset** | ![Shape component mode translation offset submode icon](/images/user-guide/components/reference/shape/shape-component-mode-submode-translation-offset.svg) | **2** | 单击鼠标左键并拖动线性或平面操纵器以编辑 **Translation Offset**。 |
| **Reset Current Mode** | | **R** | 将当前子模式操作的属性重置为其默认值。 | 

## SphereShapeComponentRequestsBus

将以下请求函数与 '`SphereShapeComponentRequestsBus`' 事件总线接口结合使用，以便与游戏中的球体形状组件进行通信。

| 方法名称 | 说明 | 参数 | 返回值 | 脚本化 |
|-|-|-|-|-|
| `GetSphereConfiguration` | 返回球体形状的配置。 | None | `SphereShapeConfig` 包含属性 '`Radius`' 的对象。 | Yes |
| `SetRadius` | 设置球体形状的 **Radius**。 | Radius: Float | None | Yes |

请参阅 [Shape 组件 Ebus 接口](./#shape-component-ebus-interface) 以获取所有 Shape 组件可用的功能说明。
