---
linkTitle: White Box
title: White Box Gem
description: White Box Gem 为 Open 3D Engine （O3DE） 提供 White Box 快速设计组件。
toc: true
---

White Box Gem 提供了两个作为组件实现的工具;**White Box**组件和 **White Box Collision**组件。您可以使用 White Box （白盒） 零部件快速绘制实体的代理几何体。将 White Box Collision （白盒碰撞） 组件与 White Box （白盒） 组件结合使用，为 PhysX 模拟提供碰撞表面。

## 什么是白拳？

*白框化* 是一个快速的迭代设计过程。简单的建模工具用于创建定义 3D 对象和环境的形状和尺寸的代理几何体。在此过程中创建的代理几何体具有非常少的多边形数量，但可以合理地近似最终资源的大小和形状。白盒实体具有基本材质，如果需要，可以具有简单的碰撞和物理材质。在白盒流程中创建的实体可以交给艺术家，以开发成精美的生产实体。

在 **Open 3D Engine （O3DE）** 中，您可以使用 White Box 组件创建实体，使用其编辑功能快速绘制 3D 资产草图，以用作关卡的构建块。由于您在 O3DE 的 **编辑器** 中使用组件实体系统创建白盒网格，因此这些实体还可以包含来自其他组件（如脚本）的功能。在致力于开发可用于生产的艺术资源之前，您可以使用快速构建和测试迭代周期来创建精致的功能性实体。生成的白盒几何体和实体将用作生产资源的模板。此开发过程快速、高度迭代且经济高效。

## White Box 组件信息

有关 White Box 组件的信息，请参阅 [White Box 组件](/docs/user-guide/components/reference/shape/white-box)。

有关 White Box Collision （白盒碰撞） 组件的信息，请参阅 [White Box Collision 组件](/docs/user-guide/components/reference/shape/white-box-collider)。
