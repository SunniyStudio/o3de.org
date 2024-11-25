---
linkTitle: 八叉树生成
title: 八叉树生成
description: 在 Open 3D Engine （O3DE） 中使用 Kythera AI Gem 生成用于 3D 导航的八叉树
weight: 350
toc: true
---

八叉树用于 3D 飞行导航，是用于地面导航的 [导航网格](navmesh-generation)的 3D 等效项。与导航网格一样，关卡的物理几何体也用作基础。与导航网格不同，目前不支持动态更新，并且必须显式触发生成。

## 边界设置

八叉树生成将在关卡中存在的边界组件内进行。使用 **Polygon Prism Shape** 组件形成边界，并使用 **Bounds Octree** 组件标记它以生成八叉树。可以标记 octree 和 navmesh 生成的实体。

场景中的一个实体应具有 **Octree** 组件，该组件提供全局八叉树生成设置。否则将使用默认设置。

## 配置

![Octree component settings](/images/user-guide/gems/kythera-ai/octree-configuration.png)

|物业 |描述 |
| - | - |
| **Cell Size** | 八叉树中最小单元的大小（以世界单位表示）。单元格大小应设置为略大于最小飞船的半径，这样，如果半径略有变化，最小的飞船不会立即跳入下一个类别。如果需要更接近几何体的导航，则可以将此属性设置为最小船只半径的一小部分。例如，如果您当前的船只半径略低于 4m，则可以将单元格大小设置为“4”。较小的单元格大小通常会导致更多的节点并需要更多的内存，但这并不是直接关系。 |
| **Min Ship Radius** | 这指定了最小飞船的半径（以八叉树像元大小单位为单位）。如果根据最小飞船的半径设置像元大小，请将此属性设置为`1`，如果根据飞船半径的一小部分设置，则将其设置为适当的倍数。 |
| **Max Ship Radius** | 这指定了可以使用 octree 进行导航的船舶的最大半径（以 octree 单元格大小单位表示）。半径范围较广将创建更多节点，并且需要更多内存。单个半径是理想的。 |
| **Tree Depth** | 显式设置 octree 的深度 （级别数）。将此值设置得太低将导致 chunk 丢失。建议使用自动深度。 |
| **Hard Boundaries** | 选中后，导航边界的边缘为硬边界。这将阻止 AI 离开 octree。在许多用例中，这不是一个实际的要求，它确实会显著增加内存使用量 |
| **Auto Depths** | 选中后，根据像元大小和边界大小设置树深度。建议使用自动深度。|

一个关卡仅支持一种配置。但是，与导航网格不同的是，它旨在适应多个 3D 代理（飞船）大小。没有与 NavMesh.xml 文件等效的文件。

## 最小化节点和内存

octree 中的节点越多，搜索时间就越长，需要的内存就越多。要提高性能并减少内存使用量，请执行以下操作：

* 启用 **Hard Boundaries**，如果可能。

* 使用尽可能大的 **Cell Size**。确保离轴单元能够可靠地捕获所需的最小孔径。请向 Kythera 团队寻求这方面的指导。

* 使 **Min Ship Radius** 和 **Max Ship Radius** 之间的差异尽可能小。理想情况下，它们应该是相等的。如果您在尝试支持一艘非常小的飞船和非常大的飞船时遇到记忆问题，请考虑是否可以采取其他方法或与 Kythera 团队交谈。

* 将 Auto Depths 保持启用状态。

## 生成

要生成或重新生成 octree，请使用 console 命令 `kyt_GenOctree`。
  
完成后将弹出一个模态对话框。

或者，使用Kythera工具栏上的 **Generate Octree** 按钮 (![Generate octree icon](/images/user-guide/gems/kythera-ai/toolbar-generate-octree.png))。

## 储蓄

要将 octree 保存到磁盘，请使用 console 命令 `kyt_SaveOctree`. 
  
或者，使用Kythera工具栏上的 Save Octree 按钮 (![Save octree icon](/images/user-guide/gems/kythera-ai/toolbar-save-octree.png))。

## 调试可视化

Kythera 提供了 octree 的调试绘制。

![Octree debug visualization](/images/user-guide/gems/kythera-ai/octree-debug-visualization.png)

要启用 octree 调试绘制，请使用控制台变量`kyt_DrawMaster 1` 和  `kyt_DrawNavOctree <1-3>`。

可导航的八叉树单元以黄色绘制，不可导航的八叉树单元以红色绘制。

有几种调试模式：

 * `0` 关闭  
 * `1` 绘制可导航空间 
 * `2` 绘制不可导航空间
 * `3` 绘制可导航空间和不可导航空间
