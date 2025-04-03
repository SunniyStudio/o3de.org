---
title: Axis Aligned Box Shape 组件
linktitle: Axis Aligned Box Shape
description: ' Open 3D Engine (O3DE) Box Shape 组件参考。'
weight: 100
---

**Axis Aligned Box Shape** 组件可创建一个始终轴对齐的透明框。可以使用 **Dimensions** 属性或通过进入编辑模式来编辑框的尺寸。Axis Aligned Box Shape （轴对齐的盒形） 组件不是一个网格，而是一个辅助几何体，可用于定义地形、雾、生成器、形状渐变、音频、植被、PhysX 以及任何可利用 Shape EBus 的应用程序的体积。有关使用 Shape 组件的更多信息，请参阅 [Shape 组件](/docs/user-guide/components/reference/shape/)。

## 提供方

[O3DE Core (LmbrCentral) Gem](/docs/user-guide/gems/reference/o3de-core)

## Axis Aligned Box Shape 属性

![Axis Aligned Box Shape component properties](/images/user-guide/components/reference/shape/axis-aligned-box-shape-component-ui-01.png)

| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **Visible** | 启用此选项可始终在视区中显示形状，即使未选择实体也是如此。禁用可在未选择实体时隐藏形状。 | Boolean | `Enabled` |
| **Game View** | 启用此选项可在游戏模式下显示形状。 | Boolean | `Disabled` |
| **Filled** | 启用此选项可将形状显示为已填充。 禁用以将形状显示为线框。 | Boolean | `Enabled` |
| **Shape Color** | 形状的颜色。 | 每通道 8 位颜色： 0-255 | `255,255,199` |
| **Dimensions** | 形状在局部空间的 X、Y 和 Z 维度中的大小。 | Vector3: -Infinity to Infinity | X:`1.0`, Y:`1.0`, Z:`1.0` |
| **Translation Offset** | 形状相对于其实体的平移偏移。 | Vector3: -Infinity to Infinity | X:`0.0`, Y:`0.0`, Z:`0.0` |
| **Edit** | 选择 **Edit** 按钮进入 Edit 模式。在编辑模式下，您可以使用下面的 [编辑模式操作] (#edit-mode-actions)中概述的方法在视区中修改形状的尺寸。 在 Edit 模式下，菜单栏中的 Edit （编辑） 菜单显示可用的操作和热键。要退出 Edit 模式，请在组件界面中选择 **Done**。 |  |  |

## 编辑模式操作

编辑模式提供两个子模式。您可以使用 **编辑器** 左上角的 **Viewport UI Cluster** 中的按钮、使用下面列出的键盘热键或使用 **Ctrl + Mousewheel Up/Down** 在子模式之间切换。

| 模式 | 图标 | 快捷键 | 说明 |
| - | - | - | - |
| **Dimensions** | ![Shape component mode dimensions submode icon](/images/user-guide/components/reference/shape/shape-component-mode-submode-dimensions.svg) | **1** | **左键单击**并拖动 Axis Aligned Box Shape 两侧的黑色手柄，以调整矩形在其局部 X、Y 和 Z 维度上的大小。默认情况下，可以单独编辑长方体的每个面。按住 **Shift** 可对称地编辑相对的面对。 |
| **Translation Offset** | ![Shape component mode translation offset submode icon](/images/user-guide/components/reference/shape/shape-component-mode-submode-translation-offset.svg) | **2** | 单击鼠标左键并拖动线性或平面操纵器以编辑 **平移偏移**。 |
| **Reset Current Mode** | | **R** | 将当前子模式操作的属性重置为其默认值。 | 

## BoxShapeComponentRequestsBus

将以下请求函数与 '`BoxShapeComponentRequestsBus`' 事件总线接口结合使用，以便与游戏中的 Axis Aligned Box Shape 组件进行通信。

| 方法名称 | 说明 | 参数 | 返回值 | 脚本化 |
|-|-|-|-|-|
| `GetBoxConfiguration` | 返回框形状的配置。 | None | 包含属性 '`Dimensions`' 的 '`BoxShapeConfiguration`' 对象。 | Yes |
|`GetBoxDimensions`| 返回框形状的 **Dimensions**。 | None | Dimensions: Vector3 | Yes |
|`SetBoxDimensions`| 设置框形状的 **Dimensions**。 | Dimensions: Vector3 | None | Yes |


请参阅 [Shape 组件 Ebus接口](./#shape-component-ebus-interface) 以获取所有 Shape 组件可用的功能说明。
