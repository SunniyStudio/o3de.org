---
linkTitle: 场景管道
title: 场景管道和SceneAPI
description: Open 3D Engine (O3DE) 中场景资产、处理和场景管道的完整指南。
weight: 100
toc: true
---

场景管道是一种专门的资产生成器，可导入源场景文件，并允许场景生成器导出模型和动画等场景产品资产。
1
| 主题 | 说明 |
| --- | --- |
| [Source API](scene-api) | 描述处理源场景文件的库、规则和组的集合。|
| [Scene Graph](scene-graph) | 描述场景图节点、分层数据和迭代机制。 |
| [Scene Manifest](scene-manifest) | 包含有关如何处理 SceneGraph 内容的说明。|
| [Scene Builder](scene-builder) | 资产构建器，用于管理和导出场景图的部分内容。 |
| [用户定义的属性](user_defined_properties.md) | 在场景生成器管道中使用源场景文件中设置的自定义属性的机制。 |
| [Procedural Prefab](procedural_prefab.md) | 使用 Python 处理场景源文件中的预制资产。 |

下图显示了场景管道流程：

![Scene pipeline flow.](/images/user-guide/assets/scene-pipeline/scene-pipeline-flow.png)

在上图中

* 黄色轮廓是资产创建事件
* 蓝色轮廓是使用 BindCall() 的场景创建事件
* 绿色轮廓是通过行为组件发生的场景创建事件
