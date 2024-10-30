---
title: Grid 组件
linktitle: Grid
description: 'Open 3D Engine (O3DE) Grid组件参考。'
toc: true
---

**Grid** 组件渲染线框网格。它主要用于场景构建工作流，以设定参考框架，并直观显示物体之间的相对大小和距离。

## 提供方 ##

[Atom Gem](/docs/user-guide/gems/reference/rendering/atom/atom/)


## Grid 组件属性
Grid 组件具有简单的属性，可以改变其大小和外观。

![grid-component-base-properties](/images/user-guide/components/reference/atom/grid/grid-base-properties-ui.png)

| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **Grid Size** | 网格总面积（米）。 | 0.0 - 1,000,000 | 32.0 |
| **Axis Color** | 用于绘制主轴网格线的颜色。 | (0-255, 0-255, 0-255) | (0, 0, 255) |
| **Primary Grid Spacing** | 用主色调绘制的网格线之间的距离。 | 0.1 to Infinity | 1.0 |
| **Primary Color** | 用于绘制主网格线的颜色。 | (0-255, 0-255, 0-255) | (64, 64, 64) |
| **Secondary Grid Spacing** | 用辅助颜色绘制的网格线之间的距离。 | 0.1 to Infinity | 0.25 |
| **Secondary Color** | 用于绘制辅助网格线的颜色。 | (0-255, 0-255, 0-255) | (128,128,128) |

## GridComponentRequestBus

| 方法名称 | 说明 | 参数 | 返回值 | 可脚本化 |
|-|-|-|-|-|
| 'SetSize' | 设置整个网格的大小。 | Size: Float | None | Yes |
| 'SetPrimarySpacing' | 设置主网格线之间的间距。 | Spacing: Float | None | Yes |
| 'SetSecondarySpacing' | 设置辅助网格线之间的间距。 | Spacing: Float | None | Yes |
| 'SetAxisColor' | 设置网格轴线的颜色。 | Line Color: Color | None | Yes |
| 'SetPrimaryColor' | 设置主网格线的颜色。 | Line Color: Color | None | Yes |
| 'SetSecondaryColor' | 设置辅助网格线的颜色。 | Line Color: Color | None | Yes |
| 'GetSize' | 获取整个网格的大小。 | None | Size: Float | Yes |
| 'GetPrimarySpacing' | 获取主网格线之间的间距。 | None | Spacing: Float | Yes |
| 'GetSecondarySpacing' | 获取辅助网格线之间的间距。 | None | Spacing: Float | Yes |
| 'GetAxisColor' | 获取网格轴线的颜色。 | None | Line Color: Color | Yes |
| 'GetPrimaryColor' | 获取主网格线的颜色。 | None | Line Color: Color | Yes |
| 'GetSecondaryColor' | 获取辅助网格线的颜色。 | None | Line Color: Color | Yes |

## GridComponentNotificationBus

| 方法名称 | 说明 | 参数 | 返回值 | 可脚本化 |
|-|-|-|-|-|
| 'OnGridChanged' | 通知任何处理程序网格已被修改。 | None | None | No |
