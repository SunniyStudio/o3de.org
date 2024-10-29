---
linkTitle: Noise-Based Random Selection
title: Random Placement Using the Vegetation Distribution Filter
description: Create random distribution by using a noise gradient as a placement mask for Open 3D Engine dynamic vegetation.
weight: 650
toc: true
---

**Vegetation Distribution Filter** 组件通过限制**Vegetation Layer Spawner**组件生成的植被数量，来创建随机放置的外观。

在完成以下步骤之前，您必须具备以下条件：
+ 植被层中至少定义了一个资产。
+ 一个包含渐变组件的实体。有关说明，请参阅 [创建渐变实体](./gradient-random).

**创建随机分布**

1. 在 **Entity Outliner** 中，选择包含 **Vegetation Layer Spawner** 组件的实体。

1. 在 **Entity Inspector** 中，点击 **Add Component** 并选择 **Vegetation Distribution Filter**。

1. 在 **Vegetation Distribution Filter** 组件的属性中，在 **Gradient Entity Id** 中，点击目标按钮。

1. 在 **Entity Outliner** 中，选择 **Gradient** 实体。

1. (可选) 调整**Threshold Min**和**Threshold Max**的值，以指定植被中可以出现多少渐变。 
