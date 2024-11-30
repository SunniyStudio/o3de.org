---
linktitle: 创建自定义节点
title: 在 Script Canvas 中创建自定义节点
description: 了解如何在 Open 3D Engine （O3DE） 中创建自定义 Script Canvas 节点。
weight: 200
---

在 Script Canvas 中创建自定义节点可为您提供对节点功能的最大控制和灵活性。在以下情况下，您可能希望创建自定义节点：

* 当您的节点具有状态、时间或潜在结果时。
* 创建复杂节点时。
* 当您需要控制节点的拓扑时。
* 当您想反映自己的 C++ 免费函数时。

为了简化创建自定义节点的过程，Script Canvas 使用名为 [AzAutoGen](/docs/user-guide/programming/autogen/) 的模板化自动代码生成系统，以显著减少启动和运行节点所需编写的“样板代码”数量。使用 AzAutoGen 可以让开发人员立即专注于新节点的功能，因为节点显示在 [Node Palette](/docs/user-guide/scripting/script-canvas/get-started/concepts-and-terms/#node-palette)中所需的代码已经存在。

创建自定义节点需要执行以下四个步骤：

1. 创建代码生成 XML 文件。
1. 为您的节点创建 C++ 文件。
1. 将这些文件添加到 CMake。
1. 注册您的新节点。

## 相关信息

为了更好地了解如何创建自定义节点，我们建议您阅读以下内容
- [节点](/docs/user-guide/scripting/script-canvas/editor-reference/nodes/)
- [文本替换](/docs/user-guide/scripting/script-canvas/editor-reference/text-replacement/)
- [AzAutoGen](/docs/user-guide/programming/autogen/)


## 主题

|主题 | 描述                                                               |
| --- |------------------------------------------------------------------|
| [自定义Nodeable节点](custom-nodeable-nodes/) | 如何在 Open 3D Engine （O3DE） 中创建自定义 Script Canvas Nodeable节点。               |
| [自定义自由函数节点](custom-free-function-nodes/) | 如何在 Open 3D Engine （O3DE） 中创建自定义 Script Canvas Free Function 节点。 |
| [节点定义参考](custom-node-definition-reference/) | Script Canvas 节点定义的参考指南。                                         |
| [动态数据插槽](dynamic-data-slots/) | 使用动态数据槽使单个 Script Canvas 节点能够在 Open 3D Engine （O3DE） 中处理各种数据类型。  |

