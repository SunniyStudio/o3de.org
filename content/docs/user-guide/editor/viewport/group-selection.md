---
linkTitle: 组选择
title: 组选择
description: 在 Open 3D Engine (O3DE) 的 3D 视口中选择和操作实体组。
weight: 300
---

在视口中，您可以选择多个实体。这称为组选择。您可以使用以下快捷键进行分组选择。

| 快捷方式 | 说明 |
| --- | --- |
| 在视口中**鼠标左键单击** 并 **拖拽** 围绕实体创建一个盒子。 |  选择多个实体。  |
|  在视口中按住**Ctrl** 并 **鼠标左键单击** 并 **拖拽**。  | 取消选择多个实体。 |
|  按住**Ctrl** 并 **鼠标左键单击** 一个目标实体。  |  从选区中添加或删除一个实体。如果您已经选择了一组实体，则会将操纵器移动到所选实体组的中心。  |

选择一组实体时，可以使用与单个实体相同的控件。对于组选择，操纵器就像一个临时的父实体。这意味着该功能对单个实体或实体组的作用相同。默认情况下，操纵器默认为组选择的共同父实体。如果选择的实体没有共同的父实体，则会使用世界空间。

## 选择一组实体

1. 在视口中可见多个实体时，按住**Ctrl**并左键单击**单个实体，即可将它们添加到组选择中。

1. 要从组选项中移除一个实体，请按住**Ctrl**，然后**左键单击**。

您可以按住**Shift**键，从组选择的参照空间切换到世界空间。

在下面的示例中，选择了三个实体。由于该组没有共同的父实体，因此操纵器默认为世界空间。实体现在相对于父实体（操纵器）移动。

![Manipulate a group of entities using world space in O3DE.](/images/user-guide/viewportinteractionmodel/viewport-selection-model-6.gif)

## 为一组选择参照空间

1. 选中一组实体后，按住**Ctrl**和**Alt**，然后**左键单击**目标实体。这样就为该组选择了一个参照空间。

1. 使用操作器修改实体。

   在下面的示例中，一组实体相对于参考空间移动。

   ![Move entities as a group in relation to the reference space in O3DE.](/images/user-guide/viewportinteractionmodel/viewport-selection-model-7.gif)

## 为一组实体创建自定义参考空间

选中一组实体后，按住 **Ctrl**，在视口中移动操纵器。这将创建一个自定义参照空间。

在下面的示例中，为一组实体设置了自定义参照空间。

![Move a manipulator to create a custom reference space in O3DE.](/images/user-guide/viewportinteractionmodel/viewport-selection-model-8.gif)

## 父实体参考空间

选择同一父实体的子实体，并使用操纵器更改它们。当您选择一组共享同一父实体的实体时，操纵器会默认其方向为父实体的方向。

在下面的示例中，选择了两个子实体（轮胎）。操纵器会根据父实体（汽车）的方向旋转子实体。

![Manipulate child entities from a parent entity in O3DE.](/images/user-guide/viewportinteractionmodel/viewport-selection-model-9.gif)

## 在组中使用个体影响

- 按住**Alt**，同时修改一组选定的实体。这将修改局部空间中的实体。操纵器的影响是**个体**的，而不是默认的**组**的。您可以遍历和修改实体，并在视口中查看变化。

  在下面的示例中，您可以在平移过程中按住 **Alt**。这样，每个实体都会在各自的本地空间中移动（**个体**操纵器影响）。

   ![Modify each entity in their own local space during translation in O3DE.](/images/user-guide/viewportinteractionmodel/viewport-selection-model-10.gif)

- 从不同的父实体中选择子实体，然后按住 **Alt**。您可以同时修改多个实体，即使它们有不同的父实体。

  在下面的示例中，选择了来自不同父实体的子实体。操纵器同时修改子实体。

   ![Modify child entities from different parents in O3DE.](/images/user-guide/viewportinteractionmodel/viewport-selection-model-11.gif)

- 按下并释放 **Alt**，可动态更改操纵器的影响。

  在下面的示例中，操纵器会改变所选实体的比例，从群体影响切换到个体影响。

   ![Switch group influence to individual influence while modifying entities in O3DE.](/images/user-guide/viewportinteractionmodel/viewport-selection-model-12.gif)
