---
title: Terrain Detail Material
linktitle: Terrain Detail Material
description: 'Open 3D Engine (O3DE) Terrain Detail Material 参考。'
weight: 100
---

**Terrain Detail Material** 与宏观材质混合，为地形表面创建更自然的外观。

## 说明

细节材质是您可以使用 [材质编辑器](/docs/atom-guide/look-dev/tools/material-editor/)创建的标准材质。它们通过渐变生成的表面标签名称分配给地形。这允许您在大型地形表面分配许多材质并在它们之间进行混合。细节材质显示在距摄像机的 **Detail material render distance** 内的地形上。

这种细节材质与宏材质的混合使您能够通过在整个地形中创建颜色和光照的变化来使用小型、高保真、平铺的细节材质。在以下示例中，左侧的草地是未与宏材质混合的细节材质。右侧的草地是相同的细节材质，但它已混合在低保真微距材质上。请注意颜色和光照的变化，这些变化会产生不太均匀但更自然的外观：

{{< image-width src="/images/learning-guide/tutorials/environments/macro-material-blending.png" width="800" alt="An example of the difference in variation created by macro material blending." >}}

{{< tip >}}
在 **Terrain World Renderer** 关卡组件中，有一个名为 **Detail material configuration** 的属性组，可用于配置从摄像机停止将细节材质混合到宏材质中的距离，以及淡出混合细节材质的距离。
{{< /tip >}}

## 用法

应用详图材质是一个可以分为几个步骤的过程：

1. 在 [Material Editor](/docs/atom-guide/look-dev/tools/material-editor/) 中创建要应用于地形的材质。
1. 创建定义细节材质区域的图像渐变。
1. 创建表面标签以引用与特定坡度关联的地形部分。
1. 将材质作为细节材质应用于地形。

有关此过程的分步示例，请按照**从图像创建地形**教程的 [应用详细材质](/docs/learning-guide/tutorials/environments/create-terrain-from-images/#apply-detail-materials) 部分进行操作。
