---
linkTitle: 更新 O3DE 远程存储库
title: 更新 O3DE 远程存储库
description: 了解如何更新 O3DE 远程存储库的内容或元数据。
weight: 400
toc: true
---

您可以使用 O3DE CLI 更新 O3DE 远程存储库的属性，添加和管理 Gem、Projects 和模板。


## 使用`o3de`命令行工具更新 O3DE 远程存储库

`o3de edit-repo-properties` 命令允许您更新远程存储库属性、添加新内容和管理现有内容。

查看 [`o3de edit-repo-properties` CLI 参考](/docs/user-guide/project-config/cli-reference/#edit-repo-properties)以获取完整的选项列表。

### 更新远程仓库属性

您可以使用`o3de` CLI 工具通过以下命令修改存储在 O3DE 远程存储库的`repo.json`文件中的属性：

{{< tabs name="Modify O3DE remote repository properties" >}}
{{% tab name="Windows" %}}

```cmd
<engine>\scripts\o3de.bat edit-repo-properties --repo-path <local path to repo.json> --repo-name <new repo name>
```

{{% /tab %}}
{{% tab name="Linux" %}}

```cmd
<engine>/scripts/o3de.sh edit-repo-properties \
    --repo-path <local path to repo.json> \
    --repo-name <new repo name>
```

{{% /tab %}}
{{< /tabs >}}


### 将新 Gem 或新 Gem 版本添加到远程存储库，或更新现有 Gem

您可以使用`o3de` CLI 工具将 `gem.json` 文件中的信息添加到`repo.json`文件中，并创建可以使用以下命令上传的存档：

{{< tabs name="Add information about a gem to `repo.json`" >}}
{{% tab name="Windows" %}}

```cmd
<engine>\scripts\o3de.bat edit-repo-properties --repo-path <path to repo.json> --add-gems <local gem paths> --release-archive-path <path where the gem archive files will go> --download-prefix <URL prefix for where the gem archive will made available, e.g. https://github.com/o3de/o3de-extras/releases/2.0>
```

{{% /tab %}}
{{% tab name="Linux" %}}

```cmd
<engine>/scripts/o3de.sh edit-repo-properties \
    --repo-path <path to repo.json> \
    --add-gems <local gem paths> \
    --release-archive-path <path where the gem archive files will go> \
    --download-prefix <URL prefix for where the gem archive will made available, e.g. https://github.com/o3de/o3de-extras/releases/2.0>
```

{{% /tab %}}
{{< /tabs >}}

运行此命令后，将修改`repo.json`文件，以便包含每个 `gem.json`文件中的信息，并且 `sha256` 存档文件哈希值正确。

如果`repo.json`中已存在同名的 Gem，但版本不同，则新版本的`gem.json`数据将添加到[`versions_data`](/docs/user-guide/programming/gems/manifest/#gemjson-manifest-contents) 字段中。 如果已存在具有相同名称和版本的 Gem，则将更新该 Gem。

{{< tip >}}
修改远程存储库内容后，我们建议您 [在本地测试更改](#testing-o3de-remote-repository-changes)然后再将其上传到公共 Web 服务器或 GitHub。
{{< /tip >}}


## 测试 O3DE 远程存储库更改

我们建议您在将更改上传到 GitHub、Web 服务器或其他源代码控制之前，使用 Project Manager 和 O3DE CLI 测试您的远程存储库。

### 使用 Project Manager 进行测试

您可以在 Project Manager 中添加本地远程存储库，并验证内容是否按预期显示以及下载是否成功。
1. 在 Project Manager 中，在 **Engine** 选项卡上的 **Remote Sources** 页面上，按 **Add Repository** 按钮，然后使用浏览按钮选择包含`repo.json` 文件的文件夹，然后按 **Add** 按钮。
1. 在列表中选择远程存储库，并验证右侧窗格中是否列出了预期内容。
1. 验证添加的任何 Gem 是否正确显示在 **Gem Catalog** 中。
1. 验证添加的所有项目是否正确显示在 **Projects** 页面上。
1. 验证添加的所有模板是否正确显示在 **Create a New Project** 页面上。


### 使用 O3DE CLI 进行测试

您可以使用 O3DE CLI 在硬盘驱动器上添加远程存储库并验证下载是否成功。

{{< tabs name="在硬盘驱动器上注册存储库并下载 Gem" >}}
{{% tab name="Windows" %}}

```cmd
<engine>\scripts\o3de.bat register -ru <path on disk to repo.json>
<engine>\scripts\o3de.bat download --gem-name <name of gem> --dest-path <download path>
<engine>\scripts\o3de.bat download --project-name <name of project> --dest-path <download path>
```

{{% /tab %}}
{{% tab name="Linux" %}}

```cmd
<engine>/scripts/o3de.sh register -ru <path on disk to repo.json>
<engine>/scripts/o3de.sh download --gem-name <name of gem> --dest-path <download path>
<engine>/scripts/o3de.sh download --project-name <name of project> --dest-path <download path>
```

{{% /tab %}}
{{< /tabs >}}

下载 Gem、Project 或 Template 后，您应该能够检查下载路径中的内容是否正确，并且可以按预期使用内容。

## 上传到 GitHub

我们建议您在将更改上传到 GitHub、Web 服务器或其他源代码控制提供商之前，使用 Project Manager 和 O3DE CLI 测试更新的远程存储库。

有关如何创建远程存储库、添加内容以及将版本上传到 GitHub 的示例，请参阅 [远程存储库教程](/docs/learning-guide/tutorials/remote-repositories/create-remote-repository/) 。
