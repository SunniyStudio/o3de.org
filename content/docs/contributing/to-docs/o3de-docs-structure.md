---
linkTitle: O3DE 文档结构
title: O3DE 文档结构
description: Open 3D Engine （O3DE） 文档存储库结构指南。
weight: 400
---

**Open 3D Engine （O3DE）** docs 目录结构中有许多文件夹和文件。如果您是新的贡献者，那么整理在哪里可以找到或放置您想要贡献的主题和图像可能是一个难题。当您了解两个关键的高级目录时，该结构更易于导航。

* `/content/docs/`: Markdown (`.md`) 文件的根目录，这些文件构成了各种 O3DE 指南的文档。

* `/static/images/`: 各种 O3DE 指南中使用的图像(`.png`, `jpg`, 和 `.svg`)文件的根目录。

绝大多数内容贡献将位于上述两个目录中的某个位置。每个目录下的结构都是相同的，并且易于导航。

{{< note >}}
`/static/images/`下的目录结构反映了`/content/docs/`下的目录结构。将图像添加到主题时，请确保将它们放在适当的目录中。在某些情况下，您可能需要在`/static/images/`中创建新目录以复制 `/content/docs/` 的结构。
{{< /note >}}

在`/content/docs/` 和 `/static/images/`下面的结构中，目录映射到各种 O3DE 文档指南。您很可能只对几个特定领域感兴趣。下图突出显示了这些感兴趣的领域：

![O3DE directory structure diagram.](/images/contributing/to-docs/o3de-directory-structure.svg "O3DE important directories.")

The highlighted directories in the above diagram are where most contributions will be made:

* `atom-guide`: [**Atom Render**](/docs/atom-guide/) 部分。Atom 指南包含 Atom Renderer 独有的 Atom 功能和参考文档。各种 O3DE 指南中介绍了 Atom 和 O3DE 之间的联系以及在 O3DE 上下文中使用 Atom 的主题。

* `engine-dev`: [**引擎开发人员指南**](/docs/engine-dev/)部分。引擎开发人员指南包含有关 O3DE 的内部体系结构、设计原则和执行流程的信息，供对 O3DE 项目进行自定义引擎修改和其他贡献的开发人员使用。您可以在本指南中找到有关 O3DE 框架、Gem 和工具的技术信息。

* `learning-guide`: [**教程和示例**](/docs/learning-guide/) 部分。教程和示例包含用户教程、说明书和示例文档。教程应仅使用属于 O3DE 发行版的资产，这些资产是免费提供的，或者可以在开源内容创建工具中快速轻松地复制的资产。Cookbook 部分包含目标 *配方*，用于描述和演示如何执行特定任务，例如，使用 Script Canvas 图形的片段。Cookbook 是 O3DE 用户快速、有用贡献的绝佳场所。Samples 部分包含 O3DE 发行版中包含的示例的文档。

* `user-guide`: [**用户指南**](/docs/user-guide/) 部分。用户指南包含属于 O3DE 分发的工具、Gem、组件和系统的功能和参考文档。

* `welcome-guide`: The [**欢迎**](/docs/welcome-guide/) 部分。欢迎部分包含 O3DE 概述、安装指南、帮助新用户入门的教程以及指向 O3DE 用户的各种中心的链接。

{{< note >}}
上述每个目录下的结构都反映在每个指南左侧显示的目录中。在规划和添加新主题时，请记住这一点。
{{< /note >}}

其余目录不太可能引起单个贡献者的兴趣，因为它们的主题是通过其他过程生成的，或者因为它们的主题与使用 O3DE 没有直接关系。


* `api`: 映射[**API 参考**](/docs/api/) 部分。O3DE 的 API 参考是根据 O3DE 源代码中的 Doxygen 格式注释生成的。生成的内容放置在 `/static/docs/api` 目录中。

* `contributing`: 映射[**贡献**](/docs/contributing/) 部分。这是有关为 O3DE 代码和文档做出贡献的所有信息。您在这里。

* `release-notes`: The [**发布日志**](/docs/release-notes/) 部分。发行说明包含由 O3DE 特别兴趣小组提供的 O3DE 版本特定信息。

* `tools-ui`: [**工具 UI 开发人员指南**](/docs/tools-ui/) 部分。本指南包含有关用于为 O3DE 创建工具和应用程序的设计概念、框架和 UI 小组件的信息。
