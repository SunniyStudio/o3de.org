---
linkTitle: 移植地形
title: 移植地形
description: 了解如何从 Lumberyard 导出地形数据和材质，以及如何在 O3DE 中使用它们
weight: 500
toc: true
---

本教程将介绍如何将地形数据从 Lumberyard 导出到一组纹理，以及如何将这些纹理与 [O3DE 地形系统](docs/learning-guide/tutorials/environments/create-terrain-from-images/)结合使用，以创建具有相同地形和整体外观的地形。

|O3DE 体验 |完成时间 |功能聚焦 |最后更新 |
| - | - | - | - |
| 初级 |20 分钟 |将地形数据从 Lumberyard 移植到 O3DE | February 27, 2024 |

## 从 Lumberyard 导出高度贴图和展开贴图

使用 Lumberyard 打开 [Starter Game project](get-starter-game-project) 项目，打开主地图，然后通过顶部菜单打开 **“Tools/Terrain Editor”** （步骤 *1.* 和 *2.*）。

![Lumberyard Terrain editor](/images/learning-guide/tutorials/lumberyard-to-o3de/terrain-editor.png)

通过 Terrain Editor File（地形编辑器的 File）菜单（步骤 *3.*）您将能够 **导出这个地形的高度贴图**。您可以将其命名为 `heightmap_gsi.tif`并将其放置在您的 O3DE 项目或 Gem 中 （`_gsi`对于 O3DE 知道如何导入此纹理非常重要）。

然后，您可以通过 Tools 菜单（步骤 *4.*）打开 Terrain Texture Layers（地形纹理图层），然后单击每个图层的 **“Export Splat Map”（导出 Splat Map）**。您可以根据需要命名它们，并将其导入到 O3DE 项目中。

![Splat map](/images/learning-guide/tutorials/lumberyard-to-o3de/splat-map.png)

完成后，请不要忘记从任务栏关闭 Lumberyard Asset Processor。

## 在 O3DE 中创建地形

您需要在项目中启用地形 Gem 和表面数据 Gem（按照 [本文档](/docs/user-guide/project-config/add-remove-gems/)了解如何启用 Gem），然后打开 SinglePlayer 关卡。

在左侧的 Entity Outliner 中，选择最顶部的 SinglePlayer Prefab。添加 [**Terrain World**](/docs/user-guide/components/reference/terrain/world) 和 [**Terrain World Renderer**](/docs/user-guide/components/reference/terrain/world-renderer) 关卡组件。

创建一个新实体并将其重命名为 **“TerrainSpawner”**。创建一个子实体并将其重命名为 **“HeightMap”**。选择 “TerrainSpawner” 实体 ：

1. 将其平移设置为 (1024, 1024, 256).
2. 添加一个 [**Terrain Layer Spawner**](/docs/user-guide/components/reference/terrain/layer_spawner)。它将显示有关缺少所需组件的警告。
3. 选择 **Add Required Component** ，然后从列表中选择 [**Axis Aligned Box Shape**](/docs/user-guide/components/reference/shape/axis-aligned-box-shape) 。您可以将框尺寸设置为 (2048, 2048, 512).
4. 最后添加一个 [**Terrain Height Gradient List**](/docs/user-guide/components/reference/terrain/height_gradient_list) 组件。从Entity Outliner中选择HeightMap实体。

现在，您可以选择 **“HeightMap”** 实体：

1. 将其平移设置为 (0, 0, 0).
2. 添加一个 [**Shape Reference**](/docs/user-guide/components/reference/shape/shape-reference) 组件，从Entity Outliner中选择"TerrainSpawner"实体。
3. 添加一个[**GradientTransformModifier**](/docs/user-guide/components/reference/gradient-modifiers/gradient-transform-modifier/)组件和一个 [**Image Gradient**](/docs/user-guide/components/reference/gradients/image-gradient/)组件。保存关卡。
4. 在 Image Gradient（图像渐变）中，选择`heightmap_gsi.tif`（选择 StreamingImage with 下拉菜单，否则如果选择图像，当前会出现崩溃）

您的关卡应如下图所示。

![O3DE terrain](/images/learning-guide/tutorials/lumberyard-to-o3de/terrain.png)

## Re-create terrain materials in O3DE

我们需要手动移植位于 Lumberyard 中 `YOUR_LUMBERYARD_FOLDER\dev\Gems\StarterGame\Environment\Assets\Materials\Terrain`中的 5 个地形材质。它们取决于位于 `YOUR_LUMBERYARD_FOLDER\dev\Gems\StarterGame\Environment\Assets\Textures\Terrain` 中的纹理，因此**请将此文件夹复制到您的 O3DE 项目** 或 Gem。

如果你用文本编辑器打开`.mtl`文件，你会发现它们是非常简单的材料。在 O3DE 中，您可以在资源浏览器中右键单击以创建新材质，然后选择 **“Terrain Detail Material”** 类型。那么您需要填写的值是：

- Base color texture -> 使用 “_diff” 纹理
- Displacement Height Map -> 使用 “_displ” 纹理
- UV 中心（始终为 0,0）、平铺 U 和平铺 V（使用漫反射贴图 UV）

下图中有一个 O3DE 中移植的 GroundRock 材质的示例。

![O3DE terrain material](/images/learning-guide/tutorials/lumberyard-to-o3de/terrain-material.png)

## 在 O3DE 中分配地形材质

由于我们引入了 5 种新的地形材质，因此需要创建一个 [**Surface Tag Name List**](/docs/user-guide/gems/reference/environment/surface-data). 类型的新资产。为此，请打开 **资产编辑器**（从顶部栏的“工具”菜单）并选择“文件/新建/曲面标记名称列表”。

添加 “grass”、“hexagon”、“mud”、“rock”、“rockground” 条目，并将资源保存在您的项目 Asset 文件夹中。

![Surface tags](/images/learning-guide/tutorials/lumberyard-to-o3de/surface-tags.png)

选择 **“TerrainSpawner（地形生成器）”实体并创建 5 个新的子实体。它们的名称将为 “Grass”、“Hexagon”、“Mud”、“Rock” 和 “RockGround”。对于他们每个人 ：

1. 设置它的平移为 (0, 0, 0)。
2. 添加一个 [**Shape Reference**](/docs/user-guide/components/reference/shape/shape-reference) 组件，从Entity Outliner中选择"TerrainSpawner"实体。
3. 添加一个 [**GradientTransformModifier**](/docs/user-guide/components/reference/gradient-modifiers/gradient-transform-modifier/) 组件和一个[**Image Gradient**](/docs/user-guide/components/reference/gradients/image-gradient/) 组件。保存关卡。
4. 在 Image Gradient（图像渐变）中，选择与其对应的 splat map。

然后重新选择 **“TerrainSpawner（地形生成器）”** 实体：

1. 添加 [**Terrain Surface Materials List**](/docs/user-guide/components/reference/terrain/surface-material-list)  组件。您需要添加 5 种地形材质，并为它们提供正确的 Surface Tag（带有terrain_grass材质的 Grass 标签等）。
2. 添加一个 [**Terrain Surface Gradient List**](/docs/user-guide/components/reference/terrain/surface-gradient-list) 组件。您需要添加 5 个条目，并从 Entity Outliner 中选取每个表面实体，并为它们指定正确的 Surface Tag（带有 Grass 标签的 Grass 实体等）。

您的关卡应如下图所示。

![O3DE terrain final](/images/learning-guide/tutorials/lumberyard-to-o3de/terrain-final.png)
