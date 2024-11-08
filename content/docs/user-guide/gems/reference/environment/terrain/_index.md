---
linkTitle: Terrain
title: Terrain Gem
description: Open 3D Engine （O3DE） Terrain Gem 简介。
toc: true
---


Terrain Gem 实现了一个 *terrain system*，该系统定义地表的几何体、颜色和表面类型，渲染表面，并提供物理表示。

## 特征

Terrain Gem 具有以下主要功能：

* 将高度、颜色和表面数据映射到世界各个区域。
* 提供基于渐变和基于形状的创作工具和工作流，用于创建和操作地形数据。
* 公开可由模拟和渲染使用的可查询 API。
* 在整个视图距离上渲染高效、高质量的地形可视化。
* 与物理集成，以提供虚拟世界中地形的“物理”模拟。

## 启用 Terrain Gem

要启用 Terrain Gem，请执行以下操作：

1. 使用 **Project Manager** 或命令行将 Terrain Gem 添加到您的项目中。
2. 使用 Project Manager、Visual Studio 或 CMake 构建您的项目。

## 入门

有关创作地形的分步说明，请参阅 [从图像创建地形](/docs/learning-guide/tutorials/environments/create-terrain-from-images)教程。
请参阅 [地形开发人员指南](/docs/user-guide/visualization/environments/terrain/terrain-developer-guide) ，了解有关开发人员使用和扩展地形系统的信息。

## 关卡组件

|组件 |描述 |
| - | - |
| [Terrain World](/docs/user-guide/components/reference/terrain/world) | 启用 terrain 系统并提供其他 terrain 组件所需的数据。 |
| [Terrain World Debugger](/docs/user-guide/components/reference/terrain/world-debugger) | 提供许多地形调试功能。这些可视化效果完全是可选的，可以单独打开或关闭。 |
| [Terrain World Renderer](/docs/user-guide/components/reference/terrain/world-renderer) | 可视化世界中的地形并控制全局地形渲染属性。 |

## 组件

|组件 |描述 |
| - | - |
| [Terrain Layer Spawner](/docs/user-guide/components/reference/terrain/layer_spawner) | 生成包含在可配置边界内的地形区域，并允许对重叠的地形图层进行优先级排序。 |
| [Terrain Height Gradient List](/docs/user-guide/components/reference/terrain/height_gradient_list) | 提供来自渐变列表的地形高度数据。 |
| [Terrain Surface Gradient List](/docs/user-guide/components/reference/terrain/surface-gradient-list) | 定义地形图层上渐变和表面类型之间的映射。 |
| [Terrain Macro Material](/docs/user-guide/components/reference/terrain/terrain-macro-material) | 提供定义 terrain 区域外观的宏观级别方法。 |
| [Terrain Surface Materials List](/docs/user-guide/components/reference/terrain/surface-material-list) | 定义曲面类型和渲染材质之间的映射。 |
| [Terrain Physics Heightfield Collider](/docs/user-guide/components/reference/terrain/terrain-physics-collider) | 以高度场和表面到材质映射的形式向物理碰撞体提供地形数据。 |

## CVARs

Terrain Gem 在运行时通过控制台使用以下控制台变量 （CVAR），或者将它们放置在配置中。有关配置 CVAR 的更多信息，请参阅通用 [CVAR 指南](/docs/user-guide/appendix/cvars/)。

### 地形物理

|姓名 |描述 |
| - | - |
| `cl_terrainPhysicsColliderMaxJobs` |更新 Terrain Physics Collider （地形物理碰撞体） 时要使用的最大作业数（'`-1`' 将使用所有可用内核）。 |
| `physx_heightfieldDebugDrawDistance` | PhysX Heightfield 调试可视化的距离（以米为单位）。 |
| `physx_heightfieldDebugDrawBoundingBox` | 绘制用于高度场调试可视化的边界框。 |
| `physx_heightfieldColliderUpdateRegionSize` | 高度场碰撞体更新区域的大小（以米为单位），用于对更新进行分区以更快地取消。 |

### 地形渲染

|姓名 |描述 |
| - | - |
| `r_terrainClipmapDebugEnable` | 在屏幕上打开 clipmap 调试渲染。 |
| `r_terrainClipmapDebugOverlay` | 要在屏幕上渲染的剪辑图类型。'`0`' 表示无，'`1`' 是宏纹理裁剪图，'`2`' 是细节纹理裁剪图。 |
| `r_terrainClipmapDebugClipmapId` | 要在屏幕上渲染的 clipmap 通道。“`0`”是宏颜色，“`1`”是宏法线，“`2`”是细节颜色，“`3`”是细节法线，“`4`”是细节高度，“`5`”是细节粗糙度，“`6`”是细节镜面反射，“`7`”是细节金属度，“`8`”是细节遮挡。 |
| `r_terrainClipmapDebugClipmapLevel` | 要在屏幕上渲染的剪辑图级别。|
| `r_terrainClipmapDebugScale` | 剪辑贴图纹理的调试显示的大小乘数。 |
| `r_terrainClipmapDebugBrightness` | 剪辑贴图纹理的调试显示的最终输出的乘数。 |
| `r_terrainDebugDetailMaterials` | 启用细节材质 ID 可视化。 |
| `r_terrainDebugDetailImageUpdates` | 启用细节材质更新区域的可视化。 |
| `r_debugTerrainLodLevels` | 为地形网格 LOD 启用调试着色。 |
| `r_debugTerrainAabbs` | 启用 terrain 扇区 AABB 的可视化。 |
