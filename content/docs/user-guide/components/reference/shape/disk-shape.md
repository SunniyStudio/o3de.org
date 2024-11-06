---
title: Disk Shape 组件
linktitle: Disk Shape
description: ' Open 3D Engine (O3DE) Disk Shape 组件参考。 '
weight: 100
---



**Disk Shape** 组件创建一个以局部 Z 轴为导向的透明圆形表面。可以使用 **Radius** 属性编辑磁盘的维度。Disk Shape （圆盘形状） 组件不是网格，而是一个辅助几何体，可用于定义区域光、生成器、形状渐变、音频、植被、PhysX 以及任何可利用形状 EBus 的应用程序的区域。有关使用 Shape 组件的更多信息，请参阅 [Shape 组件](/docs/user-guide/components/reference/shape/)。

## 提供者

[O3DE Core (LmbrCentral) Gem](/docs/user-guide/gems/reference/o3de-core)

## Disk Shape 属性

![Disk Shape component properties](/images/user-guide/components/reference/shape/disk-shape-component-ui-01.png)

| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **Visible** | 启用此选项可始终在视区中显示形状，即使未选择实体也是如此。禁用可在未选择实体时隐藏形状。 | Boolean | `Enabled` |
| **Game View** | 启用此选项可在游戏模式下显示形状。 | Boolean | `Disabled` |
| **Filled** | 启用此选项可将形状显示为已填充。 禁用以将形状显示为线框。 | Boolean | `Enabled` |
| **Shape Color** | 形状的颜色。 | 每通道颜色 8 位: 0-255 | `255,255,199` |
| **Radius** | 形状的半径（以米为单位）。 | 0.0 to Infinity | `0.5` |

## DiskShapeComponentRequestsBus

将以下请求函数与 '`DiskShapeComponentRequestsBus`' 事件总线接口结合使用，以便与游戏中的磁盘形状组件进行通信。

| 方法名称 | 说明 | 参数 | 返回值 | 脚本化 |
|-|-|-|-|-|
| `GetDiskConfiguration` | 返回磁盘形状的配置。 | None | `DiskShapeConfig` 包含属性 '`Radius`' 的对象。 | Yes |
|`GetRadius`| 返回圆盘形状的 **Radius**。 | None | Radius: Float | Yes |
|`SetRadius`| 设置圆盘形状的 **Radius**。 | Radius: Float | None | Yes |

请参阅 [Shape 组件 Ebus 接口](./#shape-component-ebus-interface) 以获取所有 Shape 组件可用的功能说明。
