---
linkTitle: 植被
title: 植被
description: 在Open 3D Engine (O3DE)中以程序化方式在整个世界中放置植被。
weight: 100
toc: true
---

动态植被系统使用植被组件为任何大小的关卡定制动态植被覆盖。要使用动态植被系统，必须为项目启用 Vegetation Gem 。

![Example of vegetation landscape that you can achieve by using dynamic vegetation.](/images/shared/dynamic-vegetation-intro.png)

利用植被和其他类别组件的组合，您可以实现以下功能：

* 创建背景植被层，并在环境对象和建筑物等兴趣点周围指定局部植被区域。
* 使用所见即所得（WYSIWYG）的实时植被编辑功能放置植被。
* 在任何开发阶段重新配置植被的任何方面，无需从头开始。所有编辑都是无损的，并在整个关卡中快速填充。
* 在一个嵌套Slice中创建复杂的生态生物群落，其中包含多层广泛的覆盖范围，可用于跨区域或整个世界的混合。
* 配置植被，使其只能生长在特定表面。使用组件指定植被的生长位置，如在一定范围的地面坡角或指定高度。

动态植被系统与静态植被系统有多种互动方式。例如，您可以执行以下操作：

* 在动态和静态植被系统中使用相同的资产
* 配置静态植被以阻止动态植被

<!-- 需要检查其准确性

动态植被与静态植被有以下不同。

|  | 动态植被 | 静态植被 |
| --- | --- | --- |
| World size | Limited only by O3DE maximum world size | Maximum of 2K-4K |
| Templates | Slices | Not templatable |
| Saved as | Procedural mechanisms | Static placement data |
| Generated | In-game just-in-time placement | As level data |
-->

您可以使用其他类别的组件进一步修改动态植被。要使用动态植被系统的全部功能，请启用以下 Gems：

**Vegetation Gem (required)**
提供动态植被系统和核心植被组件。这些是创建区域并在系统中注册的基础。

**Gradient Signal Gem (dependency)**
提供梯度和梯度修改器组件，这对动态植被的程序驱动方法至关重要。梯度组件生成各种梯度信号，如随机噪声和佩林噪声。梯度修改器组件可修改和混合梯度信号。
将梯度信号与修改器组件（如位置抖动）或滤波器组件（如植被分布）一起使用，可在游戏世界中产生真实的随机植被表达。

**Surface Data Gem (dependency)**
使地形或网格等表面能够发出信号或标签，以传达其表面类型。使用植被表面屏蔽过滤器，您可以通过包含列表选择在特定表面生长植被，或通过排除列表阻止植被生长。您还可以使用**Surface Mask Gradient**组件，以梯度信号的形式重新捕获标签。

**FastNoise Gem (optional)**
提供了一个极具表现力的**FastNoise Gradient**组件，可生成多种程序噪声变化。在**Entity Inspector**中，**FastNoise Gradient**组件出现在**Gradient**类别中。您可以像使用其他渐变组件一样使用它。

**主题**

* [动态植被概念](./concepts)
* [动态植被程序化](./procedures)
