---
linkTitle: 添加修改器
title: 添加缩放、旋转和位置修改器
description: 在 Open 3D Engine中添加缩放、旋转和位置修改器，以改变动态植被的外观。
weight: 150
toc: true
---

在完成此步骤之前，您必须先 [创建植被层](./layer/)。

如果您添加了[用于随机选择的梯度](./gradient-random/)，植被的选择看起来是随机的，但仍然会产生网格状的图案。这是因为每种植被都大小相同、朝向相同，而且确实是种植在网格上的点上。

![Vegetation area before adding any modifiers.](/images/user-guide/vegetation/dynamic/dynamic-vegetation-procedures-adding-modifiers-before.png)

要解决这个问题，请执行以下操作：
+ **添加缩放修改器** - 改变植被的大小。您可以指定一个比例范围，在保持原始比例的情况下增大或减小植被的大小。
+ **添加旋转修改器** - 更改植被的朝向。在此步骤中，您只需修改 Z 轴旋转。
+ **添加位置修改器** - 更改植物出现在网格点上的位置。您可以改变植物在 x、y 和 z 轴上的位置。修改 x 轴和 y 轴可移动植物在地平面上的位置。修改 Z 轴可改变植物萌发的高度（此步骤不会修改 Z 轴）。

在植被上应用修改器可以让植被看起来更加逼真自然。

![Vegetation area after adding the scale, rotation, and position modifiers.](/images/user-guide/vegetation/dynamic/dynamic-vegetation-procedures-adding-modifiers-after.png)

## 添加缩放修改器

缩放修改器可改变植被的大小。

**添加缩放修改器**

1. 选择**BasicCoverage**实体。

1. 在**Entity Inspector**中，添加**Vegetation Scale Modifier**组件。

   这个组件本身没有任何作用，因为还没有任何信息告诉它如何分配数值。

1. 在 **Vegetation Scale Modifier** 组件的属性中，在 **Gradient** 下，**Gradient Entity Id** 旁边，单击目标。

    ![In the Vegetation Scale Modifier component's properties, under Gradient, next to Gradient Entity Id, click the target.](/images/user-guide/vegetation/dynamic/dynamic-vegetation-adding-scale-modifier-target.png)

1. 在**Entity Outliner**中，选择**Gradient**。

   **Gradient Entity Id**字段会填入实体名称。

   该组件的**Range Min**和**Range Max**默认都设置为 1.0，因此还没有结果。

1. 将**Range Min**值调整为`0.4`，将**Range Max**值调整为`1.25`。

   **Range Min** 设置梯度信号最低值的比例大小。**Range Max***为梯度信号的最高值设置比例大小。由于梯度信号从黑色到白色的范围不同，因此该植被实例将使用介于最小值和最大值之间的比例值。

## 添加旋转修改器

旋转修改器可改变植被的旋转。

**添加旋转修改器**

1. 选择**BasicCoverage**实体。

1. 在**Entity Inspector**中，添加**Vegetation Rotation Modifier**组件。

   这个组件本身没有任何作用，因为还没有任何信息告诉它如何分配数值。

1. 在**Vegetation Rotation Modifier** 组件的属性中，在**Rotation Z**、**Gradient**下，在**Gradient Entity Id**旁，点击目标。


    ![In the Vegetation Rotation Modifier component's properties, under Rotation Z, Gradient, next to Gradient Entity Id, click the target.](/images/user-guide/vegetation/dynamic/dynamic-vegetation-adding-rotation-modifier-target.png)

1. 在 **Entity Outliner**中，选择 **Gradient**。

   **Gradient Entity Id** 字段中填入实体名称。

## 添加位置修改器

位置修改器会使每个植被实例移动一个由梯度决定的量，从而消除植被的网格状外观。

**添加位置修改器**

1. 选择 **BasicCoverage** 实体。

1. 在**Entity Inspector**中，添加**Vegetation Position Modifier**组件。

   这个组件本身没有任何作用，因为还没有任何信息告诉它如何分配数值。

1. 在 **Vegetation Position Modifier** 组件的属性中，在 **Position X** 下，**Gradient**，在 **Gradient Entity Id** 旁边，单击目标。

    ![In the Vegetation Position Modifier component's properties, under Position X, Gradient, next to Gradient Entity Id, click the target.](/images/user-guide/vegetation/dynamic/dynamic-vegetation-procedures-adding-modifiers-target.png)

1. 在**Entity Outliner**中，选择 **Gradient**。

   **Gradient Entity Id** 字段会填入实体名称。

   其结果是 x 轴在指定范围内略有变化（默认值为 -0.3 至 0.3）。如需更大的变化，可修改**Range Min**和**Range Max**。

1. 在**Vegetation Position Modifier**组件的属性中，在**Position Y**, **Gradient**下。在**Gradient Entity Id**旁，点击目标。

    ![In the Vegetation Position Modifier component's properties, under Position Y, Gradient, next to Gradient Entity Id, click the target.](/images/user-guide/vegetation/dynamic/dynamic-vegetation-procedures-adding-modifiers-target-y.png)

1. 在**Entity Outliner**中，选择**Gradient**。

   **Gradient Entity Id** 字段会填入实体名称。

   在此过程中，您会将相同的梯度信号传递给 x 和 y 两个修改器，从而使两个修改器产生相同的偏移量。这样，所有植被都会向一个共同的对角线方向移动，而不是根据 Y 值改变 X 值。

   您可以通过在**Position Y** 属性中提供一些额外的值来覆盖这一效果。

    {{< note >}}
   您也可以在现有渐变上使用渐变修改器，或创建一个单独的渐变并链接到它，从而解决这个问题。
{{< /note >}}

1. 在 **Position Y**下，展开**Advanced**头下，勾选 **Enable Transform**。

![Under Position Y, expand the Advanced header and check Enable Transform.](/images/user-guide/vegetation/dynamic/dynamic-vegetation-procedures-adding-modifiers-transform.png)

1. 要在 Y 轴上产生旋转效果，请使用以下数值或其变体。

| | **X** | **Y** | **Z** |
| -: | :- | :- | :- |
| **Translate** | `0.5` | `0.5` | `0.0` |
| **Scale** | `-1.0` | `-1.0` | `1.0` |
| **Rotate** | `0.0` | `0.0` | `45.0` |
