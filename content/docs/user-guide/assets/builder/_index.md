---
linkTitle: Python Asset Builder
title: 使用 Python 资产生成器定制资产处理流程
description: 利用 Python Asset Builder 在 Open 3D Engine 中使用 Python 处理资产。
weight: 600
toc: true
---

利用**Python Asset Builder**，您可以为 Maya 和 Houdini 等内容创建工具或任何已知文件格式的源资产类型制作的资产创建自定义流程作业。

要使用 Python 资产生成器，必须启用 [Python Asset Builder Gem](/docs/user-guide/gems/reference/script/python/python-asset-builder)。

## Python Asset Builder 实现

Python 资产生成器遵循与 [Asset Builders](../pipeline/asset-builders)相同的设计模式和功能。Python 资产生成器接收源资产并生成运行时优化的产品资产，这些资产存储在**资产缓存**中。Python 资产生成器有三个部分：

* **Descriptor** 是一个类，可为 ** 资产处理器**提供可处理资产类型的构建程序 ID 和文件模式。
* **Create Jobs** 提供了一个`CreateJobsRequest`处理程序，可生成一个 `CreateJobsResponse`。该响应包含 Asset Processor 用来为 Python Asset Builder 队列作业进程的信息。
* **Process Job** 提供了一个 `ProcessJobRequest` 处理程序，可生成  `ProcessJobResponse`并生成产品资产。该响应包含资产处理器在资产缓存中放置产品资产所需的信息，以及用于跟踪产品资产及其产品依赖关系的信息。

## 编写一个Python Asset Builder

创建 Python 资产创建器有四个步骤：

* [添加启动脚本](bootstrap) - 将 Python Asset Builder 脚本添加到引导位置。
* [添加描述符](descriptor) - 添加描述符，该描述符提供资产创建器 ID 和文件模式，并为作业创建和处理注册处理程序。
* [实现创建任务](create-jobs) - 在 `CreateJobs` 的回调方法中定义响应作业信息的逻辑。
* [实现处理任务](process-job) - 为 `ProcessJob` 定义生成产品资产文件和依赖关系的逻辑。
