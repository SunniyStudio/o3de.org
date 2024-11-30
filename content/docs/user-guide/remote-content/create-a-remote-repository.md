---
linkTitle: 创建 O3DE 远程存储库
title: 创建 O3DE 远程存储库
description: 了解如何创建 O3DE 远程存储库。
weight: 300
toc: true
---

您可以使用 O3DE CLI 创建 O3DE 远程存储库。

## 使用`o3de`命令行工具创建 O3DE 远程仓库

您可以使用`o3de` CLI 工具通过以下命令创建远程存储库：

{{< tabs name="Create an O3DE remote repository" >}}
{{% tab name="Windows" %}}

```cmd
<engine>\scripts\o3de.bat create-repo --repo-name <repo name> --repo-path <local path>
```

{{% /tab %}}
{{% tab name="Linux" %}}

```cmd
<engine>/scripts/o3de.sh create-repo --repo-name <repo name> --repo-path <local path>
```

{{% /tab %}}
{{< /tabs >}}

上述命令将在指定的本地路径创建一个 `repo.json` 文件。

查看 [`o3de create-repo` CLI 参考](/docs/user-guide/project-config/cli-reference/#create-repo)以获取完整的选项列表。

创建存储库后，在将其上传到公共 Web 服务器或 GitHub 之前，[将内容添加到远程存储库](/docs/user-guide/remote-content/update-a-remote-repository#add-a-new-gem-or-new-gem-version-to-a-remote-repository) 和 [在本地测试](/docs/user-guide/remote-content/update-a-remote-repository#testing-o3de-remote-repository-changes) 。
