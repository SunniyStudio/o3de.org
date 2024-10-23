---
linkTitle: Navigation Seed
description: ' 使用Navigation Seed组件生成一个彩色编码标记，显示AI在 Open 3D Engine中可以移动的位置。
'
title: Navigation Seed 组件
draft: True
---



**Navigation Seed**组件标记了人工智能代理可以访问的[**Navigation Area**](/docs/user-guide/components/reference/ai/nav-area/) 的大块区域。游戏开发人员可以使用该组件作为视觉辅助工具，以确定人工智能代理可以去的地方。

[**Navigation Area**](/docs/user-guide/components/reference/ai/nav-area/) 组件可以生成一个看起来很复杂的网格，网格中的岛屿互不相连。如果出现这种情况，就很难确定人工智能可以到达的精确位置。在这种情况下，可以使用**Navigation Seed**组件来渲染彩色编码地图。蓝色区域可供人工智能进入，红色区域则无法进入。

![Example Navigation Seed component with red and blue chunks.](/images/user-guide/component/component-navigation-mesh-seed-enabled.png)

例如，[静态物体](/docs/user-guide/components/reference/ai/nav-area#navigating-around-static-objects)、[排除区域](/docs/user-guide/components/reference/ai/nav-area#creating-navigation-mesh-exclusion-areas)或地形特征可以将导航区域划分为多个小块。导航种子***组件会用蓝色标记人工智能可以到达的区域，如果它们已经在该区域内（例如，如果它们在该区域内生成）。您可能会在一个位置设置多个导航区域，例如不同的代理类型。

**要使用Navigation Seed组件**

1. [创建一个导航区域](/docs/user-guide/components/reference/ai/nav-area/)。

1. 使用静态物体、排除区域或地形将导航区域划分为多个区块。

1. 像导航区域实体或另一个实体添加**Navigation Seed**组件。

2. 如果要指定AI代理类型，在 **Navigation Seed** 组件中选择AI代理类型。

3. 移动带此组件的实体。

  如果您开启了可视化功能，那么在您放置导航种子的区块中，所有AI可进入的区域都会呈现蓝色。无法进入的区域则显示为红色。

**计算AI代理类型的可达性**
+ 在**Navigation Seed**组件中，完成以下之一的操作：

  1. 要计算所有AI代理类型的可达性，将 **Agent Type** 字段留空。

  1. 要为指定的AI代理类型计算可达性，在 **Agent Type** 下拉列表中选择一个类型。

默认情况下，导航种子可视化系统未启用。您必须使用控制台启用某些标记。

**要启用Navigation Seed可视化**
+ 启用以下控制台变量。为此，请将值设置为 `1`。
  + `ai_MNMDebugAccessibility` (在 O3DE 编辑器中，你也可以选择 **Game**, **AI**, **Visualize Navigation Accessibility**。/)
  + `ai_DebugDraw`
  + `ai_DebugDrawNavigation`

  更多信息，请参阅 [使用控制台窗口](/docs/user-guide/editor/console/)。

{{< note >}}
**Navigation Seed**组件仅存在于O3DE编辑器中。
{{< /note >}}
