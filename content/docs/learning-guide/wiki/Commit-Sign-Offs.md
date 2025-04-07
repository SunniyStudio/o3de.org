---
title: "提交签字"
description: ""
toc: false
---

对于所有 O3DE 和相关代码，必须先签署提交，然后才能作为 Linux 基金会的代码库要求进行合并（有关详细信息，请参阅 [此 SO 帖子](https://stackoverflow.com/questions/1962094/what-is-the-sign-off-feature-in-git-for))。要签署提交，只需在 `git commit` 命令中添加 `-s` 命令行选项。这将添加一行，如下所示：

```
Signed-off-by: Humpty Dumpty <humpty.dumpty@example.com>
```

在提交正文中。如果你忘记这样做，你可以通过修改每个缺少 sign off 的提交的提交消息来补救。有许多机制可以做到这一点，但最简单的方法是变基受影响的提交范围，并强制将分支推送到 fork。失败的 DCO 说明说明了如何执行此作，以及在什么条件下可以使用 rebase 进行批量签核更正。这些说明转载如下：

```
变基分支
如果你有一个本地 git 环境并满足以下条件，一个选项是变基分支并在新提交中添加你的 Signed-off-by 行。
请注意，如果其他人已经根据此分支中的提交开始工作，则此解决方案将重写历史记录并可能导致严重问题
for collaborators ([described in the git documentation](https://git-scm.com/book/en/v2/Git-Branching-Rebasing) under "The Perils of Rebasing").

只有在以下情况下，您才应执行此作：

您是此分支中提交的唯一作者
您绝对确定没有其他人基于此分支执行任何工作
分支中没有空提交（例如，使用 --allow-empty 添加的 DCO 补救提交）
要将 Signed-off-by 行添加到此分支中的每个提交中：

通过 [通过命令行在本地签出拉取请求](https://help.github.com/en/github/collaborating-with-issues-and-pull-requests/checking-out-pull-requests-locally).
在本地分支中，运行：git rebase HEAD~10 --signoff
强制推送您的更改以覆盖分支：git push --force-with-lease origin class_inheritance
```
