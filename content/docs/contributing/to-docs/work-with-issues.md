---
linkTitle: 处理问题
title: 处理 O3DE 文档问题
description: 创建和处理 Open 3D Engine （O3DE） 文档项目中的问题的指南。
toc: true
weight: 300
---

为 Open 3D Engine （O3DE） 文档做出贡献的最简单方法是在 GitHub 上提交 *issue*。在 O3DE 文档中，议题用于跟踪文档错误、新文档请求、建议和改进。您还可以搜索现有的 O3DE 文档问题，以查找要解决和评论的任务。要查看 O3DE 文档问题的当前列表，请参阅 [o3de.org 问题](https://github.com/o3de/o3de.org/issues)

请参阅 [GitHub issues](https://guides.github.com/features/issues/) 了解 GitHub 问题的概述。

## 搜索 O3DE 文档问题

在创建新问题之前，请始终搜索现有问题，以避免重复问题。您可以使用 [问题列表](https://github.com/o3de/o3de.org/issues) 顶部的搜索字段搜索 O3DE 文档问题。如果没有任何筛选条件，搜索将返回议题和拉取请求 （PR）。使用`is:issue` 和 `is:open`筛选结果以仅显示未解决的问题。您可以使用左侧的 **Filters** 列表来选择一些快速过滤器，例如您的问题或提及您的每个问题。您可以使用 `"` 括起来的字符串来搜索期刊中的特定文本。最重要的过滤器之一是 `label:`。O3DE 文档问题通常具有多个标签，以便更轻松地查找特定类型的问题。请参阅以下示例：

[Search results, good first issue](https://github.com/o3de/o3de.org/issues?q=is%3Aopen+is%3Aissue+label%3Agood-first-issue)

在上面的搜索结果中，搜索字符串 `is:open is:issue label:good-first-issue` 返回所有带有 `good-first-issue` 标签的未解决的 O3DE 文档问题。这些问题已被确定为新贡献者的良好切入点。

有关在 GitHub 上搜索问题的完整文档，请参阅 [搜索问题和拉取请求](https://docs.github.com/en/github/searching-for-information-on-github/searching-issues-and-pull-requests)。

## O3DE文档问题标签

O3DE 文档使用标签来组织问题和拉取请求。有关当前标签列表，请参阅 [O3DE 文档标签](https://github.com/o3de/o3de.org/labels)。

标签由 O3DE 文档和社区特别兴趣小组 （D&C SIG） 提供。并非列表中的所有标签都适用于议题，也不是每个议题都必须有标签。也就是说，如果您发现列表中的一个或多个标签适用于某个问题，我们鼓励您添加它们！标签使查找问题和确定问题的优先级变得更加容易，并且可以帮助贡献者更快地解决错误和响应请求。

尽管有数十个标签可用，但下表中显示了一些特别感兴趣的标签。

|标签 |用法 |未解决的问题 |
| - | - | - |
| `good-first-issue` | 对新贡献者来说是个好问题。 | [Good first issues](https://github.com/o3de/o3de.org/issues?q=is%3Aopen+is%3Aissue+label%3Agood-first-issue) |
| `tutorial` | 请求新教程。 | [Tutorial issues](https://github.com/o3de/o3de.org/issues?q=is%3Aopen+is%3Aissue+label%3Atutorial) |
| `enhancement` | 请求新的文档和网站功能。 | [Enhancement issues](https://github.com/o3de/o3de.org/issues?q=is%3Aopen+is%3Aissue+label%3Aenhancement+) |

## 创建 O3DE 文档问题

在理想情况下，提交到存储库的每个 PR 都将有一个关联的问题。根据问题进行工作可以提高 PR 提交的质量和 PR 审查的速度，因为在完成任何工作之前，可以通过讨论来澄清和完善问题。有关在 GitHub 上创建问题的信息，请参阅 [创建问题](https://docs.github.com/en/github/managing-your-work-on-github/creating-an-issue)。

创建新问题时，请记住以下几点：

* 为问题提供一个简洁的标题，以清楚地标识请求。

* 每个问题都应该针对一个请求。如果请求足够大，例如主要新功能或网站部分，则可以将其拆分为分类。

* 提供您认为与解决问题相关的尽可能多的信息。包括站点的 URL 和用于呈现问题的浏览器版本等内容。

* 使用适当的标签。正确的标签有助于分类，并使贡献者更容易找到适合其技能的内容。

* 对于技术问题，包括重现问题的步骤以及可能相关的信息，例如您的浏览器版本。

* 回复对您创建的问题的评论。提供其他信息并积极参与贡献者有助于更快地解决您的问题。

## 分配 O3DE 文档问题

如果您在列表中看到要解决的问题，可以将其分配给自己。要将问题分配给自己，请转到问题描述，然后在右侧的 **Assignees** 组中选择 **assign yourself**。如果问题已经有受理人但没有最近的活动，请在对问题分配进行任何更改之前通过评论问题来请求状态更新。
