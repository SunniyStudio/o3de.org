---
title: Capsule Shape 组件
linktitle: Capsule Shape
description: ' Open 3D Engine (O3DE) Capsule Shape 组件参考。 '
weight: 100
---



**Capsule Shape** 组件创建一个以局部 Z 轴为导向的透明胶囊。胶囊的尺寸可以使用 **Height** 和 **Radius** 属性进行编辑。Capsule Shape （胶囊体形状） 组件不是网格，而是一个辅助几何体，可用于定义区域光、形状渐变、音频、植被、PhysX 以及任何可利用 Shape EBus （形状事件总线） 的应用程序的体积。有关使用形状组件的更多信息，请参阅[Shape 组件](/docs/user-guide/components/reference/shape/)。

## 提供者

[O3DE Core (LmbrCentral) Gem](/docs/user-guide/gems/reference/o3de-core)

## Capsule Shape 属性

![Capsule Shape component properties](/images/user-guide/components/reference/shape/capsule-shape-component-ui-01.png)

| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **Visible** | 启用此选项可始终在视区中显示形状，即使未选择实体也是如此。禁用可在未选择实体时隐藏形状。 | Boolean | `Enabled` |
| **Game View** | 启用此选项可在游戏模式下显示形状。 | Boolean | `Disabled` |
| **Filled** | 启用此选项可将形状显示为已填充。 禁用以将形状显示为线框。 | Boolean | `Enabled` |
| **Shape Color** | 形状的颜色。 | 每通道 8 位颜色： 0-255 | `255,255,199` |
| **Height** | 形状的高度（以米为单位）。height 值必须至少是 **Radius** 值的两倍。| 0.0 to Infinity | `1.0` |
| **Radius** | 形状的半径（以米为单位）。radius 值不能大于 **Height** 值的一半。 | 0.0 to Infinity | `0.25` |
| **Translation Offset** | 形状相对于其实体的平移偏移。 | Vector3: -Infinity to Infinity | X:`0.0`, Y:`0.0`, Z:`0.0` |
| **Edit** | 选择 **Edit** 按钮进入 Edit 模式。在编辑模式下，您可以使用下面的 [编辑模式操作](#edit-mode-actions) 中概述的方法在视区中修改形状的尺寸。在 Edit 模式下，菜单栏中的 Edit （编辑） 菜单显示可用的操作和热键。要退出 Edit 模式，请在组件界面中选择 **Done**。 |  |  |

## 编辑模式操作

编辑模式提供两个子模式。您可以使用 **编辑器** 左上角的 **Viewport UI Cluster** 中的按钮、使用下面列出的键盘热键或使用 **Ctrl + Mousewheel Up/Down** 在子模式之间切换。

| 模式 | 图标 | 键盘快捷键 | 说明 |
| - | - | - | - |
| **Dimensions** | ![Shape component mode dimensions submode icon](/images/user-guide/components/reference/shape/shape-component-mode-submode-dimensions.svg) | **1** | 单击鼠标左键并拖动胶囊形状末端的黑色手柄以编辑 **Height**。默认情况下，胶囊的每一端都可以单独编辑。按住 **Shift** 可对称地编辑两端。单击鼠标左键并拖动位于胶囊体形状中间的黑色手柄以编辑 **Radius**。 |
| **Translation Offset** | ![Shape component mode translation offset submode icon](/images/user-guide/components/reference/shape/shape-component-mode-submode-translation-offset.svg) | **2** | 单击鼠标左键并拖动线性或平面操纵器以编辑 **Translation Offset**。 |
| **Reset Current Mode** | | **R** | 将当前子模式操作的属性重置为其默认值。 | 

## CapsuleShapeComponentRequestsBus

将以下请求函数与 '`CapsuleShapeComponentRequestsBus`' 事件总线接口结合使用，以便与游戏中的胶囊体形状组件进行通信。
2
| 方法名称 | 说明 | 参数 | 返回值 | 脚本化 |
|-|-|-|-|-|
| `GetCapsuleConfiguration` | 返回胶囊形状的配置。 | None | 包含属性 '`Height`' 和 '`Radius`' 的 '`CapsuleShapeConfig`' 对象。 | Yes |
| `SetHeight` | 设置胶囊形状的 **Height**。 | Height: Float | None | Yes |
| `SetRadius` | 设置胶囊体形状的 **Radius**。 | Radius: Float | None | Yes |

请参阅 [Shape 组件 Ebus 接口](./#shape-component-ebus-interface) 以获取所有 Shape 组件可用的功能说明。
