---
linktitle: Terrain World Debugger
title: Terrain World Debugger 组件
description: Terrain World Debugger 组件提供了一种显示 Terrain World 的线框或边界表示的方法。
toc: true
---

**Terrain World Debugger**组件提供了许多地形调试功能。这些可视化效果完全是可选的，可以单独打开或关闭。

{{< important >}}
您必须将此组件添加到 *Level* 实体，该实体是 **Open 3D Engine （O3DE）** 关卡中所有实体的父级。
{{< /important >}}

## 提供者

[Terrain Gem](/docs/user-guide/gems/reference/environment/terrain)

## 依赖

[Terrain World](/docs/user-guide/components/reference/terrain/world)

## 属性

![Terrain World Debugger component interface.](/images/user-guide/components/reference/terrain/terrain-world-debugger.png)

| 属性 | 说明 | 值 | 默认值 |
| - | - | - | - |
| Show Wireframe | 显示 Terrain World 的线框表示。 | Boolean | `Enabled` |
| Show World Bounds | 显示 Terrain World 边界。 | Boolean | `Enabled` |
| Show Dirty Region | 显示地形的最新脏区。 | Boolean | `Disabled` |
| Show Terrain Queries | 显示 terrain 查询可视化效果。 | Boolean | `Disabled` |

### Terrain Queries Configuration

| 属性 | 说明 | 值 | 默认值 |
| - | - | - | - |
| **Sampler** | 用于查询 terrain 值的查询采样器的类型。 | `Exact`, `Clamp`, 或 `Bilinear` | `Bilinear` |
| **Point count** | 要可视化的每个方向上的点数。 | 1.0 - 64.0 | `32.0` |
| **Spacing (m)** | 确定查询结果应相距多远（以米为单位）。 | 0.001 - 10000.0 | `0.5` |
| **Draw Heights** | 启用 terrain 高度查询的可视化。 | Boolean | `Enabled` |
| **Height Point Size (m)** | 确定高度点的大小（以米为单位）。 | 0.0 - 10000.0 | `0.0625` (`1.0f / 16.0f`) |
| **Draw Normals** | 启用 terrain normal 查询的可视化。 | Boolean | `Enabled` |
| **Normal Height (m)** | 确定法线的高度（以米为单位）。 | 0.0 - 10000.0 | `1.0` |
| **Use Camera Position** | 确定是使用当前摄像机位置还是指定位置。 | Boolean | `Enabled` |
| **World Position** | 要在其中绘制查询结果的区域的中心。 | Vector3: -Infinity to Infinity | X:`0.0`, Y:`0.0`, Z:`0.0` |
