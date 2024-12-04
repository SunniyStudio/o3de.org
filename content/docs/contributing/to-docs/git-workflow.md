---
linkTitle: Git Runbook
title: "O3DE 文档贡献: Runbook"
description: 开始使用 Open 3D Engine （O3DE） 文档存储库时要采取的操作清单。
weight: 500
toc: true
---

## 快速入门：O3DE 文档贡献的运行手册

| 步骤 | 操作 |笔记|
|-|-|-|
|  \#1       | 将 [O3DE docs repo](https://github.com/o3de/o3de.org) 分叉到您的个人 GitHub 帐户。|需要 GitHub 帐户。注册 [这里](https://github.com/join). |
|  \#2       | 将 O3DE docs 存储库的 _your_ 分支克隆到本地工作站。 | 确保您记住本地工作站上克隆的存储库的文件路径。什么是“克隆”？阅读我们的 [详细的 O3DE 文档入门贡献指南](./get-started)! |
 |  \#3       | `git remote add upstream  https://github.com/o3de/o3de.org` | 将 _upstream_ 设置为 O3DE 文档存储库。_origin_ 是您的远程分叉。最终，您将把更改推送到 _origin_，然后从您的分支 （_origin_） 向 O3DE 文档存储库 （_upstream_） 提交拉取请求 （PR）。对你推送的分支的任何后续更改推送都将反映在你从 _origin_ 到 _upstream_ 所做的 PR 中——只要你的分支的 PR 保持打开状态，你就不需要创建另一个从 _origin_ 到 _upstream_ 的 PR。  |
 |  \#4       | `git fetch upstream` | 这将更新有关上游存储库中可用内容的本地 Git 信息。 |
 |  \#5       | `git switch -c development upstream/development` | 将本地开发分支设置为跟踪上游开发分支作为其远程存储库。这确保了你的本地`development`始终与`git pull`之后对`development`的最新更改保持同步。 |
 |  \#6       | `git switch -c your-new-branch-name-here upstream/development` | 从 `development` 创建一个新的工作分支。请给它起一个有用且易于理解的名称，表明它的范围和生命周期，例如 `style-guide-updates`。作为最佳实践，请从 `development` 为每个潜在的 PR 创建一个新分支，并尝试在该分支中确定您的工作范围，以便于社区审查。 |
 |  \#7       | 进行更改并在本地确认。 | |  
 |  \#8       | 对你编辑的每个文件运行`git add`。|将文件添加到本地 Git 暂存。你可以使用 `git add .`来添加你编辑过的每个文件，但要小心。不要添加 *任何* 不是您提交以供审核的工作的文件！ |
 |  \#9       | 使用 `git status` 验证要提交的文件列表。 | 在提交更改之前，最好确保没有暂存任何不打算包含在 PR 中的更改。 使用 `git restore --staged your-filename-here`以从暂存中删除修改后的文件。 |
 |  \#10      | `git commit -s -m "useful-commit-message-here"` |将您的更改写入分支历史记录，为提交做准备。 `-s` 参数将向你的提交添加一个 `Signed-off-by: user.name <user.email>"` 形式的 DCO 签名，其中 _user.name_ 和 _user.email_ 分别是 `.gitconfig` 文件中的 `user.name` 和`user.email`。你的提交信息应该清楚地评估你所做的工作。 |
 |  \#11      | `git push origin` | 将你的提交推送到你的远程 fork (_origin_). |
 |  \#12      | 要创建 PR，请使用作为 `git push` 输出一部分的 URL，或者转到 [GitHub](https://github.com) 上的远程分支，并从您的工作分支创建一个 PR 到 [O3DE docs repo](https://github.com/o3de/o3de.org)的开发分支。 | 确保 PR 中的 _base_ 以 o3de.org 的 development 分支为目标。 <br> ![Base branch is set to development in most PRs](/images/contributing/to-docs/pr-base-target.png) |
 |  \#13      | 在 O3DE docs repo 上查看你的新 PR。|您的 PR 将是自动分配的审阅者。请留意评论和请求的更改，并及时回复它们，以免 PR 变得“过时”。O3DE 社区中的某个人将在您处理完所有审核建议后合并您的更改。 |

