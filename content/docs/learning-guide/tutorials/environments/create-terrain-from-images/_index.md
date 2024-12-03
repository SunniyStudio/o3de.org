---
linkTitle: 从图像创建地形
title: 从图像创建地形
description: 了解如何在 Open 3D Engine （O3DE） 中使用源图像作为输入来创建地形。
---

**Open 3D Engine （O3DE）** 中的地形是由 *梯度* 生成的。在本教程的上下文中，渐变表示材质的海拔和权重掩码的变化。渐变可以按程序生成，也可以从图像中处理。在本教程中，您将使用图像渐变创建地形高程并应用细节材质，并添加对具有高度场碰撞器和物理材质的 PhysX 模拟的支持。

本教程要求您在项目中启用**Terrain**, **Gradient Signal**, **Landscape Canvas**, 和 **PhysX** Gem。
最终关卡将如下所示：

| | |
| - | - |
| {{< image-width src="/images/learning-guide/tutorials/environments/terrain-from-images/terrain-final-level-2.png" alt="Illustration of the final tutorial level results." >}} | {{< image-width src="/images/learning-guide/tutorials/environments/terrain-from-images/terrain-final-level.png" alt="Illustration of the final tutorial level results." >}} |

本教程分为几个部分，以便于回顾各个部分。

| 教程                                       | 描述                      |
|------------------------------------------|-------------------------|
| [创建地形就绪关卡](create-a-terrain-ready-level) | 为 O3DE 创建一个准备好编写地形的新关卡。 |
| [创建地形资产](create-terrain-assets)          | 创建本教程所需的 地形资产。          |
| [创建地形形状](create-terrain-shape)           | 在关卡中创建地形的整体形状.          |
| [对地形进行纹理处理](texture-terrain)             | 向地形添加颜色和表面类型。           |
| [物理化地形](physicalize-terrain)             | 通过添加物理碰撞器来物理化地形。        |
| [进一步实验](experiment)                      | 对创建的地形进行试验。             |
