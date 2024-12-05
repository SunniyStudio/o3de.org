---
linktitle: 远程存储库
title: 创建远程仓库
description: 了解如何使用 Open 3D Engine （O3DE） 的远程存储库工具打包项目并与 GitHub 或其他版本控制平台远程共享。
---
在本教程中，您将以 Open 3D Engine （O3DE） 为基础，并使用远程存储库模板轻松管理和分发项目。您将学习如何使用 GitHub 或其他版本控制平台远程打包和共享您的项目，从而促进协作并加速项目开发。

在本教程中，您将从命令提示符使用 O3DE 命令行界面 （CLI） 工具。你可以列出所有 O3DE 命令，并使用 `--help` 选项获得特定命令的帮助：
```
cd <o3de-engine-directory>/scripts
o3de.bat --help
o3de.bat create-gem --help
```
{{< note >}}
为方便起见，所有 `o3de` 命令选项都有缩写。使用命令的 `--help` 选项来了解详细信息。
{{< /note >}}

## 创建仓库
为您的远程项目创建一个新存储库，并将此存储库克隆到您的本地计算机。
此版本控制存储库链接是您上传项目 `repo-uri` 的位置。

下面是 GitHub 存储库 URI 的示例：

```
"https://github.com/<YourGitAccount>/<RemoteProjectName>.git"
```

## 创建远程仓库 `repo.json` 配置文件
从 Open 3D Engine （O3DE） 的根目录导航到 scripts 文件夹。
```
cd <o3de-engine-directory>/scripts
```
要创建远程存储库配置，您需要提供以下信息：
- `repo-path` - 远程仓库的本地路径，这是您克隆仓库的位置。
- `repo-uri` - 存储库的 URI。

{{< tabs name="O3DE CLI command" >}}
{{% tab name="Windows" %}}
```cmd
o3de.bat create-repo --repo-path "C:\remote-repo" --repo-uri "https://github.com/<YourGitAccount>/<RemoteRepoName>.git"
```
{{< note >}} 如果您使用的是 PowerShell，请使用 ./o3de.bat。{{< /note >}}

{{% /tab %}}
{{% tab name="Linux" %}}
```
./o3de.sh create-repo --repo-path "~/remote-repo" --repo-uri "https://github.com/<YourGitAccount>/<RemoteRepoName>.git"
```
{{% /tab %}}
{{< /tabs >}}

`create-repo` 命令执行以下操作：
1. 使用`repo-path`中指定的远程存储库名称创建一个目录，并在该目录的根目录中创建一个`repo.json`文件。
2. 在远程存储库文件夹中创建三个文件夹：Gems、Projects 和 Templates。确保将所有关联的对象放在正确的文件夹类型中，以避免以后混淆。
这是您的远程仓库文件夹在此步骤后的样子：

    {{<image-width src="/images/learning-guide/tutorials/remote-repositories/init.png" width="300">}}

- 例如：Gem 应放在 Gems 文件夹中，项目应放在 Projects 文件夹中，模板应放在 Templates 文件夹中。

{{< note >}}
请记住将本地更改推送到远程版本控制存储库。
{{< /note >}}

##（可选）创建 Gem 发布存档以添加到您的远程存储库
要测试远程存储库的创建，您可以创建自己的 Gem 并将其设为公开，以允许其他用户下载。如果您的远程存储库中已有 Gem，请跳过此步骤。

要创建 Gem，您需要提供以下信息：

- `gem-path`: 您希望存储 Gem 的目录的路径（我们建议您在远程存储库的 Gems 文件夹中选择一个路径）
- `gem-name`: 指定 Gem 名称的可选参数
   
```
o3de.bat create-gem --gem-path "C:\remote-repo\Gems\MyGem" --gem-name "MyGem"
```

`create-gem`命令执行以下操作：
1. 在指定路径中创建具有提供名称的 Gem。如果未指定名称，O3DE 将使用`gem-path`中的尾随目录名称。
3. 将您的 Gem 注册到 O3DE 引擎。现在，您可以在 Project Manager 的 Gems 选项卡下找到具有 Gem 名称的新 Gem。

接下来，使用`edit-repo-properties`命令将 Gem 添加到远程存储库。

**将 Gem 添加到远程存储库**

要将 Gem 添加到您的项目中，您需要提供以下信息：
- `--repo-path`: repo.json 文件的路径
- `--add-gem`: Gem 目录的路径

    ```
    o3de.bat edit-repo-properties --repo-path "C:\remote-repo\repo.json" --add-gem "C:\remote-repo\Gems\MyGem"
    ```
    `edit-repo-properties` 命令会将你的 gem 注册到远程仓库的 `repo.json` 文件中的 `gems_data` 字段下。

在下一步中，您将为您的 Gem 创建一个可下载的发布存档，以便与全世界共享。确保在进入下一步之前推送所有更改。

### 为您的 Gem 创建发布存档
_release archive_ 的主要目的是提供定义明确且稳定的软件版本，该版本可以轻松共享、安装和分发。

在 O3DE 中，我们为用户提供了创建.zip格式的发布存档的选项。

为了安全起见，O3DE 在创建发布存档之前使用“`sha-256`”对文件进行哈希处理。每次您对 Gem（包括“`repo.json`”文件）进行更改时，“`sha-256`”哈希值都会更改。

**自动将您的发布存档上传到 Github**

通过提供发布 '`tag_name`' 和 GitHub 令牌，将发布 zip 上传到 GitHub。

