---
linktitle: Terrain World
title: Terrain World 组件
description: Terrain World 组件允许设置世界边界和高度查询分辨率。
toc: true
---

**Terrain World**组件 提供其他地形系统组件所需的数据。此组件为关卡启用地形系统。

{{< important >}}
您必须将此组件添加到 *Level* 实体，该实体是 **Open 3D Engine （O3DE）** 关卡中所有实体的父级。
{{< /important >}}

## 提供者 ##

[Terrain Gem](/docs/user-guide/gems/reference/environment/terrain)

## Terrain World 属性 ##

![Terrain World component interface.](/images/user-guide/components/reference/terrain/terrain-world-A.png)

| 属性 | 说明 | 值 | 默认值 |
| - | - | - | - |
| Min Height | terrain 高度的最小值。**Min Height**值必须小于**Max Height**值。<br><br>任何高度低于此高度的地形图层生成器都会将其最小高度裁剪为 **Min Height** 值。 | `-65536.0` - `65536.0` | `0.0` |
| Max Height | terrain 高度的最大值。**Max Height**值必须大于**Min Height**值。<br><br>任何高度高于此高度的地形图层生成器都会将其最大高度裁剪为 **Max Height** 值。 | `-65536.0` - `65536.0` | `1024.0` |
| Height Query Resolution (m) | 每个高度采样位置之间的距离，以米为单位。 | `0.1` - `Infinity` | `1.0` |
| Surface Data Query Resolution (m) | 每个表面数据采样位置之间的距离，以米为单位。 | `0.1` - `Infinity` | `1.0` |

## 用法 

我们建议您将 **Min Height** 和 **Max Height** 边界设置为尽可能小的范围，以获得更准确的高度值。地形渲染器使用 16 位高度值，该值提供 65536 个可能的不同高度。要获得分辨率，请计算`(<Max Height> - <Min Height>) / 65536`。因此，如果高度范围为 1 公里，则分辨率约为 1.5 厘米。这将产生更精细的高度细节。

**Height Query Resolution** 和 **Surface Data Query Resolution** 用于为整个世界的地形高度和表面查询定义一致的间距，以原点为中心。使用地形数据的不同系统（例如物理和渲染）也可以使用这些概念网格来做出有关如何最好地使用地形数据的实施决策。默认情况下，terrain 系统仅在与网格对齐的点处查询输入数据。这将创建输入数据的一致视图。但是，您可以以任何分辨率编写或生成 terrain 系统的输入数据。由于输入数据仅在这些分辨率下使用，因此我们建议具有量化源数据（如图像）的输入梯度与适当的查询分辨率匹配。

例如，如果地形使用高度贴图图像，其高度为 10 个纹素/米，查询分辨率为 1 米，则 10 个纹素中有 9 个将被忽略。如果高度图图像具有 1 个纹素/米，高度查询分辨率为 1 米，则使用所有纹素。

对于物理特性，**Height Query Resolution**控制底层碰撞器的高度场四边形间距。对于渲染，这决定了最接近细节层次的三角形密度。

**Surface Data Query Resolution** 会影响用于细节材质混合的细节材质权重数据的密度。
