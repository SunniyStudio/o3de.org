---
linkTitle: 阻挡植被
title: 在选定区域阻挡植被
description: 在 Open 3D Engine中创建不希望出现植被的区域。
weight: 800
toc: true
---

您可以在关卡中创建阻止植被出现的区域。例如，您可以使用此功能在建筑物或住宅周围创建一些区域，使植被不会出现。

**在选定区域阻挡植被**

1. 创建一个实体，命名为*Blocker*。

1. 点击 **Add Component** 并选择 **Vegetation Layer Blocker** 组件。

1. 点击 **Add Required Component** 并选择一个形状，例如 **Box Shape** 组件。

1. 在 **Box Shape** 组件属性中，对于 **Dimensions**，输入X、Y和Z轴的值，例如`6.0`, `6.0`, 和 `4.0`。

    ![Create a vegetation area blocker.](/images/user-guide/vegetation/dynamic/block-vegetation-select-areas-1.png)

1. 将**Blocker**实体移动到植被区域。该实体会阻止植被出现在指定的形状中。

    **示例:** 与**Mesh**组件放置在同一区域的箱形植被遮挡器可遮挡塔网格内和周围的植被。

    ![Specify vegetation blockers in your level.](/images/user-guide/vegetation/dynamic/block-vegetation-select-areas-2.png)
