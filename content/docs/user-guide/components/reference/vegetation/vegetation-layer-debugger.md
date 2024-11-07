---
linkTitle: Vegetation Layer Debugger
title: Vegetation Layer Debugger 组件
description: 使用 Open 3D Engine （O3DE） 中的 Vegetation Layer Debugger 组件查看如何在关卡中应用植被图层。
weight: 450
---

使用 **Vegetation Layer Debugger** 组件查看如何在关卡中应用植被层。向每个植被层添加一个调试器，以可视化 **Layer Priorities**、**Sub Priorities** 和植被层混合的最终组成。

## 提供者

[Vegetation Gem](/docs/user-guide/gems/reference/environment/vegetation/)

## Vegetation Layer Debugger 属性

![Vegetation Layer Debugger component properties](/images/user-guide/components/reference/vegetation/vegetation-layer-debugger-component.png)

| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **Debug Visualization Color** | 设置此层的调试可视化效果的颜色。 | 每通道 8 位颜色： 0-255 | Random Color |
| **Debug Visualization Cube Size** | 将调试可视化的大小设置为植被层 **Preview Size**的百分比。 | Float: 0.0 to Infinity | `0.25` |
| **Hide created instance in the Debug Visualization** | 如果为“`Enabled`”，则隐藏调试可视化立方体中的植被实例。 | Boolean | `Disabled` |
