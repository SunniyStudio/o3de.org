---
linktitle: 注册 Gem
title: 将 Gem 注册到项目
description: 了解如何在 Open 3D Engine （O3DE） 中将外部 Gem 注册到项目。
toc: true
weight: 140
---

Open 3D Engine （O3DE） 包含许多 Gem，用于扩展功能并向项目添加资产。有关完整列表，请参阅 [Gem 参考](/docs/user-guide/gems/)。这些 Gem 已注册到引擎中，并可以添加到项目中。

您还可以从 O3DE 以外的源注册 * 外部* Gem，以便它们也可以在您的项目中使用。注册 Gem 使您的项目能够找到该 Gem。Gem 文件夹可以位于计算机上的任何位置。

注册 Gem 后，您可以启用它以在您的项目中使用。有关启用或禁用 Gem 的说明，请参阅 [在项目中添加和删除 Gem](/docs/user-guide/project-config/add-remove-gems/)。

## 注册

要将 Gem 注册到您的项目：

1. 打开引擎所在文件夹的命令提示符。

2. 使用以下命令将 Gem 注册到您的项目中。此命令在将 Gem 注册到项目之前验证指定的 Gem 路径是否包含有效的`gem.json` 配置文件。将 Gem 注册到项目后，Gem 路径将添加到项目的`project.json` 配置文件的`external_subdirectories`列表中。
   
    ```cmd
    scripts/o3de register -gp <gem-path> -espp <project-path>
    ```

    选项有短记法和长记法，可用于指定 Gem 和项目的路径。
    - `-gp`, `--gem-path`: Gem 文件夹的路径（可以是绝对路径或相对路径）。
    - `-espp`, `--external-subdirectory-project-path`: 项目文件夹的路径（可以是绝对路径或相对路径）。

## 注销
要从项目中取消注册 Gem，请在用于注册 Gem 的命令中添加`--remove`选项。这会从项目的 `project.json` 列表中的`external_subdirectories`配置文件中删除 Gem 路径。

```cmd
scripts/o3de register -gp <gem-path> -espp <project-path> --remove
```
