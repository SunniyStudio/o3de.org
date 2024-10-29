---
linkTitle: 重复使用植被
title: 在多个区域重复使用植被
description: 将动态植被引用到 Open 3D Engine中的另一个区域。
weight: 500
toc: true
---

创建植被实体后，您可以指定自定义的植被出现在另一个区域。为此，您可以引用另一个形状。例如，如果您为关卡创建了一个包含植被资产和排列的森林补丁，那么您可以创建另一个不同形状的实体，并为植被引用这个新形状。有了这项功能，您就可以在关卡的不同区域重复使用植被，而无需每次都创建一个单独的植被实体。

**在新区域中重复使用植被**

1. 在**Entity Inspector**中，选择 **BasicCoverage** 实体。

1. 右击该实体，选择 **Create child entity**， 然后将它命名为 **Cylinder(PlacementTest)**。

1. 点击 **Add Component** 并选择 **Cylinder Shape** 组件。

1. 选择 **Cylinder(PlacementTest)** 实体，将它移除植被区域。

    ![Multiple reference shapes for vegetation areas.](/images/user-guide/vegetation/dynamic/create-new-vegetation-reference-area.png)

1. 选择 **BasicCoverage** 实体。

1. 在 **Vegetation Reference Shape** 组件上，对于 **Shape Entity Id**，指定 **Cylinder(PlacementTest)** 实体。

    ![Specify different shapes for the Vegetation Reference Shape component.](/images/user-guide/vegetation/dynamic/create-new-vegetation-reference-area-1.png)


    **示例**: 植被呈圆柱状，而不是方框状。

    ![Switch the reference shape for the vegetation to appear.](/images/user-guide/vegetation/dynamic/create-new-vegetation-reference-area-2.png)
