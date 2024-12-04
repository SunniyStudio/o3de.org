---
linktitle: GitHub 工作流
title: O3DE 代码贡献 GitHub 工作流
description: 通过 GitHub 向 Open 3D Engine （O3DE） 贡献代码的概述和说明。
toc: true
weight: 200
---

想要向 Open 3D Engine （O3DE） 提交新的或更改的代码？令人兴奋的！按照以下指南提交您的第一个 PR。

### GitHub 代码贡献工作流程

O3DE 基础存储库位于 GitHub 上，网址为  [https://{{< links/o3de-source >}}](https://{{< links/o3de-source >}})。

![GitHub code contribution workflow diagram](/images/contributing/to-code/code-git-workflow.png)

概括地说，工作流是：

1. 从分叉的本地克隆创建分支，执行您的工作，将其推送到您的分叉（源），然后将分叉的拉取请求提交到 O3DE GitHub 存储库（上游）。

2. 检查您的拉取请求是否存在合并冲突。如果找到一个，则拉取请求将被拒绝。

3. 如果没有合并冲突，则会触发自动审查 （AR），并将拉取请求标记为代码审查。

4. 如果拉取请求通过了代码审查，SIG 维护者（或代表）会将其合并到 O3DE GitHub 仓库的 `development` 分支中。

### 初始 Git 贡献工作流步骤

1. 在你自己的 GitHub 账户中创建`https://{{< links/o3de-source >}}.git`的分支。为此，请转到 O3DE 公共 GitHub 存储库 [https://{{< links/o3de-source >}}](https://{{< links/o3de-source >}})，然后选择右上角的“分叉”按钮创建分叉。这会将 O3DE 公有存储库克隆到您的存储库中，并且可能需要几分钟时间。你的 fork 的 URL 将类似于`https://github.com/<YOUR GITHUB NAME HERE>/o3de.git`。

1. 现在，通过打开 GitBash（或支持 Git 的 shell 或实用程序）在本地克隆您的分支。将目录更改为要在其中克隆存储库的文件夹并运行：`git clone https://github.com/<YOUR GITHUB NAME HERE>/o3de.git`。现在，您将在本地桌面上拥有复刻的克隆，并且可以直接处理这些文件。

1. 但是，要简化此工作流程，您必须对本地 Git 配置进行一些更改。在这种情况下，您需要将分叉的 URL 设置为`origin`存储库，将 O3DE 公共存储库设置为`upstream`存储库，并更新 LFS URL。从您本地克隆的 fork 路径运行以下 Git 命令：

    ```bash
    git remote add upstream https://{{< links/o3de-source >}}.git
    ```

    确认 `upstream` 指向 O3DE 公共仓库，而 `origin` 指向你的 fork：

    ```bash
    git remote -v
    ```

    至少，您应该会看到如下所示的输出：

    ```bash
    origin  https://github.com/<FORK>/o3de.git (fetch)
    origin  https://github.com/<FORK>/o3de.git (push)
    upstream  https://{{< links/o3de-source >}}.git (fetch)
    upstream  https://{{< links/o3de-source >}}.git (push)
    ```

    您还可以将上游配置为以特定分支为目标。

    更新 LFS URL 以包含您的复刻。 这将使您能够将更改推送到大文件。 打开存储库根目录下的 .lfsconfig 文件，以获取完整说明和要使用的 **DISTRIBUTION**。

    ```cmd
    git config lfs.url https://<DISTRIBUTION>.cloudfront.net/api/v1/fork/<FORK> 
    ```

    下次拉取或推送时，系统可能会提示您重新进行身份验证。请记住使用 GitHub 个人访问令牌，而不是 GitHub 密码。

    如果以后需要还原此更改，可以运行以下命令：

    ```cmd
    git config --unset lfs.url 
    ```

1. 现在，通过`git fetch`来更新 O3DE 存储库上当前活动的分支，以更新您的本地存储库。你可以用 `git fetch upstream --all`获取所有工作分支，或者用`git fetch upstream <name-of-branch>`获取一个特定的分支。

1. 将提交历史记录变基为上游`development`分支的最后一个提交：

    ```bash
    git rebase upstream/development
    ```

1. 查看你将要处理的分支，并从中 **获取你自己的分支** 来执行你的工作。

    ```bash
    git checkout <name-of-fetched-branch>
    ```

    确认你已经用`git branch`成功切换了分支。如果你在你想要的分支上，从它创建你自己的分支：

    ```bash
    git checkout -b <name-of-your-working-branch>
    ```

### 正在进行的 Git 工作流步骤

现在，您已准备好执行一些工作！在您进行了一些更改并保存了您的工作后，是时候将其作为拉取请求 （PR） 提交以供审核了。

1. （可选）：首先，根据自最初创建分支以来经过的时间，您可能希望将`upstream/development`中的最新内容合并到您的分支中。这将确保您的更改不会与任何其他最近的代码提交冲突，并且还会为您的自动审核 （AR） 提供最大的成功机会，因为您的分支将更接近最新的开发快照。

    您可以使用以下命令执行此操作（**首先使用 `git branch` 检查您位于哪个分支上！**）：

    ```bash
    git fetch upstream --all
    git pull
    git merge upstream/development --signoff
    ```

1. 接下来，暂存（添加）新的或修改的代码文件，提交您的更改，并使用以下命令向您的 fork （origin） 提交拉取请求。（**首先用 `git branch` 检查你所在的分支！**）

    ```bash
    git add .
    git commit -s -m "<commmit_message>"
    git push -u origin <your-branch-name>
    ```

    这会将更新推送到您的 fork，而不是 O3DE 代码存储库。

    {{< note >}}
我们要求对所有代码提交进行 DCO 签名。这要求您在`.gitconfig`文件中同时包含贡献者姓名和电子邮件地址，或者之前已经从支持 Git 的 shell 运行以下 Git 命令：`git config user.name "YOUR CONTRIBUTOR NAME HERE"` `git config user.email "YOUR CONTRIBUTOR CONTACT MAIL HERE"`。（此命令会更新您的`.gitconfig`）你必须在每次提交时使用`-s`选项。如果您使用的是支持 Git 的 IDE，例如 Visual Studio 或 Visual Studio Code，请在首选项中打开提交签名。
    {{< /note >}}

1. （可选）：使用 Jenkins 测试您的分支。

    代码构建管道的源代码和所需的基础设施存储在 O3DE 存储库中。贡献者可以利用它来启动自己的构建/测试管道，也可以在本地进行测试。

    `Jenkinsfile`（AutomatedReview/Jenkinsfile） 是 O3DE 使用的自动审核 （AR）/Jenkins 管道的源。有关如何在本地运行测试的更多信息，请参阅用户指南中的 [模拟自动审核运行](/docs/user-guide/build/configure-and-build/#simulate-an-automated-review-run)。

    用于在构建节点上安装所有依赖项的脚本和其他基础设施设置脚本也存储在存储库中，供贡献者和客户使用。

1. 将 Fork 的拉取请求提交到 O3DE 代码存储库。

    * 导航到 GitHub 中的分支存储库，单击**Pull Requests** 标签页，点击 **New Pull Request**。
    * 在 **Compare** 页面上，验证Base 存储库和分支点是否设置为`O3DE/development`.（这应该是默认设置。）
    * 在 `head` 下拉菜单中，选择您的分支存储库和分支，然后选择 **Create Pull Request**。

    ![Creating a pull request from your fork to o3de/development.](/images/contributing/to-code/code-pr-from-fork.png)

    * 为您的拉取请求添加标题和描述。用尽可能少的字提供清晰的更改范围。
    * 添加审阅者（注意：所需的审阅者和其他 PR 要求将在`CONTRIBUTING.md`文件中最终确定）。

    ![Adding reviewers to an O3DE pull request.](/images/contributing/to-code/code-add-reviewers.png)

    * 现在，点击 **Create pull request**!

1. 受影响组件的 SIG 维护者/审阅者（或代表）审查拉取请求。同时，触发自动审核 （AR）。

    {{< note >}}
SIG 维护者/审阅者将审查拉取请求，并且必须批准 AR 运行才能开始。这是防止管道运行恶意代码所必需的。在拉取请求上触发的 AR 构建在 O3DE 拥有的基础设施上运行。
    {{< /note >}}

1. 一旦所有审查评论都得到解决，SIG 成员将批准拉取请求。

    拉取请求从 SIG 获得所需的批准，并且 AR 通过。

    ![An O3DE contribution pull request in a green approved state.](/images/contributing/to-code/code-pr-accepted.png)

    然后 SIG 维护者（或代表）可以将您的拉取请求合并到 `o3de/development` 中，您就完成了！好！

### 对拉取请求的审查和反馈

SIG 维护者/审阅者可以通过提供反馈来请求更改。您应该与评论互动，并在同一分支上对您的代码贡献进行任何有效的更正或更新。如果自动审查 （AR） 失败，请查看错误，进行任何必要的修复，并在同一分支上更新拉取请求。

![An example of a failed AR check in a pull request.](/images/contributing/to-code/code-ar-failed.png)

如果您不进行更改以通过 AR，或忽略代码审查反馈，SIG 维护者可以通过将其标记为关闭来拒绝拉取请求中的更改。
