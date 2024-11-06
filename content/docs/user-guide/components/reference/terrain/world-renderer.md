---
linktitle: Terrain World Renderer
title: Terrain World Renderer 组件
description: Terrain World Renderer （地形世界渲染器） 组件渲染世界中的地形。
toc: true
---

 **Terrain World Renderer** 组件可视化世界中的地形并控制其各种全局属性。

{{< important >}}
您必须将此组件添加到 *Level* 实体，该实体是 **Open 3D Engine （O3DE）** 关卡中所有实体的父级。
{{< /important >}}

## 提供者

[Terrain Gem](/docs/user-guide/gems/reference/environment/terrain)

## 依赖

[Terrain World](/docs/user-guide/components/reference/terrain/world)

## 属性

![Terrain World Renderer component interface.](/images/user-guide/components/reference/terrain/terrain-world-renderer-A.png)

### Mesh Configuration 属性

| 属性 | 说明 | 值 | 默认值 |
| - | - | - | - |
| **Mesh render distance** | 距地形网格渲染到的摄像机的距离（以米为单位）。 | 1.0 - 100000.0 | `4096.0` |
| **First LOD distance** | 与第一个 LOD 渲染到的摄像机的距离（以米为单位）。后续 LOD 位于距前一个 LOD 的距离的两倍处。 | 1.0 - 10000.0 | `128.0` |
| **Continuous LOD (CLOD)** | 如果启用，则激活使用连续细节级别，从而平滑地混合地形 LOD 之间的几何体。 | Boolean | `Enabled` |
| **CLOD Distance** | 第一个 LOD 混合到下一个 LOD 的距离（以米为单位）。后续 LOD 的混合距离是前一个 LOD 的两倍。这将创建一致的视觉外观。 <br><br>  我们建议此值小于 **First LOD distance** 的 25% 左右。如果设置得太低，则地形中可能会出现接缝。 | 0.0 - 1000.0 | `16.0` |

### Detail Configuration

| 属性 | 说明 | 值 | 默认值 |
| - | - | - | - |
| **Height based texture blending** | 如果启用，细节材质将使用高度纹理进行混合。 | Boolean | `Disabled` |
| **Detail material render distance** | 与细节材质渲染到的摄像机的距离（以米为单位）。 | 1.0 - 2048.0 | `512.0` |
| **Detail material fade distance** | 细节材质淡出到宏材质中的距离（以米为单位）。 | 0.0 - 2048.0 | `64.0` |
| **Detail material scale** | 所有细节材质渲染的比例。 | 0.0001 - 10000.0 | `1.0` |

### Clipmap Configuration

| 属性 | 说明 | 值 | 默认值 |
| - | - | - | - |
| **Clipmap Enabled** | 如果启用，则使用裁剪图渲染地形材质，而不是每帧直接渲染材质。| Boolean | `Disabled` |
| **Clipmap image size** | 每个图层中 clipmap 图像的大小。 | `512`, `1024`, or `2048` | `1024` |
| **Macro clipmap max resolution: texels/m** | 堆栈中最高分辨率裁剪图的分辨率。 | 0.1 - 100.0 | `2.0` |
| **Detail clipmap max resolution: texels/m** | 堆栈中最高分辨率裁剪图的分辨率。 | 10.0 - 4096.0 | `2048.0`
| **Macro clipmap scale base** | 宏材质的两个相邻剪贴图图层之间的比例基。例如，3 表示第(n+1)<sup>th</sup> 裁剪图覆盖了n<sup>th</sup>裁剪图覆盖的区域的 3<sup>2</sup> = 9 倍。 | 1.1 - 10.0 | `2.0` |
| **Detail clipmap scale base** | 宏材质的两个相邻剪贴图图层之间的比例基。例如，3 表示第(n+1)<sup>th</sup>个裁剪图覆盖了n<sup>th</sup>裁剪图覆盖的区域的 3<sup>2</sup> = 9 倍。 | 1.1 - 10.0 | `2.0` |
| **Macro clipmap margin size: texels** | 裁剪图超出可见数据的边距。增加边距会导致裁剪图更新频率降低，但在更靠近摄像机的渲染时，也会导致裁剪图的分辨率较低。 | 1 - 16 | `4` |
| **Detail clipmap margin size: texels** | 裁剪图超出可见数据的边距。增加边距会导致裁剪图更新频率降低，但在更靠近摄像机的渲染时，也会导致裁剪图的分辨率较低。 | 1 - 16 | `4` |

## 用法

Mesh Configuration 属性控制网格密度和地形 LOD 的混合（细节级别）。它们对顶点性能有非常直接的影响。
第一个地形 LOD 的密度由 [Terrain World](/docs/user-guide/components/reference/terrain/world) 组件中的查询分辨率设置。**First LOD distance** 控制在此密度下渲染的距离。

每个后续 LOD 以一半的分辨率渲染两倍的距离，直到满足 **Mesh render distance**。这意味着 **First LOD distance** 对三角形总数的影响大于对总渲染距离的影响。

细节配置属性会影响 [地形细节材质](/docs/user-guide/components/reference/terrain/terrain-detail-material) 的渲染距离、淡出方式以及它们之间的相互混合方式。这些设置中的大多数对性能影响很小。但是，渲染距离越大，当您在世界中移动时，需要查询的表面数据点就越多，并且还会使用更多的内存。

_Clipmaps_ 是以视图为中心的纹理堆栈，取代了地形细节和宏材质的直接使用。昂贵的细节材质混合操作只完成一次并保存到剪辑图中，而不是针对每帧上的每个像素完成。它们覆盖不同的距离，并且都具有相同的分辨率。这允许视图附近的区域具有最多的纹理细节。使用 clipmap 可以提高性能，但会消耗纹理内存。您可以调整 clipmap 配置，以在性能增益和内存成本之间找到平衡。
