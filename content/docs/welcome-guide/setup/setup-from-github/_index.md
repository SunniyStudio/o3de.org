---
title: 设置 O3DE from GitHub
description: 了解如何从 Open 3D Engine （O3DE） 的 GitHub 源中设置 Open 3D Engine。
weight: 300
toc: true
---

从 GitHub 获取 **Open 3D Engine （O3DE）** 的源代码是设置开发环境的好方法。此方法可让您轻松同步未来的引擎更新，并为开源项目库做出贡献。有关从 GitHub 设置 O3DE 的说明，请观看以下视频或本页面上的教程。

本节中的视频和书面说明将指导您完成以下步骤：

* 为 Git LFS 配置凭证。
* 分叉并克隆 O3DE GitHub 存储库。
* 构建 O3DE 引擎。
* 注册引擎。

{{< youtube-width id="GZF_6SkAXPw" title="Setting up O3DE from GitHub" >}}

## 先决条件

GitHub 设置说明假定您拥有：

* [Git client](https://git-scm.com/downloads) 已安装 （需要 1.8.2 或更高版本，建议使用 2.23.4 或更高版本）。

## 为 Git LFS 配置凭证

O3DE GitHub 存储库使用 Git 大型文件存储 （LFS） 系统来存储大型二进制文件。以下说明将准备您的 PC 在从存储库克隆、提取或拉取时自动验证和下载这些文件。

**要配置 Git LFS**

1. 验证是否已安装 **Git LFS**。

    {{< tabs name="Git LFS install" >}}
    {{% tab name="Windows" %}}

```cmd
git lfs install
```

如果此命令的输出为“Git LFS initialized”，则您已经安装了 Git LFS。如果您尚未安装 Git LFS，此命令将为您安装它。

    {{% /tab %}}
    {{% tab name="Linux" %}}

```bash
sudo apt install git-lfs
```

    {{% /tab %}}
    {{< /tabs >}}

1. 验证您是否为 Git 设置了 **凭证管理器**。最新版本的 Git 会安装凭证管理器来存储您的凭证，这样您就不必为每个请求输入凭证。

    ```cmd
    git config credential.helper
    ```

    此 `git config` 命令的常见结果示例包括`manager-core`, `wincred`, `osxkeychain`, 和 `cache`。但是，如果您在响应此命令时没有看到 _anything_，则可以安装  [Git Credential Manager Core](https://github.com/microsoft/Git-Credential-Manager-Core#readme)作为您的凭证管理器。

1. 创建范围为`repo` and `read:org`的 GitHub **个人访问令牌**。

    1. 访问[https://github.com/settings/tokens](https://github.com/settings/tokens)。

        (GitHub 菜单等效项： Settings > Developer Settings > Personal Access Tokens)
    1. 选择 **Generate new token**。
    1. （可选）在 **Note** 下添加一个条目。这仅供您参考。您可以将其用作令牌用途的个人提醒。
    1. 在 **Select scopes** 下，选择以下项：
        * `repo` (all)
        * `read:org` (under admin:org)
    1. 选择 **Generate token**（生成令牌）。将令牌放在手边（但要保密）以备后续步骤使用。

    {{< important >}}
此令牌可以代替您的 GitHub 密码，因此请像保护 GitHub 密码一样保护它！
    {{< /important >}}

## 分叉和克隆

在提交拉取请求之前，对 O3DE 存储库的所有贡献都应在 fork 中暂存。有关贡献工作流程的详细信息，请参阅《O3DE 贡献者指南》中有关 [为 O3DE 代码源做贡献](/docs/contributing/to-code)  的文档。

**从 GitHub 在您的 PC 上分叉和克隆 O3DE**

1. 从 [https://{{< links/o3de-source >}}](https://{{< links/o3de-source >}}) 创建 O3DE GitHub 仓库的分支。或者，如果您作为已创建复刻的团队的成员工作，则可以跳过此步骤并使用团队的现有复刻。

    ![Create a fork using the Fork button on the O3DE GitHub repo](/images/welcome-guide/setup-create-fork.png)

    有关在 GitHub 中创建和使用复刻的一般说明和帮助，请参阅[克隆项目](https://guides.github.com/activities/forking/)。

1. 克隆 fork 的存储库。使用您首选的 Git UI 或命令行在您选择的目录中克隆存储库。您将需要 GitHub 用户名和之前创建的个人访问令牌。

    1. 从复刻克隆存储库。

        ```cmd
        # Cloning from your personal fork.
        git clone https://github.com/YOUR-USERNAME/o3de.git

        # Cloning from your team's fork.
        git clone https://github.com/TEAM-FORK/o3de.git
        ```

    1. 如果显示 **Connect to GitHub** 对话框，请按照说明使用浏览器或 personal access token 登录到 GitHub。

        ![GitHub sign in dialog box](/images/welcome-guide/setup-github-signin.png)

    1. 在下一个 _sign in_ 对话框中输入 LFS 终端节点的凭证。使用您的 **GitHub 用户名** 和 **个人访问令牌** 作为密码。

        ![Credential manager asking for LFS credentials](/images/welcome-guide/setup-credential-manager-lfs.png)

   1. 验证您有 LFS 文件。

        克隆操作完成后，验证您是否拥有来自 LFS 终端节点的所有文件。您应该不会再收到凭证提示。

        ```cmd
        # Change to the directory name that was created when you cloned the engine repo.
        # The default is o3de.
        cd o3de
        
        git lfs pull
        ```

1. 添加远程以跟踪上游存储库。这将使您能够将更新从 O3DE 存储库直接拉取到本地克隆中。

    ```cmd
    git remote add upstream https://{{< links/o3de-source >}}.git
    ```

    验证上游仓库。您应该看到复刻的 URL 为`origin`，原始仓库的 URL 为`upstream`。

    ```cmd
    git remote -v
    ```

    Output:

    ```cmd
    origin  https://github.com/<FORK>/o3de.git (fetch)
    origin  https://github.com/<FORK>/o3de.git (push)
    upstream        https://{{< links/o3de-source >}}.git (fetch)
    upstream        https://{{< links/o3de-source >}}.git (push)
    ```

1. （可选）如果您不希望对这些文件进行更改，请忽略此步骤。否则，请更新 LFS URL 以包含您的复刻。这允许您将更改推送到 LFS 中的文件。有关完整说明和要使用的 **DISTRIBUTION**，请打开存储库根目录下的`.lfsconfig`文件。

    ```cmd
    git config lfs.url https://<DISTRIBUTION>.cloudfront.net/api/v1/fork/<FORK> 
    ```

    下次拉取或推送时，系统可能会提示您重新进行身份验证。 请记住使用 GitHub 个人访问令牌，而不是 GitHub 密码。

    如果您希望还原此更改，可以运行以下命令：

    ```cmd
    git config --unset lfs.url 
    ```

1. 每当您想要同步存储库和 LFS 中的最新文件时，您都可以合并您正在使用的上游分支中的更改。默认分支为 **development**。

    ```cmd
    git fetch upstream
    git merge upstream/development
    ```

1. 在典型的 contributor 工作流程中，您将主要从 fork 的 development 分支的分支开始工作。你可以使用 git `switch`命令创建本地工作分支，并将其设置为跟踪上游开发分支。

    ```cmd
    git fetch upstream
    git switch -c <NEW_WORKING_BRANCH> upstream/development
    ```

    当设置为跟踪上游分支时，每当你想从上游同步最新更改时，都可以使用 git `pull` 命令。

    ```cmd
    git pull
    ```

有关常见贡献者工作流程的更多信息和示例，请参阅《贡献者指南》中的 [O3DE 代码贡献 GitHub 工作流](/docs/contributing/to-code/git-workflow)。

要完成设置，请继续执行与您的平台匹配的引擎构建和注册说明。

* [为 Windows 构建](building-windows)
* [为 Linux 构建](building-linux)
