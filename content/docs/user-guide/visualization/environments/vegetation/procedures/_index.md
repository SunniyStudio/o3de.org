---
linkTitle: 程序化
title: 动态植被程序
description: 学习在Open 3D Engine中创建逼真动态植被的基本程序。
---

在 O3DE 中创建逼真的动态植被始于几个基本程序，您可以对其进行无尽的自定义。

您可以按照出现的顺序执行这些程序，这样就形成了一个基本的工作流程。或者，您也可以选择您需要的程序。这些程序中的示例使用了 Starter Game 项目中的资产。

下表总结了每个记录的程序、其目的和使用的组件。


****

| 程序 | 目的 | 使用的组件 |
| --- | --- | --- |
| [创建植被图层](./layer) | 创建动态植被的第一步。确定植被区域的大小和形状，并添加植被资产。 | Vegetation Layer Spawner, 一个Shape component, 和 Vegetation Asset List |
| [使用渐变创建随机分布](./gradient-random) | 渐变效果可以在选择和放置植被时创造出随机、自然的外观。您可以将 Gradient 组件添加到一个单独的实体中，并从Vegetation Layer Spawner实体中引用它。 | 在 **Gradient** 实体上: **Perlin Noise Gradient** 和 **Gradient Transform Modifier** 在 **Vegetation Layer Spawner** 实体上: **Vegetation Asset Weight Selector** 和 **Vegetation Distribution Filter** |
| [添加缩放、旋转和位置修改器](./adding-modifiers) | 通过添加比例、旋转和位置变化，进一步改变植被的外观。使用您在上一步骤中创建的渐变效果。 | Vegetation Rotation Modifier, Vegetation Scale Modifier, 和 Vegetation Position Modifier |
| [扩大植被覆盖范围](./coverage) |  扩大植被覆盖范围，用于其他区域或覆盖整个关卡。  |  **Vegetation Reference Shape** 或 **Shape** 组件  |
| [在选择区域内阻挡植被](./blockers) |  创建植被阻挡器，防止植被出现在关卡的特定区域。  |  **Vegetation Layer Blocker** 和一个 **Shape** 组件  |
