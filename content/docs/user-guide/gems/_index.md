---
linkTitle: Gems
title: Open 3D Engine中的Gem
description: 您可以使用 Gem 向 Open 3D Engine 游戏项目添加功能和资源。
weight: 400
---


**Gem** 是包含代码和/或资产的包，用于增强您的 **Open 3D Engine （O3DE）** 项目。使用 Gem 系统，您可以选择项目所需的功能和资源，而无需包含不必要的组件。您还可以创建自定义 Gem 来包含资产集合、扩展 Editor 或为项目开发功能和游戏逻辑。

O3DE 具有两种类型的 Gem：

- **Code Gem（代码Gem）**: 包含资产以及对资产执行某些功能的代码。

- **Assets Gem**（资产Gem）: 仅包含资产，不包含代码。

Gem 来自三个不同的来源：

- **标准 O3DE Gems**: 被视为核心 O3DE 一部分的 Gem。所有标准 O3DE Gem 均提供 O3DE 材质。

- **第三方 Gems**: 由第三方开发人员提供的 Gem。第三方 Gem 由外部来源提供。
  
- **自定义 Gems**: 您的团队创建的 Gem。自定义 Gem 由外部来源提供。

O3DE 与标准 O3DE Gem 捆绑在一起。它们的源代码位于`<engine>/Gems`目录中。有关 O3DE 中包含的所有 Gem 的列表，请参阅 [Gem 参考](./reference)。

## 章节主题

| 主题 | 说明 |
| --- | --- |
| [Gem 参考](./reference/) | 对 O3DE 中所有可用 Gem 的完整引用。 |
| [Gem 仓库](/docs/user-guide/remote-content/) | 了解如何通过创建和托管 Gem 存储库来共享 Gem。 |
| [核心 O3DE Gems](./core-gems) | 了解 O3DE 中大多数项目所需的核心 Gem。 |
| [Gem 版本控制](./gem-versioning) | 了解 Gem 版本控制和兼容性。 |


## 相关主题

| 主题 | 说明 |
| --- | --- |
| [扩展编辑器教程](/docs/learning-guide/tutorials/extend-the-editor/) | 通过在 C++ 或 Python 中创建自定义工具 Gem 来扩展 **O3DE 编辑器** 的教程。 |


## 常见工作流

### 将 Gem 添加到项目

Gem 为您的项目提供模块化功能和资源。您可以添加标准 O3DE Gem、第三方 Gem 或自定义 Gem。

根据您的项目配置，您的项目默认启用一组 Gem。O3DE 构建系统检测已启用的 Gem 并将其设置为项目的依赖项。您可以在`<project>/Code/`目录的`enabled_gems.cmake`文件中找到项目中启用的 Gem 列表。

以下是向项目添加 Gem 的高级概述：

1. 注册 Gem（如果尚未注册）。与 O3DE 捆绑在一起的标准 Gem 已注册。但是，您必须注册从外部源获取的 Gem。请参阅 [将 Gem 注册到项目](/docs/user-guide/project-config/register-gems/)。

1. 添加和删除 Gem。您可以在项目中添加和删除已注册的 Gem。请参阅 [在项目中添加和删除 Gem](/docs/user-guide/project-config/add-remove-gems/)。

### 创建自定义 Gem

您可以创建资产 Gem 或代码 Gem。资产 Gem 仅包含资产的集合。代码 Gem 包含用于扩展 Editor 或为项目开发功能和游戏逻辑的代码和资产。

以下推荐主题可帮助您创建资产 Gem 或代码 Gem：

1. [O3DE中的Gem模块系统](/docs/user-guide/programming/gems/overview/): 在本节中，了解 Gem 模块、模块管理器以及 O3DE 如何加载和初始化 Gem。

1. [创建O3DE Gem](/docs/user-guide/programming/gems/creating/): 资产 Gem 和代码 Gem 具有相同的目录结构，您可以以相同的方式创建它们。本主题演示如何创建 Gem 以及 Gem 的目录结构中包含哪些内容。

1. [代码 Gem 规范](/docs/user-guide/programming/gems/code-gems/): 了解如何创建代码 Gem 并了解其关键组件。