要允许自动更新到 GitHub，您需要提供以下信息：
- `--repo-path`: 远程仓库的 `repo.json` 文件的绝对路径
- `--add-gem`: 要添加的实际 Gem 所在的目录的路径
- `--release-archive-path`: 生成的 ZIP 的保存位置。我们建议将其放在存储库之外，这样就不会意外签入
- `--upload-git-release-tag`: GitHub 在创建发布时使用的tag_name。建议使用版本号;例如，“v1.0.0”

{{< note >}}
1. `--release-archive-path` 命令只能在您调用 `--add-gem` 命令后使用。请参阅以下示例：<br>
2. 您的 GitHub 令牌必须允许存储库内容读取和写入访问权限。在鼠标上按右键粘贴您的 GitHub 令牌。
{{< /note >}}

```
o3de.bat edit-repo-properties --repo-path "C:\remote-repo\repo.json" --add-gem "C:\remote-repo\Gems\MyGem" --release-archive-path "C:\remote-repo\Gems" --upload-git-release-tag v1.0.0
```

O3DE Engine 将提示您输入 GitHub 令牌。GitHub 版本需要标签。如果 GitHub 上已存在 `tag_name` ，该命令会将你的 zip 添加到现有版本中。否则，此命令会将 zip 发布到新版本。

将 Gem 上传到 GitHub 后，其他用户应该能够下载此 Gem。在 Project Manager 远程源中，添加远程存储库 URI 以下载关联的 Gem。否则，请使用 O3DE CLI 下载关联的 Gem。

**使用 download 前缀创建发布存档。**

要将发行版上传到 GitHub 以外的其他版本控制平台，您可以使用 `--download-prefix` 命令。

- “`o3de.bat`”工具需要一个“下载前缀”，这是您的档案所在的文件夹的 URI。 对于 GitHub 存储库中的版本，“下载前缀”如下所示：

    `https://github.com/<YourGitAccount>/<RemoteRepoName>/releases/download/<ReleaseTag>`

要创建发布存档，您需要提供以下信息：
- `--repo-path`: 远程仓库的 `repo.json` 文件的绝对路径
- `--add-gem`: 要添加的实际 Gem 所在的目录的路径
- `--release-archive-path`: 生成的 ZIP 的保存位置。我们建议将其放在存储库之外，这样就不会意外签入
- `--download-prefix`: 可从中下载 Gem 存档的 URI。在实际创建存档 zip 之前，您必须知道 zip 可以在哪里下载。

{{< note >}}
`--release-archive-path` 命令只能在您调用 `--add-gem` 命令后使用。请参阅以下示例：
{{< /note >}}


```
o3de.bat edit-repo-properties --repo-path "C:\remote-repo\repo.json" --add-gem "C:\remote-repo\Gems\MyGem" --release-archive-path "C:\remote-repo\Gems" --download-prefix "https://github.com/<YourGitAccount>/<RemoteRepoName>/releases/download/<ReleaseTag>"
```

1. 以下是您应该将发布上传到 GitHub 的位置示例（您可以在版本控制 URI 上找到）：

    {{<image-width src="/images/learning-guide/tutorials/remote-repositories/add_release.png" width="500">}}

2. 这是您将设置发布标签的地方（确保此标签与您为 `--download-prefix` 参数传递的最后一个参数相同）：

    {{<image-width src="/images/learning-guide/tutorials/remote-repositories/release_tag.png" width="500">}}

{{< important >}}
确保您知道在将版本上传到版本控制平台后，您的版本将在哪里可用。上传后，您将无法更改“`repo.json`”文件中的“`download_source_uri`”。如果输入的下载路径错误，则需要重复此步骤并重新压缩文件，然后再次将版本上传到版本控制平台。
{{< /important >}}

将 Gem 上传到版本控制平台后，已添加远程存储库的其他用户将能够使用 Project Manager 和“`o3de`”CLI 下载它。用户可以导航到 Remote Sources（远程源），并使用您的远程项目 URL 根据您在此步骤中提供的下载路径下载关联的 Gem。


## 在本地测试你的远程仓库
在上传到远程存储库之前，您可以在本地测试远程存储库，以确保使用本地文件路径远程存储库按预期工作。

您可以通过使用上述步骤创建另一个远程存储库和 Gem 来本地测试，并将 Gem 添加到新的远程存储库中。接下来，使用“下载前缀”的本地路径为新 Gem 创建发布存档：
```cmd
o3de.bat edit-repo-properties --repo-path "C:\local-repo\repo.json" --add-gem "C:\local-repo\Gems\MyLocalGem" --release-archive-path "C:\local-repo\Gems" --download-prefix "C:\local-repo\Gems"
```

执行上述步骤后，您应该能够使用本地路径 `file://c/local-repo` 添加新的远程存储库，然后能够从项目管理器中的“`Gems`”选项卡下载“`MyLocalGem`”。


#### 使用 Python 托管“本地服务器”进行测试

1. 打开命令窗口并切换到包含“`repo.json`”文件的本地 repo 文件夹。
    ```
    cd c:\remote-repo
    ``` 

2. 在端口 8080 上启动本地 Python 服务器。
    ```
    python -m http.server 8080
    ```

3. 对 `--download-prefix` 使用以下格式。
    ```
    http://localhost:8080/Gems
    ```

    以下是下载 URI 所在的位置示例：
    ```
    http://localhost:8080/Gems/camera-0.1.0-gem.zip
    ```


4. 打开 Project Manager，然后从 Engine > Remote Sources 页面添加远程存储库。出现提示时，请使用本地服务器的 URL，该 URL 应为“http://localhost:8080/”

{{< note >}}
如果没有尾部斜杠，则添加存储库可能会失败，并且名称应与repo.json中的“`repo-uri`”匹配
{{< /note >}}
