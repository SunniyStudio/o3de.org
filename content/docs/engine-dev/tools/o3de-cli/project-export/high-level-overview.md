---
linkTitle: 概述
title: 项目导出概述
description: 项目导出命令的高级开发人员概述。
---

## 动机

在 O3DE 23.10 版本发布之前，导出游戏项目以供共享对于新用户来说是一项非常困难的任务。要跟踪处理游戏所需的工具，需要与多个独立部分（如 CMake、AssetProcessor、AssetBundler 等）进行交互，这给用户带来了很大的认知负担。对于经验丰富的用户来说，最常见的任务也缺乏统一的自动化。

这导致导出体验极易出错。

以 2022 年第四季度为例，一位工程师手动打包报纸交付游戏演示项目就花费了 1 周时间。而使用 “项目导出 ”命令完成同样的过程只需 2-15 分钟。

项目导出命令还遵循了 O3DE 生态系统其他部分的模块化精神，因为在开源生态系统中，各种需求会随着时间的推移而增长。

## 命令设置
`export-project` 命令有以下设置：

![Project Export CLI setup](/images/engine-dev/o3de-cli/project-export/project-export-cli-setup.png)

该命令的核心驱动力是导出脚本。在 O3DE 中，导出脚本是用户希望使用 O3DE Python CLI 运行的任何 Python 脚本。

