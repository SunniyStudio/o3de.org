---
linktitle: 入门
title: 开始为 Docs 做贡献
description: 通过设置本地存储库、设置编写环境和提交文档，开始为 Open 3D Engine （O3DE） 文档做出贡献。
toc: true
weight: 200
---

本部分将帮助您开始为 Open 3D Engine （O3DE） 文档做出贡献。您将学习如何设置您的写作环境，以及将文档从文本编辑器转换为官方 O3DE 文档所需的过程。

O3DE 文档使用 [fork and pull model](https://en.wikipedia.org/wiki/Fork_and_pull_model) 进行贡献。作为贡献者，您在 GitHub 上维护一个 fork（您自己的 O3DE 文档存储库），并在本地编辑文档。然后，您从 fork 提交 PR 以供审核。

{{< note >}}
本页中的步骤使用终端运行 Git 命令。您可以直接在 GitHub 上完成这些步骤，并参考 GitHub 的 [Repositories](https://docs.github.com/en/repositories) 文档，或通过您选择的替代工具完成这些步骤，例如 [GitHub Desktop](https://desktop.github.com/)或其他客户端。

无论您选择哪种方法，请确保您正在完成本页中概述的正确操作。
{{< /note >}}

## 先决条件

* 在此处注册 GitHub 帐户 [加入 GitHub](https://github.com/join?ref_cta=Sign+up)。

* 安装 **Git** 版本控制软件。在此处获取 Git [Git 下载](https://git-scm.com/downloads)。

* 安装用于更改 Markdown (`.md`) 文件的编辑器。你可以使用任何编辑器，但我们建议使用支持 Markdown linting 的编辑器。VS Code 通常由贡献者使用，您可以在此处获取它 [Microsoft VS Code](https://code.visualstudio.com/download)。


## 设置本地 o3de.org 仓库

在本节中，您将学习如何创建自己的 O3DE 文档分支。


### 创建一个 fork

*fork* 是你在 GitHub 上  `o3de.org` 源存储库的自己的副本。您可以在 fork 中执行任何您喜欢的操作，但建议您保持 fork 与源存储库同步，并在 fork 的分支内工作。以这种方式工作可以确保 `o3de.org`的完整性，并使您能够轻松地按照自己的节奏工作、尝试更改并与其他贡献者协作。

要创建分叉，请执行以下步骤：

1. 转到 [O3DE 文档存储库](https://github.com/o3de/o3de.org).

1. 分叉 `o3de.org` 。选择页面右上角的 **Fork** 按钮。有关创建复刻的更多信息，请参阅 GitHub 文档，[复刻存储库](https://docs.github.com/en/get-started/quickstart/fork-a-repo).

您可以在仓库中或通过转到 `https://github.com/<your-username>/o3de.org` 来访问您的复刻。
有关使用复刻的更多信息，请参阅 GitHub 文档，[使用克隆](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/working-with-forks)。


### 克隆你的 fork

*克隆* 是创建存储库的本地副本的过程。要创建分叉的克隆，请在终端中执行以下步骤：

1. 在分叉的网页上，选择绿色的 **Code** 按钮并复制 HTTPS URL。
 
    {{< note >}}
虽然不需要 SSH 协议来参与文档，但您可以选择使用 SSH URL 而不是 HTTPS 进行克隆。如果是这样，则必须完成其他步骤。有关更多信息，请参阅 GitHub 文档 [使用 SSH URL 克隆](https://docs.github.com/en/get-started/getting-started-with-git/about-remote-repositories#cloning-with-ssh-urls)。
    {{< /note >}}

1. 在终端中，导航到要放置本地存储库的根目录，然后在该目录中克隆它。

    ```shell
    cd <root-directory>
    git clone <https://github.com/<your-username>/o3de.org.git> 
    ```

### 为您的 clone 设置上游

您的克隆必须指向两个远程存储库：您的分支 （origin） 和源存储库 （upstream）。默认情况下，克隆的`origin` 已经指向你的 fork。但是，您必须将 `upstream` 设置为指向 O3DE 文档的源存储库 `o3de/o3de.org`。设置 upstream 允许你将新的更改拉取到你的 fork 和 `o3de.org:main` 中。稍后，你还需要以 PR 的形式向上游提交更改。PR 是你提交更改的唯一方式，你不能直接推送到上游。

在终端中，执行以下步骤：

1. 将克隆的上游设置为源 O3DE docs 存储库。

    ```shell
    git remote add upstream https://github.com/o3de/o3de.org.git
    ```

1. 禁止从克隆推送到源 O3DE docs 存储库。

    ```shell
    git remote set-url --push upstream NONE
    ```
有关此步骤的更多信息，请参阅 GitHub 文档 [为 fork 配置远程](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/working-with-forks/configuring-a-remote-repository-for-a-fork)。


### 分支

`o3de.org` 存储库中有两个重要的分支：`main` 和 `development`。根据你贡献的文档，你可以选择将更改贡献给任一分支。

| 分支 | 说明 |
| --- | --- |
| `main` | 如果您正在为 O3DE 稳定版本中已经存在的功能贡献文档，或者[`o3de:main`](https://github.com/o3de/o3de/tree/main)。 |
| `development` | 如果您正在为仍在开发中的功能贡献文档，或者在[`o3de:development`](https://github.com/o3de/o3de/tree/development)。 |

提前决定要将文档贡献给哪个分支。了解这一点很重要，因为它可以帮助您保持本地更改与正确的上游分支同步，尤其是在您执行以下操作之一时：
- 创建新分支
- 同步您的分支
- 创建 PR


### 同步您的克隆

在整个编写过程中，在你的克隆中，你必须将你正在工作的任何分支与适当的上游分支 `main` 或 `development` 同步。这可确保您的分支与最新提交保持同步，并可以帮助您在创建 PR 时避免出现问题。最好经常执行此步骤，例如在创建新分支或创建 PR 之前。

要同步您的分支：

1. 签出要同步的本地分支。

    ```shell
    git checkout <branch-name>
    ```

1. 从上游获取最新的提交，您在上一节中将其设置为指向远程源 `o3de.org`。

    ```shell
    git fetch upstream
    ```

1. 将最新提交从适当的上游分支拉取到本地分支中。

    ```shell
    git merge upstream/[main|development]
    ```

或者，以下 [`git pull`](https://git-scm.com/docs/git-pull/2.22.0)  命令获取并合并：
```shell
git pull upstream [main|development]
```

作为工作流程示例，您可能希望使 fork 的 `main` 分支与源 `o3de.org:main` 分支保持同步。执行此操作的操作集如下所示：

```shell
git checkout main
git fetch upstream
git merge upstream/main
git push origin main
```


## 写作过程

本节介绍写入过程的技术细节，例如设置写入环境。

### 创建分支

您的所有工作都应该在分支中完成。您可以从 clone 中的分支提交到 fork。分支可帮助您划分您的贡献，并使其他贡献者可以轻松地与您协作。

如前所述，将分支与上游分支同步可使分支保持最新状态，并有助于防止在提交更改时出现摩擦。您想要与 `o3de.org:main` 或 `o3de.org:development` 同步。

要创建已与最新版本同步的分支，请执行以下操作：

1. 从上游获取。

    ```shell
    git fetch upstream
    ```

1. 创建一个指向最新 `upstream/main` 或 `upstream/development` 的分支，然后切换到该分支。

    ```shell
    git switch -c <branch-name> upstream/[main|development]
    ```

    {{< note >}}
命名分支时，我们建议使用短短的短划线分隔名称，以清楚地表示分支的内容。例如 `camera-follow-tutorial`。
    {{< /note >}}


1. 上一步仅在克隆中创建分支。您必须将分支推送到 fork，以便它显示在 GitHub 上的 fork 中。

    ```shell
    git push origin <branch-name>
    ```

### 同步、写入、添加、提交和推送

在 PR 过程中迭代编写文档并进行更改时，您将循环执行以下 Git 操作：

1. **`git fetch upstream`** 和 **`git merge upstream/[main|development]`** -- 使用最新提交使分支保持最新状态。
1. 使用文本编辑器编写文档。确保您的文档在技术上准确无误，并遵循 [O3DE 风格指南](./style-guide/)。
1. **`git add`** -- 将文件添加到本地 Git 暂存。不要添加 *任何* 不是您提交以供审核的工作的文件。
1. **`git commit -s -m "<message>"`** --  将您的更改写入分支历史记录，为提交做准备。`-s` 为您的提交签署 DCO。您的初始提交消息应引用相应的 GitHub 问题，并对您所做的工作进行清晰的评估。
1. **`git push origin <branch-name>`** -- 将你的提交推送到你的远程 fork(_origin_). 

根据需要重复这些步骤。完成编写后，您的每个提交都应该有一个 DCO 签核，并且仅包含您所做的更改。提交必须推送到您的远程 fork。稍后，您将提交 PR 以将您的更改合并到远程源。 


### DCO 签署您的提交

DCO 代表 [*开发者原产地证书 （DCO）*](https://github.com/apps/dco)。DCO 签字是您的证明，证明您的贡献是您自己的原创作品，或者您有权提交该作品。在每次提交时，您必须使用`-s` 或 `--signoff` 添加 DCO sign-off。包含未签署的提交的 PR 将不会被审查或合并。DCO 签核很容易做到，也很容易忘记。

DCO 签名以`Signed-off-by: user.name <user.email>"` 的形式出现在提交消息的最后部分，其中 _user.name_ 和 _user.email_ 分别是`.gitconfig`文件中的`user.name` 和 `user.email`。

{{< important >}}
在某些情况下，你可能需要在你的提交信息中手动添加一行`Signed-off-by:`，例如，如果你使用的 GUI 工具不支持 DCO signoff。
对于 2017 年 7 月 18 日之前创建的 GitHub 帐户，请使用`username@users.noreply.github.com`作为电子邮件地址。对于在该日期之后创建的帐户，请使用 GitHub 提供的 no-reply 电子邮件地址。新的未回复电子邮件地址是一个七位数的 ID 号和`ID+username@users.noreply.github.com`形式的用户名，可以在 GitHub 帐户设置的电子邮件选项卡中找到。 有关设置提交电子邮件地址的更多信息，请参阅 [设置提交电子邮件地址说明](https://docs.github.com/en/account-and-profile/setting-up-and-managing-your-personal-account-on-github/managing-email-preferences/setting-your-commit-email-address).
{{< /important >}}

### 预览您的文档

在你写作的时候，你应该预览你的文档，以确保它的格式正确。由于 o3de.org 网站基于 Hugo 构建，因此预览文档的最佳方式是在本地计算机上运行 Hugo。有关如何设置和使用 Hugo 的信息，请参阅 o3de.org [`README.md`](https://github.com/o3de/o3de.org#readme)文件。

此外，预览文档的一种快速方法是使用 VS Code 的 Markdown 的 [编辑器和预览同步](https://code.visualstudio.com/docs/languages/markdown#_editor-and-preview-synchronization)功能。此功能仅限于 VS Code 的 Markdown 支持，因此它不会呈现 Hugo 独有的任何功能，例如短代码。

最后，当文档位于 PR 中时，您可以查看和共享文档的预览。Netlify Web 部署服务为每个 PR 创建一个预览。创建 PR 后，这需要几分钟来部署，并且每次推送提交时都会刷新。您可以在 PR 网页底部访问已部署的预览版：找到 **netlify/o3deorg/deploy-preview — Deploy Preview ready！** 并单击 **详细信息**。


## 提交文档

当您进行更改或创建新文档时，必须先创建拉取请求 （PR） 以供审核，然后才能将更改合并到 `o3de.org`中。PR 允许同行贡献者审查几个潜在问题的贡献，包括技术准确性、拼写、语法、清晰度和风格。

当前活动 PR 的列表在这里 [O3DE 存储库拉取请求 （PRs）](https://github.com/o3de/o3de.org/pulls)。


### 创建 PR

PR 是在分支的 GitHub Web 界面中创建的。转到 GitHub 上的分支并执行以下步骤。

1. 在 **Pull Request** 选项卡中，选择绿色的 **New pull request** 按钮以创建新的 PR。

1. 在 **Comparing changes** 页面中，您将 fork 的分支与远程源的分支进行比较。确保你正在比较的 **base** 指向 `o3de/o3de.org:main` 或 `o3de/o3de.org:development`。**head repository** 应该是您的复刻。然后，在 **compare** 下拉列表中，选择要提交的分支。

1. 验证更改的文件是否仅包含要提交的更改。只应列出您所做的提交。如果提交数量超出预期，请在此处停止，并确保使用前面解释的 [迭代步骤](#sync-write-add-commit-and-push) 中的 fetch、merge 和 push 命令正确同步您的分支。

1. 选择绿色的 **Create pull request** 按钮以打开 **Open a pull request** 页面。

1. 添加描述性标题和说明。如果 PR 正在解决现有问题，请务必在 PR 的标题中包含问题号。

1. 在 **审阅者**中，添加 **o3de/docs-reviewers**，以便他们可以审阅您的 PR。根据需要添加其他审阅者。如果您要提交技术文档，请添加可以验证文档技术准确性的审阅者。

1. 选择 **Create pull request** 以提交 PR。

如果您转到 GitHub 上的主 o3de.org 存储库并参考 [O3DE 存储库拉取请求 （PR）](https://github.com/o3de/o3de.org/pulls)，您的新 PR 将显示在列表顶部。

有关创建拉取请求的更多信息，请参阅 [创建拉取请求](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/creating-a-pull-request)。

### 回复反馈

反馈的形式是评论，你可以通过编辑本地分支中的文件来解决这些评论，以及可以从 GitHub PR 界面提交的*建议*。重要的是要了解 PR 过程是一个协作讨论。每条评论都不需要处理，每个建议都不需要整合。当所需数量的审阅者批准您的贡献时，可以对其进行集成。以下是处理建议和评论的一些提示：

* 要提交多个建议，请使用批处理功能将多个建议集成到单个提交中。
* 请尽量确认您收到的所有反馈。这并不意味着整合每条评论并提交每条建议。它只是意味着避免在没有行动或回应的情况下解决评论和建议。拥抱协作。
* 在处理评论时，在 PR 中保持相对对话，根据需要编辑主题，并将更改提交到你的 fork/branch。你的新提交将自动添加到 PR 中。
* 如果需要，请务必请求重新审核您的新提交。

有关合并 PR 反馈的更多信息，请参阅 [在拉取请求中合并反馈](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/reviewing-changes-in-pull-requests/incorporating-feedback-in-your-pull-request)。

### PR 合并

当你获得至少一名文档审核人和至少一名技术审核人的批准时，你的 PR 可以合并。永远不要合并你自己的 PR。Docs 维护者负责合并 PR。
