---
linkTitle: 前往官方 O3DE 远程存储库
title: 为官方 O3DE 远程存储库做出贡献
description: O3DE 为 O3DE 拥有的内容维护一个官方远程存储库。
weight: 500
toc: true
---

O3DE 为 O3DE 拥有的内容维护一个官方远程存储库。 仓库的`repo.json` 在 https://github.com/o3de/canonical.o3de.org 进行管理，任何更改都是使用 GitHub 拉取请求进行的。

{{< note >}}
只有 O3DE 拥有的 GitHub 存储库中 O3DE 拥有的内容才能添加到官方远程存储库。 在提交内容以包含在官方远程存储库之前，请遵循 [贡献者指南](/docs/contributing/to-code/git-workflow/) 将您的内容添加到 O3DE GitHub 存储库，例如`o3de-extras`。
{{< /note >}}

#### 远程仓库内容审批流程：
1. 从 https://github.com/o3de/canonical.o3de.org 分支的本地克隆创建分支
1. 使用 `o3de edit-repo-properties` 命令在本地对 `repo.json` 文件进行更改，并将更改推送到您的复刻。
1. 创建一个拉取请求，将您的更改合并到 https://github.com/o3de/canonical.o3de.org 的`development`分支中，然后按照拉取请求模板中的说明进行操作，该模板将要求您提供一些必需的信息。
1. 将运行自动和人工审核以检查您的更改。
1. 审核通过后，更改将被合并，并且可以使用暂存 URL 测试内容。
1. 测试完成后，更改将合并到 `main` 分支，使内容可供所有用户使用。
