---
linkTitle: 基于权重的随机选择
title: 创建基于权重的随机选择
description: 在 Open 3D Engine 中使用基于权重的随机选择创建随机分布。
weight: 700
toc: true
---

使用随机选择创建分布时，网格上特定点所选择的植被资产会有所不同。每种资产的指定权重决定了其被选中的几率。在此过程中，您可以使用**Vegetation Asset Weight Selector**组件指定权重并链接到梯度。

## 准备植被实体

植被实体或包含**Vegetation Layer Spawner**组件的实体必须包含一个为选择所列资产赋值的组件。本程序为此使用了**Vegetation Asset Weight Selector**组件。如果还没有添加多个资产，添加多个资产也是有帮助的，尽管没有必要。如果您只列出了一个资产，那么在[创建Gradient实体](#create-gradient-entity)中链接渐变时将使用您的一个资产和裸地。

**准备植被实体**

1. 确保您的实体上有三个或更多具有 **Vegetation Layer Spawner** 的植被资产。

    要添加更多植被资产，请执行以下操作：

    1. 在 **Vegetation Asset List** 组件的属性中，在 **Embedded Assets**旁边，点击 **+**。

        {{< note >}}
在指定第一个资产时，不需要点击 +，因为默认情况下会显示一个空资产。指定网格。
{{< /note >}}

        ![In the Vegetation Asset List component's properties, next to Embedded Assets, click the plus sign.](/images/user-guide/vegetation/dynamic/dynamic-vegetation-procedures-gradient-random-selection-browse.png)

    1. 在空字串下，列出为 **<asset name>**，在 **Mesh Asset** 旁，点击 **Browse (…)**。

       ![Under the blank asset, next to Mesh Asset, click Browse (…).](/images/user-guide/vegetation/dynamic/dynamic-vegetation-procedures-gradient-random-selection-add-asset.png)

    1. 在搜索框中，输入搜索词，例如 **flower**，然后选择一个植被资产。

        ![In the search bar, enter a search term, such as flower, and select a vegetation asset.](/images/user-guide/vegetation/dynamic/dynamic-vegetation-procedures-gradient-random-selection-select-flower.png)

        用不同的资产重复这一步骤，直到有三项或更多资产。

       虽然您指定了多个资产，但只有第一个添加的资产才会出现在视口中。接下来的几个步骤将向您展示如何分配植被，使其全部出现。

1. 点击 **Add Component**, **Vegetation Asset Weight Selector**。

   在下一步[创建Gradient实体](#create-gradient-entity)中，您将把该组件与梯度连接起来，梯度会在给定位置提供 0.0 到 1.0 之间的值。然后根据这些值以及每个资产的权重和顺序值来映射资产。

## 创建Gradient实体

梯度实体为植被实体提供噪声信号参考。

**创建梯度实体**

1. 在植被区域实体下创建一个子实体并选中它。

1. 将新实体重命名为一个描述性名称，如**Gradient**。

    ![Rename your new entity to a descriptive name, such as Gradient.](/images/user-guide/vegetation/dynamic/dynamic-vegetation-procedures-gradient-random-selection-rename-entity.png)

1. 添加 **Perlin Noise Gradient** 组件。

    该组件生成的噪声类型称为Perlin噪声，它模仿了自然界中的随机性类型。

    {{< note >}}
如果组件列表中没有 **Gradient** 类别，则必须启用 **Gradient** gem。
{{< /note >}}

1. 添加所需的 **Gradient Transform Modifier** 组件。

   该组件控制如何在世界空间中生成程序噪音。

1. 点击 **Add Required Component** 并选择 **Vegetation Reference Shape**。

1. 在 **Vegetation Reference Shape** 组件的属性，在 **Shape Entity Id** 旁边，点击目标图标。

    ![In the Vegetation Reference Shape component's properties, next to Shape Entity Id, click the target button.](/images/user-guide/vegetation/dynamic/dynamic-vegetation-procedures-gradient-random-selection-target.png)

1. 在 **Entity Outliner** ，选择 **TestBox** 实体 (或包含形状的实体，如果您给它取了别的名字)。

    **Shape Entity Id** 字段弹出您选择的实体名称，并使用该实体上的形状作为参考形状。

    ![In the Entity Outliner, select the TestBox entity.](/images/user-guide/vegetation/dynamic/dynamic-vegetation-procedures-gradient-random-selection-basic-coverage.png)

## 将渐变效果链接到植被区域

在您创建的梯度对植被产生任何影响之前，您必须在植被区域内引用梯度。这意味着您引用梯度的组件（本例中为**Vegetation Asset Weight Selector**）会使用梯度信息来选择值。

您可以在任何组件中引用您创建的渐变效果。在操作步骤 [添加缩放、旋转和位置修改器](./adding-modifiers/)中，植被修改器使用了相同的渐变效果。

**将梯度实体与植被实体连接起来**

1. 选择实体 **BasicCoverage**。

1. 在 **Vegetation Asset Weight Selector** 组件属性中，在 **Gradient Entity Id** 旁边，点击目标。

    ![In the Vegetation Asset Weight Selector component's properties, next to Gradient Entity Id, click the target.](/images/user-guide/vegetation/dynamic/link-gradient-entity-to-vegetation-target.png)

1. 在 **Entity Outliner**， 选择 **Gradient** 实体。

    **Gradient Entity Id** 字段使用实体名称填充。

   现在，您的植被区域在选择植被时应有所变化。

    ![Vegetation area with random selection of listed assets.](/images/user-guide/vegetation/dynamic/link-gradient-entity-to-vegetation-distributed.png)
