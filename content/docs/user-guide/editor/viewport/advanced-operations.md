---
linkTitle: 高级操作
title: 3D视口高级操作
description: 学习在 Open 3D Engine (O3DE) 的 3D 视口中操作实体的高级操作
weight: 400
---

## 重置实体的变换、旋转或缩放

您可以重置实体的变换，这样就可以将实体恢复到默认位置、旋转和缩放。

在视口中，选择一个实体或一组实体，然后按**R**。

请注意，重置的效果取决于当前的操纵模式。例如，如果您处于平移模式，所选实体的平移将被重置。 要重置所选实体的比例，请先切换到比例操纵器。

![Modify entities and reset their transform in O3DE.](/images/user-guide/viewportinteractionmodel/viewport-selection-model-reset-transform-1.gif)

## 修改操纵器

您可以将操纵器设置为自定义位置、旋转或缩放。

1. 在视口中，选择一个实体。

1. 选择要修改的操纵器的变换模式。

1. 按住**Ctrl**，**左键单击并拖动**操纵器，修改其位置、旋转或比例。

在下面的示例中，操纵器远离小车，切换到旋转模式，并旋转小车。

![Move and rotate an entity using a manipulator in O3DE.](/images/user-guide/viewportinteractionmodel/viewport-selection-model-3.gif)

## 重置操纵器的变换、旋转或缩放

您可以在相关操纵器处于激活状态时按下 **Ctrl + R** 键，将其重置为默认位置、旋转或缩放。

## 匹配实体变换

您可以使用**同上**功能将一个实体的变换数据从一个实体共享到另一个实体，而不是手动将一个实体的位置值复制到另一个实体。通过该功能，您可以为实体复制相同的变换数据。例如，要使实体共享相同的旋转，可以选择一个实体，然后使用同上功能将目标实体的旋转应用到所选实体。

1. 在视口中，选择一个实体。

1. 按住**Ctrl**并按下**鼠标中键**，将鼠标悬停在目标实体上。选定目标后，当前的选择变换将与目标相匹配。

   在下面的示例中，同上功能与另一个实体共享一个实体的旋转。这将导致两个实体共享相同的值。

   ![Share the transform data from one entity to another using the ditto feature in O3DE.](/images/user-guide/viewportinteractionmodel/viewport-selection-model-13.gif)

## 同上一组选择

您可以对一组实体使用同上功能。这样可以更方便地同时修改多个实体。

1. 在视口中，选择一组实体。

1. 将鼠标悬停在目标实体上时，按住**Ctrl**并按下**鼠标中键**，即可将组的变换数据与目标数据匹配。

   在下面的示例中，轮胎组选择使用了同上功能来匹配目标实体--汽车。

   ![Share the transform data from multiple entities to another using the ditto feature in O3DE](/images/user-guide/viewportinteractionmodel/viewport-selection-model-14.gif)

##同上一组选择到本地空间

您可以使用单个影响（**Alt**）将一组实体同上，这样就可以在各自的局部空间中修改实体与另一个实体的关系。

1. 在视口中，选择一组实体。

1. 按住**Ctrl**和**Alt**，并在鼠标悬停在目标实体上时按下**鼠标中键**。这样就可以将选定组中每个实体的局部空间设置为您指定的目标实体。

   在下面的示例中，一组实体使用同上功能将其局部空间变换设置为目标实体的局部空间变换。

   ![Use the ditto feature to set the local space for a selection of entities to a target entity in O3DE.](/images/user-guide/viewportinteractionmodel/viewport-selection-model-15.gif)

