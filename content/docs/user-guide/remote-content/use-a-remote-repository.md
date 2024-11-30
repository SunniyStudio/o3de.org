---
linkTitle: 使用 O3DE 远程存储库
title: 使用 O3DE 远程存储库U
description: O3DE 远程存储库提供了一种在 Open 3D Engine （O3DE） 中共享 Gem、项目和模板的方法。
weight: 100
toc: true
---

您可以使用 [Project Manager](#using-project-manager-to-access-o3de-remote-repositories#using-project-manager-to-access-o3de-remote-repositories)和 [O3DE CLI](#using-the-o3de-command-line-tool-to-access-o3de-remote-repositories) 来显示、下载和注册 **Remote Repositories**中的 **Gem**、**Projects** 和 **Templates**。

## 使用 Project Manager 访问 O3DE 远程存储库
### 添加远程仓库

您可以使用 Project Manager 中的 **Remote Sources** 页面添加现有的 O3DE 远程存储库，您可以从 **Engine** 选项卡或 **Gem Catalog** 右侧面板菜单访问该页面。
1. 按 **Add Repository** 按钮。在打开的对话框中，您可以提供 Gem 存储库的 URL 或本地路径。

    ![Add Remote Source Dialog](/images/user-guide/remote-content/ProjectManagerAddRemoteSource.JPG)

1. 如果您有在线 Gem 存储库的 URL，请将其复制并粘贴到`Repository Path`字段中，然后按 **Add** 按钮。
1. 或者，如果您想从本地硬盘驱动器添加 O3DE 远程存储库，请输入计算机上的本地路径，或按“存储库路径”字段右侧的文件夹按钮打开`Browse`对话框，选择具有 O3DE 远程存储库的文件夹，按 **Select Folder** 按钮，最后按 **Add** 按钮。


远程源列表中将添加一个新条目，并且 O3DE 远程存储库提供的 Gem、Projects 和 Templates 将可供下载和使用。

### 激活或停用远程仓库

您可以通过在 **Remote Sources** 页面上按下与 O3DE 远程存储库相同的行上的可见性按钮来切换 O3DE 远程存储库中远程内容的可见性。 这是在不删除远程存储库的情况下隐藏该存储库中内容的有用方法。

![Visibility buttons on the Remote Sources page](/images/user-guide/remote-content/ProjectManagerRemoteSources.JPG)

### 删除远程仓库

您可以从 Project Manager 的 **Remote Sources** 页面中删除 O3DE 远程存储库 URI，以便不再列出其中的远程内容，并且元数据不再更新。

1. 单击远程源列表中的条目，选择要删除的 O3DE 远程存储库。
2. 按下右侧面板中的 **Remove** 按钮。 系统将要求您确认是否要删除远程仓库。

### 从远程仓库下载 Gem

您可以从 Project Manager 的 **Gems** 选项卡下载 Gem，方法是单击要下载的 Gem 的行，然后按右侧窗格中的 **Download Gem** 按钮。

![Visibility buttons on the Remote Sources page](/images/user-guide/remote-content/ProjectManagerDownloadGem.JPG)

在项目中激活远程存储库中可用的 Gem 时，也会自动从 **Gem Catalog** 页面下载，该页面可通过按项目的 Edit Project Settings 页面上的 **Configure Gem**（配置 Gem）按钮或 **Projects**选项卡上的 **Configure Gems...** 项目菜单选项进行访问。

### 从远程存储库下载项目

您可以通过按要下载的项目上的 **Download** 按钮，从 Project Manager 的 **Projects** 选项卡下载项目。

### 从远程存储库下载模板

您可以从 Project Manager 中的 **Create a New Project** 页面下载模板，方法是在项目模板列表中选择项目模板，然后按右侧窗格中的 **Download Template** 按钮。

## 使用`o3de` 命令行工具访问 O3DE 远程存储库

您可以使用`o3de` 命令行工具注册、刷新、激活和停用远程存储库以及下载远程内容。
以下示例显示了用于远程存储库的一些常用命令。 请参阅 [`o3de repo` CLI 参考](/docs/user-guide/project-config/cli-reference/#repo) 以获取完整的选项列表。

### 使用`o3de`命令行工具添加远程存储库

您可以使用`o3de` CLI 工具通过以下命令添加 O3DE 远程存储库：

{{< tabs name="添加 O3DE 远程存储库" >}}
{{% tab name="Windows" %}}

```cmd
<engine>\scripts\o3de.bat register -ru <O3DE remote repository URI>
```

{{% /tab %}}
{{% tab name="Linux" %}}

```cmd
<engine>/scripts/o3de.sh register -ru <O3DE remote repository URI>
```

{{% /tab %}}
{{< /tabs >}}

查看 [`o3de register` CLI 参考](/docs/user-guide/project-config/cli-reference/#register)以获取完整的选项列表。

### 使用 `o3de`命令行工具激活或停用远程存储库

您可以使用 `o3de` CLI 工具通过以下命令激活或停用 O3DE 远程存储库：

{{< tabs name="Activate an O3DE remote repository" >}}
{{% tab name="Windows" %}}

```cmd
<engine>\scripts\o3de.bat repo --activate-repo <O3DE remote repository URI>
```

{{% /tab %}}
{{% tab name="Linux" %}}

```cmd
<engine>/scripts/o3de.sh repo --activate-repo <O3DE remote repository URI>
```

{{% /tab %}}
{{< /tabs >}}

停用远程存储库后，您将无法再看到该远程存储库的远程内容（该远程存储库尚未在 Project Manager 中下载），也无法从该远程存储库下载内容，直到它再次激活。

查看 [`o3de register` CLI 参考](/docs/user-guide/project-config/cli-reference/#register)以获取完整的选项列表。

### 使用 `o3de`命令行工具从远程存储库下载内容

您可以使用 `o3de` CLI 工具通过以下命令从 O3DE 远程存储库下载内容：

{{< tabs name="Download from an O3DE remote repository" >}}
{{% tab name="Windows" %}}

```cmd
<engine>\scripts\o3de.bat download --gem-name <Gem name with optional version specifier>
```

下载名为`RemoteGem` 版本 `1.0.0`的 Gem 的示例：
```cmd
<engine>\scripts\o3de.bat download --gem-name RemoteGem==1.0.0
```

如果未提供版本说明符，则下载可用的最高版本。

下载名为`RemoteProject`的项目的最新版本的示例：
```cmd
<engine>\scripts\o3de.bat download --project-name RemoteProject
```

将名为 `RemoteTemplate` 的模板的最新版本下载到`c:\o3de-templates\RemoteTemplate`的示例：
```cmd
<engine>/scripts/o3de.bat download --template-name RemoteTemplate  --dest-path c:/o3de-templates
```

{{% /tab %}}
{{% tab name="Linux" %}}

```cmd
<engine>/scripts/o3de.sh download --gem-name <Gem name with optional version specifier>
```

下载名为`RemoteGem` 版本 `1.0.0`的 Gem 的示例：
```cmd
<engine>/scripts/o3de.sh download --gem-name RemoteGem==1.0.0
```

如果未提供版本说明符，则下载可用的最高版本。

下载名为 `RemoteProject` 的项目的最新版本的示例：
```cmd
<engine>/scripts/o3de.sh download --project-name RemoteProject
```

将名为`RemoteTemplate`的模板的最新版本下载到 `~/o3de-templates/RemoteTemplate` 的示例：
```cmd
<engine>/scripts/o3de.sh download --template-name RemoteTemplate  --dest-path ~/o3de-templates
```

{{% /tab %}}
{{< /tabs >}}

查看 [`o3de register` CLI 参考](/docs/user-guide/project-config/cli-reference/#register)以获取完整的选项列表。

