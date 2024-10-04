---
linkTitle: Windows和Linux项目导出
title: Windows和Linux项目导出
description: 了解如何使用项目导出功能自动准备项目发布。
toc: true
weight: 400
---

## 先决条件
要充分利用 “项目管理器 ”中的 “导出 ”按钮或`export-project`CLI 命令，建议先准备一个至少有一个起始层的项目，并准备好所有必要的种子列表文件。可能需要调整 AssetProcessor 注册表文件设置。要了解如何设置，请参阅以下页面：[为Windows创建项目游戏发布布局：设置起始关卡](../windows-release-builds/#set-the-starting-level).

要了解更多有关 AssetBundler 和种子文件的信息，请访问 [AssetBundler 工具概述页面](https://docs.o3de.org/docs/user-guide/packaging/asset-bundler/overview/)。


{{< note >}}
如果您想了解如何为 Windows 手动准备项目，请参阅页面 [为 Windows 创建项目游戏发布布局](/docs/user-guide/packaging/windows-release-builds)。
{{< /note >}}
## 使用项目管理器

### 入门
在 “项目管理器”中使用导出功能与构建项目类似，只需一个操作即可启动导出过程。单击所需项目卡的下拉菜单，即可看到下面列出的导出选项。

{{< image-width "/images/user-guide/packaging/project-export-pc/export-button-dropdown.png" "400">}}

导出启动器 选项会显示一个子菜单，其中列出了运行导出过程的平台。最上面的选项总是与开发机器的操作系统相匹配。

{{< image-width "/images/user-guide/packaging/project-export-pc/export-button-platform-submenu.png" "400">}}

点击其中一个平台选项后，导出过程将开始。弹出的确认窗口会询问您是否要继续，并提醒您检查导出设置（如果尚未检查）。

{{< image-width "/images/user-guide/packaging/project-export-pc/export-button-confirmation.png" "400">}}

确认准备就绪后，项目管理器将开始导出项目。如果您选择的是 Windows/Linux 平台，它将根据导出设置创建一个基于 PC 的游戏启动器。最终输出将位于[导出设置面板](#export-settings-panel)中 “输出路径 ”设置所指定的目录中。

{{< image-width "/images/user-guide/packaging/project-export-pc/export-card-running.png" "200">}}

### 导出设置面板

要访问导出设置面板，只需单击上一节所示下拉菜单中的"Open Export Settings..."按钮。点击后，您将看到以下表格。

{{< image-width "/images/user-guide/packaging/project-export-pc/export-settings-pc.png" "400">}}

面板分为两个主要部分。面板的上半部分包含 O3DE 所有支持平台的通用设置。下半部分包含由选项卡分隔的特定平台设置。在本指南中，我们将重点介绍 “Windows”选项卡（注：如果您使用 Linux 发行版进行开发，则该选项卡的名称应为 “Linux”）。

要了解每个设置的更多信息，可以将鼠标悬停在每个设置的文本标签上，以获取工具提示说明。或者，您也可以查阅[CLI参数表](#usage) 查看导出设置所依据的参数。

## 使用 CLI
`export-project` CLI 命令用于自动打包发布的游戏代码和资产。

### CLI 和脚本入门
要正确使用 `export-project` 命令，首先要了解它的工作原理。

该命令分两个阶段运行：
1. 脚本加载
2. 运行脚本

#### 脚本加载
在导出过程开始之前，export-project命令会执行以下预处理步骤：
* 解析和验证初始参数集，并筛选参数供所需的导出脚本处理
* 将导出脚本加载为 python 模块
* 验证预期项目路径
* 使用 O3DE CLI Python API 修改系统路径，以便用户脚本可以轻松导入 API 代码和模块
* 准备 O3DE 脚本上下文，其中包含项目路径、引擎路径、项目名称、CMake 参数等有用值。
* 注入准备好的上下文运行导出脚本

#### 运行脚本
`export-project` 命令可以运行用户指定为导出脚本的任何 python 脚本。因此，具体行为完全取决于所提供的脚本。O3DE 包含一个标准脚本，[下文](#standard-export-script)将进一步介绍。 

通过该命令运行的任何脚本都可以访问出口程序的运行上下文以及常用 API。

### 导出 CLI 命令
`export-project` 命令可通过引擎随附的 O3DE CLI 访问。这可以在`<engine>/scripts/o3de.bat`中找到，或在Unix系统`<engine>/scripts/o3de.sh`中。此命令具有以下参数：

| 参数名               | 说明                                                                                                          | 是否必须? |
|-------------------|-------------------------------------------------------------------------------------------------------------|-------|
| `--export-script` | 选择要运行的外部 python 脚本。路径可以是相对路径或绝对路径。                                                                          | 是     |
| `--project-path`  | 要导出的项目。路径可以是相对路径，也可以是绝对路径。如果未提供，将由导出脚本位置推断。                                                                 | 否     |
| `--log-level`     | 设置导出日志的冗长程度。INFO 表示输出所有信息。ERROR 表示除非发生故障，否则将保持沉默。选项有`[DEBUG, INFO, WARNING, ERROR, CRITICAL]`，而`ERROR`是默认值。 | 否     |

您可以查看解析参数的代码 [此处](https://github.com/o3de/o3de/blob/753480ec930e55f3431f92ed7b974ba7f9e73a13/scripts/o3de/o3de/export_project.py#L354)。

调用示例如下
```cmd
<engine-folder>\scripts\o3de.bat export-project --export-script C:\..\path\to\export-script --project-path C:\..\path\to\project-folder --log-level INFO
```

或浓缩形式：
```cmd
<engine-folder>\scripts\o3de.bat export-project -es C:\..\path\to\export-script -pp C:\..\path\to\project-folder -ll INFO
```

#### 项目模板

项目导出功能也可按项目使用。这是因为项目模板中包含了辅助导出脚本，如[`package.bat`](https://github.com/o3de/o3de/blob/f25503785829c3eb75d3f00420d2072985d5ed05/Templates/DefaultProject/Template/package.bat)和[`package.sh` 用于 Unix](https://github.com/o3de/o3de/blob/f25503785829c3eb75d3f00420d2072985d5ed05/Templates/DefaultProject/Template/package.sh)。这些脚本包含在使用 `DefaultProject` 模板创建的每个新项目的根文件夹中。

软件包脚本会为给定项目预先填充相关参数，然后代表用户调用 `export-project`。

要使用它，只需运行以下命令即可（假设项目已创建并注册）：

```
# On Windows
\path\to\project\package.bat

#On Unix
\path\to\project\package.sh
```

### 标准导出脚本
O3DE 现在随附一个[标准项目导出脚本](https://github.com/o3de/o3de/blob/753480ec930e55f3431f92ed7b974ba7f9e73a13/scripts/o3de/ExportScripts/export_source_built_project.py)，能够自动处理大多数典型的项目用例。该脚本提供了代码和应用程序接口，因此有特殊需求的用户可以根据需要研究、扩展或修改代码。目前，该脚本仅支持 Windows 和 Linux 平台，不处理部署到外部服务的问题。

导出脚本有两个主要部分：函数 [`export_standalone_project`](https://github.com/o3de/o3de/blob/753480ec930e55f3431f92ed7b974ba7f9e73a13/scripts/o3de/ExportScripts/export_source_built_project.py#L19) 和 [仅在 CLI 调用脚本时运行的启动代码](https://github.com/o3de/o3de/blob/753480ec930e55f3431f92ed7b974ba7f9e73a13/scripts/o3de/ExportScripts/export_source_built_project.py#L187)。 有关这两个部分的深入讨论，请参阅 [开发人员指南](/docs/engine-dev/tools/o3de-cli/project-export)。


#### 用法
要使用导出脚本，首先要确保您拥有项目所需的种子列表文件（如前提条件中所述）。

只要将该脚本作为指定的导出脚本，就可以在运行 `export-project` 命令的同时发出该脚本的参数。脚本的特定参数将推迟到脚本开始运行时才发布。

参数如下：

| 参数名                                                                                                                                                                                  | 说明                                                                                                                                                                                                                               | 是否必须? |
|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------|
| [`--script-help`](https://github.com/o3de/o3de/blob/753480ec930e55f3431f92ed7b974ba7f9e73a13/scripts/o3de/ExportScripts/export_source_built_project.py#L202)                         | 显示专门针对导出脚本的帮助信息。                                                                                                                                                                                                                 | 否     |
| [`--output-path`](https://github.com/o3de/o3de/blob/753480ec930e55f3431f92ed7b974ba7f9e73a13/scripts/o3de/ExportScripts/export_source_built_project.py#L203)                         | 定义项目的发布布局目录在系统中的位置。对于每个所需的启动器，都将准备一个单独的发布文件夹。                                                                                                                                                                                    | 是     |
| [`--config`](https://github.com/o3de/o3de/blob/753480ec930e55f3431f92ed7b974ba7f9e73a13/scripts/o3de/ExportScripts/export_source_built_project.py#L204)                              | 定义构建项目二进制文件（如 GameLauncher）时的 CMake 构建配置。选项为 `profile` 或 `release`。默认为 `profile`                                                                                                                                                 | 否     |
| [`--archive-output`](https://github.com/o3de/o3de/blob/753480ec930e55f3431f92ed7b974ba7f9e73a13/scripts/o3de/ExportScripts/export_source_built_project.py#L206)                      | 自动将输出的发布目录归档为捆绑的归档文件，如 zip 文件。                                                                                                                                                                                                   | 否     |
| [`--should-build-assets`](https://github.com/o3de/o3de/blob/753480ec930e55f3431f92ed7b974ba7f9e73a13/scripts/o3de/ExportScripts/export_source_built_project.py#L210)                 | 这将根据资产处理器设置处理项目中的所有资产，并针对指定的种子列表文件运行资产捆绑程序。如果不想处理任何资产，请省略此项。如果资产尚未存在，发布包将不会运行。因此，如果资产尚未存在，建议加入此标记。                                                                                                                               | 否     |
| [`--fail-on-asset-errors`](https://github.com/o3de/o3de/blob/753480ec930e55f3431f92ed7b974ba7f9e73a13/scripts/o3de/ExportScripts/export_source_built_project.py#L212)                | 如果遇到资产错误，强制中断导出过程。默认情况下不启用，因为资产错误很常见，通常也不严重。如果需要更严格的验证，请使用此选项。                                                                                                                                                                   | 否     |
| [`--seedlist`](https://github.com/o3de/o3de/blob/753480ec930e55f3431f92ed7b974ba7f9e73a13/scripts/o3de/ExportScripts/export_source_built_project.py#L214)                            | 用于资产捆绑的种子列表文件的路径。为每个种子列表指定多次                                                                                                                                                                                                     | 否     |
| [`--project-file-pattern-to-copy`](https://github.com/o3de/o3de/blob/753480ec930e55f3431f92ed7b974ba7f9e73a13/scripts/o3de/ExportScripts/export_source_built_project.py#L220)        | 项目目录中的任何其他文件模式。文件模式将相对于项目路径。对于任何传统上与 O3DE 项目无关的文件，如自定义配置文件、元数据或许可证文件，请使用此选项。                                                                                                                                                     | 否     |
| [`--game-project-file-pattern-to-copy`](https://github.com/o3de/o3de/blob/753480ec930e55f3431f92ed7b974ba7f9e73a13/scripts/o3de/ExportScripts/export_source_built_project.py#L216)   | 像`--project-file-pattern-to-copy`， 但仅限于游戏启动器的文件。                                                                                                                                                                                 | 否     |
| [`--server-project-file-pattern-to-copy`](https://github.com/o3de/o3de/blob/753480ec930e55f3431f92ed7b974ba7f9e73a13/scripts/o3de/ExportScripts/export_source_built_project.py#L218) | 像 `--project-file-pattern-to-copy`， 但仅限于服务器启动器的文件。                                                                                                                                                                               | 否     |
| [`--build-tools`](https://github.com/o3de/o3de/blob/753480ec930e55f3431f92ed7b974ba7f9e73a13/scripts/o3de/ExportScripts/export_source_built_project.py#L222)                         | 指定是否构建 O3DE 工具链可执行文件。这将构建 AssetBundlerBatch 和 AssetProcessorBatch。如果资产工具尚未构建，则无法处理资产。                                                                                                                                            | 否     |
| [`--tools-build-path`](https://github.com/o3de/o3de/blob/753480ec930e55f3431f92ed7b974ba7f9e73a13/scripts/o3de/ExportScripts/export_source_built_project.py#L224)                    | 指定 O3DE 工具链的构建文件生成位置。如果未指定，默认值为 `<o3de_project_path>/build/tools`。                                                                                                                                                               | 否     |
| [`--launcher-build-path`](https://github.com/o3de/o3de/blob/753480ec930e55f3431f92ed7b974ba7f9e73a13/scripts/o3de/ExportScripts/export_source_built_project.py#L226)                 | 指定启动器构建文件（Game/Server/Unified）的生成位置。如果未指定，默认值为 `<o3de_project_path>/build/launcher`.                                                                                                                                             | 否     |
| [`--allow-registry-overrides`](https://github.com/o3de/o3de/blob/753480ec930e55f3431f92ed7b974ba7f9e73a13/scripts/o3de/ExportScripts/export_source_built_project.py#L228)            | 在配置 cmake 联编时，这将决定脚本是否允许覆盖来自外部的注册表设置。这些覆盖设置将在构建游戏二进制文件时使用，特别是在启用 CMake 构建标志时 `DALLOW_SETTINGS_REGISTRY_DEVELOPMENT_OVERRIDES`。要了解更多信息， 请阅读[设置注册表概述](https://www.docs.o3de.org/docs/user-guide/settings/developer-documentation/) | 否     |
| [`--asset-bundling-path`](https://github.com/o3de/o3de/blob/753480ec930e55f3431f92ed7b974ba7f9e73a13/scripts/o3de/ExportScripts/export_source_built_project.py#L230)                 | 指定在创建软件包之前将资产捆绑过程中的工件写入到何处。如果未指定，默认值为 `<o3de_project_path>/build/asset_bundling`.                                                                                                                                                | 否     |
| [`--max-bundle-size`](https://github.com/o3de/o3de/blob/753480ec930e55f3431f92ed7b974ba7f9e73a13/scripts/o3de/ExportScripts/export_source_built_project.py#L232)                     | 指定给定资产包的最大尺寸。                                                                                                                                                                                                                    | 否     |
| [`--no-game-launcher`](https://github.com/o3de/o3de/blob/753480ec930e55f3431f92ed7b974ba7f9e73a13/scripts/o3de/ExportScripts/export_source_built_project.py#L233)                    | 如果不需要，该标记会跳过在平台上构建游戏启动器。                                                                                                                                                                                                         | 否     |
| [`--no-server-launcher`](https://github.com/o3de/o3de/blob/753480ec930e55f3431f92ed7b974ba7f9e73a13/scripts/o3de/ExportScripts/export_source_built_project.py#L234)                  | 如果不需要，该标记会跳过在平台上构建服务器启动器。                                                                                                                                                                                                        | 否     |
| [`--no-headless-server-launcher`](https://github.com/o3de/o3de/blob/753480ec930e55f3431f92ed7b974ba7f9e73a13/scripts/o3de/ExportScripts/export_source_built_project.py#L235)         | 如果不需要，该标记会跳过在平台上构建无头服务器启动器。                                                                                                                                                                                                      | 否     |
| [`--no-unified-launcher`](https://github.com/o3de/o3de/blob/753480ec930e55f3431f92ed7b974ba7f9e73a13/scripts/o3de/ExportScripts/export_source_built_project.py#L236)                 | 如果不需要，该标记会跳过在平台上构建统一启动器。                                                                                                                                                                                                         | 否     |
| [`--platform`](https://github.com/o3de/o3de/blob/753480ec930e55f3431f92ed7b974ba7f9e73a13/scripts/o3de/ExportScripts/export_source_built_project.py#L237)                            | 处理和构建资产的预期目标平台。如果未指定，将使用用户的主机平台。                                                                                                                                                                                                 | 否     |
| [`--engine-centric`](https://github.com/o3de/o3de/blob/753480ec930e55f3431f92ed7b974ba7f9e73a13/scripts/o3de/ExportScripts/export_source_built_project.py#L238)                      | 使用以引擎为中心的工作流程导出项目。                                                                                                                                                                                                               | 否     |
| [`--quiet`](https://github.com/o3de/o3de/blob/753480ec930e55f3431f92ed7b974ba7f9e73a13/scripts/o3de/ExportScripts/export_source_built_project.py#L239)                               | 除非发生错误，否则禁止记录信息。                                                                                                                                                                                                                 | 否     |

整个 `export-project` 命令（包括本脚本）的使用示例可参见 O3DE MultiplayerSample 项目，以下是用于 Windows 的示例：

```
# On Windows
%O3DE_ENGINE_PATH%\scripts\o3de.bat export-project \
    --export-script %O3DE_ENGINE_PATH%\scripts\o3de\ExportScripts\export_source_built_project.py \
    --project-path %O3DE_PROJECT_PATH% -out %OUTPUT_PATH% \
    --config release \
    --archive-output zip -nounified \
    --game-project-file-pattern-to-copy launch_client.cfg \
    --server-project-file-pattern-to-copy launch_client.cfg \
    --should-build-assets \
    --log-level INFO \
    --seedlist \path\to\o3de-multiplayersample\AssetBundling\SeedLists\BasePopcornFxSeedList.seed \
    --seedlist %O3DE_PROJECT_PATH%\AssetBundling\SeedLists\GameSeedList.seed \
    --seedlist %O3DE_PROJECT_PATH%\AssetBundling\SeedLists\VFXSeedList.seed 
```
其中 `O3DE_ENGINE_PATH`、`O3DE_PROJECT_PATH` 和 `OUTPUT_PATH` 是环境变量。只需调用一次即可将 MultiplayerSample 完全导出到发行版目录中。

有关如何使用 CLI 导出 MultiplayerSample 项目的更多信息，请参阅 [这些说明](https://github.com/o3de/o3de-multiplayersample/blob/f00b3035285b695b2dbd1b1e59912973f4e1a32f/Documentation/O3DEMPSProjectExportTesting.md)。

### 自定义脚本

您可以学习标准导出脚本以了解 `export-project` API 如何工作，但本节将帮助您对可用内容有一个高层次的概览。

#### O3DE 上下文和记录器
运行 `export-project` 命令时，所有导出脚本都会自动注入 2 个全局变量： `o3de_context`和 `o3de_logger`。

`o3de_context` 是[export_project.O3DEScriptExportContext](https://github.com/o3de/o3de/blob/753480ec930e55f3431f92ed7b974ba7f9e73a13/scripts/o3de/o3de/export_project.py#L104) 类的一个实例。该上下文对象用于在导出脚本的整个执行过程中存储参数值和变量。它还存储了一些方便的属性，如导出脚本的路径、项目路径、引擎路径、未处理参数、cmake 特定参数以及 project.json 文件中的项目名称。

对于 `export-project` 的任何期望参数类型为 `O3DEScriptExportContext` 的 API，您可以传入正在运行的导出脚本的 `o3de_context`。


`o3de_logger` 是 Python 标准库中的 `logging.Logger` 类的实例，可用于跟踪和发布脚本生命周期中的日志。

#### API
对于希望创建自定义导出脚本的用户，`export-project` 命令提供了以下 API（请单击函数名称中的链接查看参数详情）：

| 函数名 | 说明 |
| - | - | 
| [get_default_asset_platform](https://github.com/o3de/o3de/blob/753480ec930e55f3431f92ed7b974ba7f9e73a13/scripts/o3de/o3de/export_project.py#L192) | 根据 O3DE 约定获取用户的主机平台。 |
| [process_command](https://github.com/o3de/o3de/blob/753480ec930e55f3431f92ed7b974ba7f9e73a13/scripts/o3de/o3de/export_project.py#L198) | 允许用户以子进程的形式运行终端命令，可以是以字符串形式提供完整命令，也可以是以字符串列表形式提供完整命令。 |
| [execute_python_script](https://github.com/o3de/o3de/blob/753480ec930e55f3431f92ed7b974ba7f9e73a13/scripts/o3de/o3de/export_project.py#L214) | 使用新的或现有的 O3DEScriptExportContexts 执行新的 python 脚本，以简化多个脚本之间的通信。 |
| [get_asset_processor_batch_path](https://github.com/o3de/o3de/blob/753480ec930e55f3431f92ed7b974ba7f9e73a13/scripts/o3de/o3de/export_project.py#L371) | 获取资产处理器工具的预期路径。 |
| [get_asset_bundler_batch_path](https://github.com/o3de/o3de/blob/753480ec930e55f3431f92ed7b974ba7f9e73a13/scripts/o3de/o3de/export_project.py#L386) | 获取资产捆绑工具的预期路径。 |
| [build_assets](https://github.com/o3de/o3de/blob/753480ec930e55f3431f92ed7b974ba7f9e73a13/scripts/o3de/o3de/export_project.py#L402) | 为项目构建资产。 |
| [build_export_toolchain](https://github.com/o3de/o3de/blob/753480ec930e55f3431f92ed7b974ba7f9e73a13/scripts/o3de/o3de/export_project.py#L439) | 构建（或重建）导出工具链（AssetProcessorBatch 和 AssetBundlerBatch）。 |
| [validate_export_toolchain](https://github.com/o3de/o3de/blob/753480ec930e55f3431f92ed7b974ba7f9e73a13/scripts/o3de/o3de/export_project.py#L497) | 验证导出过程所需的命令行工具是否可用。 |
| [build_game_targets](https://github.com/o3de/o3de/blob/753480ec930e55f3431f92ed7b974ba7f9e73a13/scripts/o3de/o3de/export_project.py#L512) | 为项目构建启动器（game, server, unified, headless）。 |
| [bundle_assets](https://github.com/o3de/o3de/blob/753480ec930e55f3431f92ed7b974ba7f9e73a13/scripts/o3de/o3de/export_project.py#L598) | 执行导出的 “捆绑资产 ”阶段。 |
| [setup_launcher_layout_directory](https://github.com/o3de/o3de/blob/753480ec930e55f3431f92ed7b974ba7f9e73a13/scripts/o3de/o3de/export_project.py#L699) | 设置启动器布局目录的路径。 |
| [validate_project_artifact_paths](https://github.com/o3de/o3de/blob/753480ec930e55f3431f92ed7b974ba7f9e73a13/scripts/o3de/o3de/export_project.py#L759) | 根据需要验证和调整项目工件路径。如果提供的路径是相对路径，则对照项目路径检查其是否存在。 |

#### 自定义脚本示例

为了演示 API 的基本功能，我们将使用 2 个函数来查看一个示例脚本： `execute_python_script` 和 `process_command`，它们是其余功能的基础模块。

##### test_export.py
下面是一个测试导出脚本示例：
```python
#test_export.py
import os
import o3de.export_project as exp

from o3de.export_project import process_command, execute_python_script


o3de_context.message1 = "This is a message!"
o3de_context.message2 = "___"
o3de_context.hi_there = "hi there!"
o3de_context.hi_there_2 = 22


process_command(["cmake", "--version"])


execute_python_script("C:\\Users\\O3DE_User\\Desktop\\hello.py", o3de_context)

o3de_logger.info(f"Now running the export code {o3de_context.project_path}")
o3de_logger.info(f"Engine path: {o3de_context.engine_path}")

o3de_logger.info(o3de_context.hello_back)

o3de_logger.info(o3de_context.message2)
```

首先要注意的是，在顶部，用户可以导入现有的 O3DE Python 模块，如 `export_project`、`manifest`、`utils`、`validation` 等，就像通常导入标准库中的其他 python 模块一样。所有这些模块都可以在 [Github 上找到]((https://github.com/o3de/o3de/tree/development/scripts/o3de/o3de)。这是因为 `export-project` 工具会在幕后处理系统路径，以便从导入系统访问 O3DE CLI 文件夹和导出脚本的文件夹。

接下来要注意的是，`o3de_context`可作为全局上下文使用。除了标准属性外，用户还可以添加自己的自定义值，以便在脚本的生命周期内保留这些值。

使用 `process_command`，用户可以运行任何他们想要的终端命令。这使得将手动进程转换为可用脚本变得非常容易（大多数情况下，如果你愿意，可以单独使用 `process_command`）。你所需要做的就是确保进程有一个可在终端上运行的等效命令。

在这里，函数 `export_python_script` 做了一些特别的事情。它能在机器上的任何地方运行另一个 python 脚本，同时为其提供与当前导出脚本相同的 `o3de_context`。这可以实现非常模块化的脚本能力。让我们仔细看看 `hello.py` 的功能。

##### hello.py
```python
#hello.py
import o3de.export_project as exp


i = 0

while i < 10:
    print(f"hi {i}:: {o3de_context.hi_there_2}")
    i+=1

o3de_logger.info(o3de_context.hi_there)
o3de_logger.info(o3de_context.message1)
o3de_logger.info(o3de_context.project_path)


o3de_context.hello_back = "Hello to you too!"

o3de_context.message2 = "hiya!"
```

`hello.py` 很容易获取原始导出脚本的上下文，甚至能够访问先前定义的变量，如 `hi_there_2`、`hi_there`、`message1` 和 `message2`。

它不仅能读取这些变量，还能覆盖现有变量，例如将 `message2` 从 `'____'` 改为 `'hiya!'` ，甚至引入新变量，如 `hello_back` ，在完成 `hello.py` 后，`test_export.py` 脚本就能看到这些变量。

##### 结果
如果在 Windows 上运行 `test_export.py` 脚本，日志会显示如下内容：
```
<engine-path>\scripts\o3de.bat export-project -es C:\workspace\projects\NewspaperDeliveryGame\export_rules\test_export.py -ll INFO

[INFO] root: Begin loading script 'C:\workspace\projects\NewspaperDeliveryGame\export_rules\test_export.py'...
[INFO] root: Running process 'cmake' with PID(28996): ['cmake', '--version']
[INFO] root: cmake version 3.24.1

[INFO] root:

[INFO] root: CMake suite maintained and supported by Kitware (kitware.com/cmake).

[INFO] root:
[INFO] root: Terminating process 'cmake' with PID(28996)
[INFO] root: process 'cmake' with PID(28996) terminated with exit code 0
[INFO] root: Begin loading script 'C:\Users\O3DE_User\Desktop\hello.py'...
hi 0:: 22
hi 1:: 22
hi 2:: 22
hi 3:: 22
hi 4:: 22
hi 5:: 22
hi 6:: 22
hi 7:: 22
hi 8:: 22
hi 9:: 22
[INFO] root: hi there!
[INFO] root: This is a message!
[INFO] root: C:\workspace\projects\NewspaperDeliveryGame
[INFO] root: Now running the export code C:\workspace\projects\NewspaperDeliveryGame
[INFO] root: Engine path: C:\workspace\o3de
[INFO] root: Hello to you too!
[INFO] root: hiya!
[INFO] o3de: Success!
```

本例基于 [项目导出工具的原始 PR](https://github.com/o3de/o3de/pull/15463)。
