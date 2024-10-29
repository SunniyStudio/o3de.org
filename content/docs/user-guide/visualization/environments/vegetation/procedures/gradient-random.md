---
linkTitle: 使用 Gradient
title: 使用 Gradient 创建随机分布
description: 将Gradient实体链接到植被实体，在 Open 3D Engine 动态植被中创建随机分布。
weight: 600
toc: true
---

您可以在 O3DE 的不同区域使用渐变，例如音频和人工智能。Gradient在动态植被中尤其有用，它可以在植被的分布中创建真实的随机外观。

您可以通过使用渐变来创建随机选择和随机放置，从而实现随机分布的外观。

在完成这些步骤之前，您必须先 [创建植被层](./layer)。

随机选择意味着网格上每个点被选中的植被是可变的。每个植被被选中的几率取决于分配给它的权重。您可以使用**Vegetation Asset Weight Selector**组件创建基于权重的随机选择。

随机放置意味着网格上有些点有植被，有些点没有。**Vegetation Distribution Filter**会限制**Vegetation Layer Spawner**组件生成的植被数量。

**主题**
+ [创建基于权重的随机选择](./selection-random)
+ [使用Vegetation Distribution Filter随机放置](./place-random)
