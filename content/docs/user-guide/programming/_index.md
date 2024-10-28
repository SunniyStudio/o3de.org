---
description: 了解Open 3D Engine (O3DE)的核心系统，以及如何通过编程扩展这些系统。
title: Open 3D Engine编程指南
linktitle: 编程
weight: 600
---

**引擎核心**包括源代码、系统和模块，它们是**Open 3D Engine (O3DE)** 及其工具的基础。所有这些工具--**O3DE Editor**、**Asset Processor**、**Script Canvas**等--共同使用现代渲染技术和游戏逻辑构建交互式 3D 体验。使用这些工具，您可以将设计从一个空场景转变为一个可以部署和共享的完整项目。在本节中，您将深入了解引擎核心的各个部分，并学习如何扩展 O3DE 的工具包。

## 先决条件

* C++17
* Python 3

O3DE 的源代码是用 C++ 编写的，有几个辅助工具是用 Python 编写的。我们建议您具备 C++ 或 Python 编程的基础知识来扩展 O3DE。


## 从这里开始

### 核心模块

首先了解驱动 O3DE 及其扩展功能的核心模块。核心模块是 O3DE 所有功能的入口。它们位于引擎的 `Code\Framework` 目录中。

| 模块 | 说明 |
| - | - | 
| [`AzCore`](https://o3de.org/docs/api/frameworks/azcore/) | 提供数学、序列化、内存管理、事件处理和发布/子接口，以及加载插件模块的功能。它提供组件实体模型，并包含 C++ STL 的实现，包括内存对齐感知容器和其他保证。 |
| [`AzFramework`](https://o3de.org/docs/api/frameworks/azframework/) | 提供更高级别的结构。它包含一些与大多数 O3DE 应用程序通用的附加代码。 |
| [`AzGameFramework`](https://o3de.org/docs/api/frameworks/azgameframework/) | 包含运行时应用程序专用的核心功能。它提供循环管理和运行时应用程序专用的引导序列。 |
| [`AzToolsFramework`](https://o3de.org/docs/api/frameworks/aztoolsframework/) | 包含工具专用的核心功能。它提供用户界面组件，如对象选择器、属性编辑器、源控制集成以及工具专用的引导序列。 |
| [`AzQtComponents`](https://o3de.org/docs/api/frameworks/azqtcomponents/) | 包含常用的用户界面部件（如滚动条、按钮、对话框等），可为工具应用程序提供一致的外观和感觉。 |

下图展示了 O3DE 核心模块的依赖关系图。

![O3DE core module dependency graph](/images/user-guide/programming/o3de-architecture-dependency-graph.svg)

使用这些框架的高级产品有：

* **项目运行时**: 开发人员使用 O3DE 创建的终端产品，如游戏、专用服务器运行时和世界模拟。

* **CLI 工具**: 从命令行界面（CLI）调用的工具，如**O3DE CLI**、**AZSL Compiler**和**Asset Batch Processor**。

* **GUI 工具**: 在开发 O3DE 项目时，可使用具有图形用户体验的工具，如**O3DE 编辑器**、**资产处理器**和**材质编辑器**。


### O3DE 目录

如果您对开发 O3DE、创建组件和 Gem 或在项目中编程感兴趣，熟悉 O3DE 源代码中的以下子目录会很有帮助：

  * `cmake`: 包含 O3DE 的配置、下载和构建脚本。

  * `Code`: 包含用于构建 O3DE 和提供 API 的 C++ 代码和头文件。应用程序接口由库组织，每个库都包含一个定义明确的功能集。库头文件提供了虚拟接口，您可以通过实现这些接口将您的代码连接到相关的 O3DE 系统或功能。

  * `Code/Framework`: 包含 O3DE 核心模块使用的所有源代码和头文件。

  * `Gems`: 包含引擎自带 Gem 的源文件和构建文件。每个 Gem 都有自己的子目录，一个 Gem 可能包含其他 Gem。例如，Atom Gem 包含多个为 Atom 渲染器提供各种工具、库、接口和实用程序的 Gem。

  * `Templates`: 包含Gem和项目的默认模板。

## 学习路径

根据您要开发的引擎部分，您可以遵循不同的学习路径。虽然每种学习路径都可能与您的情况不同，但以下主题可以帮助您开始一些常见的开发任务：


#### 开发组件
  
  - [组件实体系统(CES)](/docs/welcome-guide/key-concepts/#the-component-entity-system)
  
  - [事件总线(EBus)系统](/docs/user-guide/programming/messaging/ebus/)
  
  - [组件开发程序员指南](/docs/user-guide/programming/components/)


#### 开发Gem
  
  - [Gem模块系统](/docs/user-guide/programming/gems/overview/)

  - [创建O3DE Gem](/docs/user-guide/programming/gems/creating/)


#### 开发Atom渲染器功能
  
  - [Atom渲染器开发者指南](/docs/atom-guide/dev-guide/)


## 相关主题

| 主题 | 说明 |
| --- | --- |
| [O3DE C++ 编码标准](https://github.com/o3de/sig-core/blob/main/governance/Coding-Standards-and-Style-Guide.md) | 如果您要为 [o3de 存储库](https://{{< links/o3de-source >}})编写功能，请遵循 O3DE C++ 代码库中使用的编码标准。 |
