---
linkTitle: 远程内容
title: Open 3D Engine 中的远程内容
description: 您可以在远程存储库中共享和下载 Gem、Projects 和 Templates。
weight: 2000
---


**Open 3D Engine （O3DE）** 包括一种内置方式，可以使用 **Remote Repositories** 共享和下载 **Gem**、**Projects** 和 **Templates**，这些存储库可以位于您的本地计算机、Web 服务器或 Git 存储库中，就像 [GitHub](https://github.com) 中托管的那些一样。 **Project Manager** 和 **O3DE CLI** 可以显示和下载来自**Remote Repositories**的内容，并帮助您创建自己的 **Remote Repositories** 并向其添加内容。

官方的 **O3DE Remote Repository** 是 [https://canonical.o3de.org](https://canonical.o3de.org/repo.json)，它包含在引擎根目录的  [`engine.json`](https://github.com/o3de/o3de/blob/development/engine.json) 文件中，默认情况下对所有用户都可用。 对列出所有可用下载内容的 https://canonical.o3de.org/repo.json 文件的更改是通过 https://github.com/o3de/canonical.o3de.org GitHub 存储库中的拉取请求进行的。

## 本部分主题

|主题 |描述 |
| --- | --- |
| [使用 O3DE 远程存储库](use-a-remote-repository) | 了解如何从现有 O3DE 远程存储库添加和下载内容。 |
| [远程存储库概述](remote-repository-overview) | 了解 O3DE 远程存储库结构和使用案例。 |
| [创建 O3DE 远程存储库](create-a-remote-repository) | 了解如何创建您自己的 O3DE 远程存储库。|
| [Repo.json 参考](repo-json-reference) | 了解 repo.json 文件架构及其使用方法。 |
| [更新 O3DE 远程存储库](update-a-remote-repository) | 了解如何在 O3DE 远程存储库中添加、删除和更新内容。 |
| [为官方 O3DE 远程存储库做出贡献](/docs/contributing/to-official-remote-repository) | 了解如何将内容提交到官方 O3DE 远程存储库。 |
