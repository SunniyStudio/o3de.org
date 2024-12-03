---
linkTitle: 进一步实验
title: 进一步实验
description: 对创建的地形进行试验。
weight: 600
toc: true
---

在本教程部分中，您可以对创建的地形进行试验，以尝试地形系统的更多功能。

## 缩放地形

此时，地形大小为 256 m x 256 m。但是，如果需要，可以调整比例。

1. 选择 **Terrain Spawner** 实体。

2. 设置 **Axis Aligned Box Shape** 的 **Dimensions** 为 `512.0, 512.0, 100.0`。

3. 设置Transform的 **Z Dimension** 为 `50.0`。

4. 如果高度或表面材质看起来呈块状，请确保将 Image Gradients 上的 Sampling Type 设置为 `Bilinear` 或 `Bicubic`。

5. 通过将框形状设置为其他大小进行试验。

## 复制地形

1. 右键单击 Entity Inspector 中的 **Terrain Spawner** 实体，然后选择 **Create Prefab...**。

2. 将出现一个对话框，询问您是否要移动或保留对外部实体的引用。选择 **移动**。

3. 右键单击 Entity Outliner 并选择 **Instantiate Prefab...**，在关卡中创建地形的第二个副本。

4. 通过在关卡中移动第二个副本来进行实验。

## 创建一个洞

1. 创建新实体 (快捷键 **Ctrl + Alt + N**)。将实体命名为 `Terrain Hole`。

2. 选择实体后，将 [**Transform**](/docs/user-guide/components/reference/transform) 组件的 **Translate** 值设置为`0.0 m`，以便实体位于世界的原点。

3. 添加[**Terrain Layer Spawner**](/docs/user-guide/components/reference/terrain/layer_spawner)组件和 [**Axis Aligned Box Shape**](/docs/user-guide/components/reference/shape/axis-aligned-box-shape) 组件。

4. 设置**Axis Aligned Box Shape**的 **Dimensions** 为 `20.0`, `20.0`, `1.0`。这定义了孔的大小。

5. 在 **Terrain Layer Spawner** 上，禁用 **Use Ground Plane** 并将 **Sub Priority** 设置为 10。这将创建一个没有地形数据且优先级高于默认值的地形生成器。

6. 在视区窗口中，通过拖动 **Terrain Hole** 实体进行实验，使其与主地形的不同部分重叠。
