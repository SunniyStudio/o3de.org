---
linktitle: 添加和删除 Gem
title: 在项目中添加和删除 Gem
description: 了解如何在 Open 3D Engine 中添加和删除项目中的 Gem。
toc: true
weight: 120
---

本教程介绍如何使用 **Project Manager** 或 **Open 3D Engine （O3DE）** 中的命令行界面 （CLI） 在项目中添加和删除 Gem。Gem 为您的项目提供功能和资源。有关 Gem 以及 O3DE 可用的 Gem 的更多详细信息，请参阅  [Gems](/docs/user-guide/gems)部分。

O3DE 包含许多可添加到项目中的 Gem。您还可以使用来自 *外部* 源的 Gem。必须先将外部 Gem 注册到您的项目中，然后才能激活它们。有关如何注册其他 Gem 的说明，请参阅 [注册 Gem](/docs/user-guide/project-config/register-gems/)。

## 概述

为项目配置 Gem 的基本步骤如下：

1. （可选）如果 Gem 来自外部源，请注册 Gem。

1. 找到您要激活或停用的 Gem。

1. 激活或停用 Gem。

1. （可选）选择 Gem 版本。

1. 保存更改。

1. 重新构建您的项目（如有必要）。
## 使用 Project Manager

### 打开 Configure Gems 屏幕

1. 启动 **Project Manager**，它可以在你的桌面上找到，或者如果你安装了 O3DE，可以在`bin/Windows/profile/Default/o3de.exe`中找到，如果你从源代码构建了 O3DE，则可以在你的引擎构建目录中找到它。

1. 打开项目的菜单，然后选择 **Configure Gems...** 按钮。

    ![For a project, go to the menu and select 'Edit Project Settings'. ](/images/user-guide/project-config/add-remove-gems/quick-start-1.png)

1. 在 **Configure Gem** 屏幕中，您可以筛选或搜索可用的 Gem。滚动结果列表以查找要为项目激活或停用的 Gem。

    ![Configure Gems screen.](/images/user-guide/project-config/add-remove-gems/configure-gems-screen.png)

### 定位 Gem

**筛选目录**

筛选 Gem 目录以帮助您查找一组特定的 Gem。

![Filter Gems](/images/user-guide/project-config/add-remove-gems/ui-filter-by.png)

您可以按以下组进行筛选：

|标准 |描述 |
| - | - |
| Status | 显示已处于活动状态或非活动状态的 Gem，当前为项目选择或未选择的 Gem 以及缺少的 Gem。  |
| Versions | 显示与您的项目兼容或具有版本更新的 Gem。 |
| Provider | 根据 Gem 的提供程序显示 Gem。O3DE 显示使用 O3DE 源构建的 Gem。**Local** 显示由其他来源提供的本地 Gem。**Remote** 显示由远程源提供的 Gem。|
| Type | 根据 Gem 的类型显示 Gem。**Asset** 显示用于向项目添加资源的 Gem。**Code** 显示扩展项目中功能的 Gem。**Tool** 显示提供可在 O3DE 中使用的工具的 Gem。 |
| Supported Platforms | 根据支持 Gem 的平台显示 Gem。 |
| Features | 根据 Gem 提供的功能显示 Gem。 |

**搜索目录**

通过在搜索字段中输入文本来搜索 Gem 目录。您可以使用以下任一搜索词搜索 Gem：

* Gem 名称
* Gem 显示名称
* 起源
* 总结
* 特征

![Search field](/images/user-guide/project-config/add-remove-gems/ui-search.png)

**查看 Gem 详细信息**

选择一个 Gem 以查看其描述、它所依赖或与之冲突的其他 Gem 以及其他信息。

![View Gem details](/images/user-guide/project-config/add-remove-gems/ui-gem-details.png)

当活动 Gem 有可用更新时，Gem 版本旁边的 Gem 行中将出现一个黄色通知图标。

![Gem update available](/images/user-guide/project-config/add-remove-gems/ui-gem-update-available.png)

### 激活或停用 Gem

通过切换给定 Gem 的 **Selected** 列下的开关，激活或停用项目中的 Gem。

![Gems to activate or deactivate during configuration.](/images/user-guide/project-config/add-remove-gems/ui-enable-disable-gem.png)

您可以一次激活或停用多个 Gem。将对项目进行的更改汇总在窗口的右上角。选择 Gem bag 图标以查看要激活或停用的 Gem。这些更改将在您保存并 （取决于 Gem 要求） 重新构建项目后生效。

![Gems to activate or deactivate during Gem configuration.](/images/user-guide/project-config/add-remove-gems/ui-gem-changes.png)

### 选择 Gem 版本

如果 Gem 有多个版本可用，则 Gem 详细信息面板中将显示一个下拉框，其中显示可用版本。 选择一个版本以查看有关它的详细信息。

![Gem version details.](/images/user-guide/project-config/add-remove-gems/ui-gem-version-details.png)

激活 Gem 时，将使用所选版本。 如果 Gem 已处于活动状态，则当 Gem 版本发生更改时，Gem 详细信息面板中将出现 **Use Version** 按钮。

![Use Gem version button.](/images/user-guide/project-config/add-remove-gems/ui-use-version-button.png)

### 保存和重建

1. 在项目中完成 Gem 的添加和删除后，选择 **Save**（保存）** 按钮。 如果发现添加的 Gem 存在任何兼容性问题，则会显示一个包含信息的弹出窗口，并询问您是要继续添加这些 Gem 还是取消。

1. 仅资产 Gem 通常不需要重新构建项目，但包含代码的 Gem 需要。保存对项目配置的更改时，如果需要重新生成，将显示一条警告消息。

    ![Project rebuild warning](/images/user-guide/project-config/add-remove-gems/project-rebuild-warning.png)

1. 在 Project Manager 主屏幕中，选择项目上的 **Build Project** 按钮以重新构建它（如有必要）。

    ![Build project button](/images/user-guide/project-config/add-remove-gems/project-build-button.png)

## 使用命令行界面 (CLI)

您还可以使用 CLI 配置 Gem。有关其他支持的 CLI 命令，请参阅 [CLI 参考](/docs/user-guide/project-config/cli-reference/)。

1. 打开命令提示符，找到引擎所在的文件夹。

1. 使用以下命令为项目激活/启用或停用/禁用 Gem。

    **Activate/Enable**

    ```cmd
    scripts/o3de enable-gem -gp <gem-path> -pp <project-path>
    ```

    **Deactivate/Disable**

    ```cmd
    scripts/o3de disable-gem -gp <gem-path> -pp <project-path>
    ```

    这些命令有多种变体，可用于指定带有可选版本说明符的名称或 Gem 和项目的路径。
    - `-gp`, `--gem-path`: Gem 文件夹的路径（可以是绝对路径或相对路径）。
    - `-gn`, `--gem-name`: 带有可选版本说明符的 Gem 名称，可以在 Gem 的`gem.json`配置文件中找到：即`--gem-name Atom>=1.0.0`。有关 Gem 版本说明符的更多信息，请参阅 [Gem 版本控制](/docs/user-guide/gems/gem-versioning)部分。
    - `-pp`, `--project-path`: 项目文件夹的路径（可以是绝对路径或相对路径）。
    - `-pn`, `--project-name`: 带有可选版本说明符的项目名称，可以在项目的 `project.json`  配置文件中找到。

1. 重新生成项目。有关从命令行构建项目的更多信息，请参阅 [构建](/docs/user-guide/build) 部分。
