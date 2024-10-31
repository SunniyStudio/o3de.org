---
title: Post-processing Modifier 组件
linktitle: Post-processing Modifiers
description: 'Open 3D Engine (O3DE) Post-Processing Modifier 组件。'
toc: true
---

在**Open 3D Engine (O3DE)**中，后处理效果（PostFX），如**Bloom**和**Depth of Field**，是由后置效果修改器组件控制的。通过 PostFX 修改器组件，您可以将 PostFX 分配给图层，并通过形状、渐变或半径来定义它们的体积、面积和重量。在 O3DE 中，这组配置被称为 *PostFX volume*。PostFX 体积描述了一个包含 PostFX Layer组件、PostFX 修改器组件和形状的实体。


## 组件

| 组件 | 说明 | 
| - | - |
| [**PostFX Layer**](/docs/user-guide/components/reference/atom/postfx-layer/) | 控制如何在场景中应用PostFX 修改器。下面的PostFX 修改器组件需要一个PostFX Layer。|
| [**PostFX Gradient Weight Modifier**](/docs/user-guide/components/reference/atom/postfx-gradient-weight-modifier/) | 根据其他实体提供的梯度修改 PostFX 的权重。 |
| [**PostFX Shape Weight Modifier**](/docs/user-guide/components/reference/atom/postfx-shape-weight-modifier/) | 使用**Shape**组件定义一个体积，并修改体积内的后期特效重量。在体积内，PostFX 的重量保持不变，而在体积外则开始下降。 |
| [**PostFX Radius Weight Modifier**](/docs/user-guide/components/reference/atom/postfx-radius-weight-modifier/) |  以实体原点为半径定义一个球体，并根据摄像机在半径内的位置修改 PostFX 的权重。 |


## 教程

[Open 3D Engine中的后期处理效果](/docs/learning-guide/tutorials/postfx/)