其工作原理是，O3DE CLI（Windows 上的 `o3de.bat` 或 Linux 上的 `o3de.sh`）通过指定的[entry-hook](https://github.com/o3de/o3de/blob/development/scripts/o3de/o3de/export_project.py#L484) [调用](https://github.com/o3de/o3de/blob/development/scripts/o3de.py#L107-L108) `export-project` [sub-command argument](https://github.com/o3de/o3de/blob/development/scripts/o3de.py#L79)。该系统在很大程度上依赖于 `argparse` 标准库，以模块化方式为解析后的命令配置预期函数。

请注意，`export-project` 只对部分参数进行解析。这是因为剩余的参数会在主导出脚本运行时反馈给它，而主导出脚本有自己的参数解析逻辑。

## 执行路径
入口函数名为 [`_run_export_script`](https://github.com/o3de/o3de/blob/development/scripts/o3de/o3de/export_project.py#L367)，其结构是在以下各层开始设置导出过程：

##### Layer 1: **`_run_export_script`**  
启用日志记录并将用户参数输入导出系统。
##### Layer 2: **[`_export_script`](https://github.com/o3de/o3de/blob/development/scripts/o3de/o3de/export_project.py#L307)** 
验证用户提供的导出脚本和项目路径，并确定各自的位置（如果用户提供的是相对路径）。在某些情况下，该层将检查用户是否要求使用 O3DE 标准导出脚本。

如果在验证过程中出现任何错误，这一层就会停止。否则，它将构建一个 [O3DE Context](https://github.com/o3de/o3de/blob/development/scripts/o3de/o3de/export_project.py#L112) 对象并进入下一层。
##### Layer 3: **[`execute_python_script`](https://github.com/o3de/o3de/blob/development/scripts/o3de/o3de/export_project.py#L235)** 
运行用户提供的主导出脚本。它首先使用 [预输入系统路径](https://github.com/o3de/o3de/blob/development/scripts/o3de/o3de/utils.py#L154)修改会话的系统路径，以包含 O3DE CLI 文件夹以及用户导出脚本的文件夹。这样，用户只需使用 `import o3de` 即可轻松导入 API 功能。从这里开始，第四层实际执行脚本。

`execute_python_script`是可用于任何导出脚本的自定义 API 之一。

##### Layer 4: **[load_and_execute_script](https://github.com/o3de/o3de/blob/development/scripts/o3de/o3de/utils.py#L167)**
该函数使用 Python 的 `importlib` 功能加载外部脚本。它还使用 `setattr` 为脚本模块注入在第 2 层创建的 O3DE Context。这样就可以在脚本运行时无缝使用 O3DE 导出数据。

执行这四层将成功加载并运行用户的导出脚本，为其设置内置的 O3DE 上下文和必要的路径环境，以便原生使用 O3DE Python API。

## 标准导出脚本

尽管 O3DE 提供的每个标准导出脚本都有特定的平台考虑因素，但它们都遵循类似的模式。在本概述中，我们将了解 [PC 导出脚本](https://github.com/o3de/o3de/blob/development/scripts/o3de/ExportScripts/export_source_built_project.py)。

该导出脚本包含一个 [入口点](https://github.com/o3de/o3de/blob/development/scripts/o3de/ExportScripts/export_source_built_project.py#L424-L446) ，只有通过 `export-project` 命令输入脚本后才能运行，因为需要定义 `o3de_context`。

此外，还定义了以下顶级功能： [`export_standalone_parse_args`](https://github.com/o3de/o3de/blob/development/scripts/o3de/ExportScripts/export_source_built_project.py#L224), [`export_standalone_run_command`](https://github.com/o3de/o3de/blob/development/scripts/o3de/ExportScripts/export_source_built_project.py#L336), 和 [`export_standalone_project`](https://github.com/o3de/o3de/blob/development/scripts/o3de/ExportScripts/export_source_built_project.py#L24)。

这些功能被定义为顶级应用程序接口，这样其他系统就可以利用导出脚本的功能，将其作为带有自定义参数的外部模块。

`export_standalone_parse_args` 利用 [O3DE 配置系统](https://github.com/o3de/o3de/blob/development/scripts/o3de/o3de/command_utils.py#L133) 和 `argparse` 为用户请求的特定导出任务确定适当的设置配置 [`export_utility`](https://github.com/o3de/o3de/blob/development/scripts/o3de/ExportScripts/export_utility.py) 包含函数 `create_common_export_params_and_config` ，用于定义所有 O3DE 项目跨平台的通用参数。所有其他 PC 特定参数（如服务器或客户端、归档格式、操作系统平台、要复制的文件等）都直接在 `export_standalone_parse_args` 中处理。

`export_standalone_run_command` 接受先前指定的参数，验证参数，并假定一切正常，然后继续调用 `export_standalone_project`。

`export_standalone_project` 是负责将项目制作成 PC 版游戏的工作母机。以下是这一过程的主要步骤：
1. 确定引擎类型（源代码已构建或 SDK 已安装），验证路径和参数，并确定打包的级别和种子列表。 [代码](https://github.com/o3de/o3de/blob/development/scripts/o3de/ExportScripts/export_source_built_project.py#L84-L128)
1. 确定是否要构建工具链。 [代码](https://github.com/o3de/o3de/blob/development/scripts/o3de/ExportScripts/export_source_built_project.py#L133-L141)
1. 准备和捆绑资产。 [代码](https://github.com/o3de/o3de/blob/development/scripts/o3de/ExportScripts/export_source_built_project.py#L144-L156)
1. 构建游戏的目标可执行文件。 [代码](https://github.com/o3de/o3de/blob/development/scripts/o3de/ExportScripts/export_source_built_project.py#L159-L178)
1. 准备输出软件包的输出目录布局。 [代码](https://github.com/o3de/o3de/blob/development/scripts/o3de/ExportScripts/export_source_built_project.py#L181-L208)
1. 将所有相关文件复制到该处。 [代码](https://github.com/o3de/o3de/blob/development/scripts/o3de/ExportScripts/export_source_built_project.py#L211-L221)


## API
以下是构成系统其余部分的一些应用程序接口的详细介绍：

##### **[`process_command`](https://github.com/o3de/o3de/blob/development/scripts/o3de/o3de/export_project.py#L235)**
这会使用提供的参数和工作目录运行 [`CLICommand`](https://github.com/o3de/o3de/blob/development/scripts/o3de/o3de/utils.py#L68) 对象的一个实例。这将有效地调用 `subprocess.Popen`，并将所有与 CLI 相关的执行打包到 [`busy loop`](https://github.com/o3de/o3de/blob/development/scripts/o3de/o3de/utils.py#L110-L116)  中，直至进程结束。它还会封装所有 stdout 和 stderr 日志，并将其转发给导出脚本，这样用户就能在命令执行过程中看到所有日志。

通过这种方式封装子进程，我们可以[清理](https://github.com/o3de/o3de/blob/development/scripts/o3de/o3de/utils.py#L118-L124) 并安全地终止调用的所有进程。

##### **[`build_assets`](https://github.com/o3de/o3de/blob/development/scripts/o3de/o3de/export_project.py#L583)** 
首先，根据 O3DE 是源代码内置引擎还是 SDK 安装引擎，计算 `AssetProcessorBatch` 可执行文件的路径。在此基础上，使用 `process_command` 为项目和所选平台运行带有参数的批处理工具。

##### **[`bundle_assets`](https://github.com/o3de/o3de/blob/development/scripts/o3de/o3de/export_project.py#L797)** 
这将计算 `AssetBundlerBatch` 可执行文件的路径，并为捆绑设置相关文件夹。然后，它会遍历每个所需的平台，并执行以下操作：
1. 为游戏平台创建资产列表文件。[代码](https://github.com/o3de/o3de/blob/development/scripts/o3de/o3de/export_project.py#L844)
2. 为平台引擎创建资产列表文件。 [代码](https://github.com/o3de/o3de/blob/development/scripts/o3de/o3de/export_project.py#L855)
3. 使用步骤 1 中的资产列表，创建游戏的 bundle pak 文件。 [代码](https://github.com/o3de/o3de/blob/development/scripts/o3de/o3de/export_project.py#L868)
4. 使用步骤 2 中的资产列表，创建引擎的 bundle pak 文件。 [代码](https://github.com/o3de/o3de/blob/development/scripts/o3de/o3de/export_project.py#L880)

##### **[`build_game_target`](https://github.com/o3de/o3de/blob/development/scripts/o3de/o3de/export_project.py#L704)** 
根据游戏启动器的类型、游戏是否为单体以及项目是否以引擎为中心，该函数会建立相关的 CMake 参数，并传递用户在调用 `export-project` 时指定的其他 CMake 参数。然后运行[configure](https://github.com/o3de/o3de/blob/development/scripts/o3de/o3de/export_project.py#L765)命令，接着运行[build](https://github.com/o3de/o3de/blob/development/scripts/o3de/o3de/export_project.py#L790) 命令。


##### **[`setup_launcher_layout_directory`](https://github.com/o3de/o3de/blob/development/scripts/o3de/o3de/export_project.py#L704)** 

设置导出布局目录需要以下步骤：
1. 重置目录文件夹结构，并清除旧文件。 [代码](https://github.com/o3de/o3de/blob/development/scripts/o3de/o3de/export_project.py#L934-L939)
2. 复制所有未忽略的捆绑包和配置文件。 [代码](https://github.com/o3de/o3de/blob/development/scripts/o3de/o3de/export_project.py#L943-L959)
3. 从用户指定的任何其他文件模式复制文件。 [代码](https://github.com/o3de/o3de/blob/development/scripts/o3de/o3de/export_project.py#L961-L963)
4. 如果我们使用配置文件模式，请添加一个额外的 setregpatch 文件，以便在不使用 AssetProcessor 的情况下独立运行。[代码](https://github.com/o3de/o3de/blob/development/scripts/o3de/o3de/export_project.py#L968-L975)
5. 如果用户指定，则将布局目录存档（例如作为 zip 文件）。 [代码](https://github.com/o3de/o3de/blob/development/scripts/o3de/o3de/export_project.py#L978-L982)
