---
title: 创建可分发的Open 3D Engine 构建
linktitle: 引擎和项目分发
description: 了解构建完整的库和可执行文件包的过程，您可以将这些库和可执行文件包分发给内部团队，让他们使用您的定制版 Open 3D Engine (O3DE)。
weight: 200
---

通常情况下，项目开发人员所在的团队中，工程师和其他人员会根据内部需要修改核心 **Open 3D Engine (O3DE)** 代码。在这种情况下，您需要自定义 O3DE 和项目的可分发构建（也称为 “预构建 SDK 引擎”），这样您的团队就可以使用内部提供的相同技术栈进行协作。

本主题将指导您创建一个引擎构建，作为项目开发的一部分在内部分发，该构建可由项目专用团队使用，同时保持引擎专用团队的工作分离和独立。

本节概述的步骤适合构建工程师建立定期创建和发布新构建的持续集成 (CI) 系统，也适合需要为较小团队定期进行一次性构建的开发人员。

## 先决条件

创建可分发版本所需的先决条件是

* O3DE 源代码。
* 可发布的构建版的托管位置，如源代码控制。

## 示例值

这些说明使用了以下示例路径：

* O3DE 源代码路径： `C:\o3de-source`
* 项目目录： `W:\MyProject`

这些说明使用以下 CMake 缓存值示例：

* `O3DE_INSTALL_ENGINE_NAME`: "MyO3DE"

{{< note >}}
本示例使用 Visual Studio 生成器和 Windows `cmd` 命令行指令语法。对于 Linux 用户，请将 CMake 生成器更改为`Ninja Multi-Config`，使用不同的 CMake 生成目录，并相应修改 CLI 语法。出于性能考虑，确保 Linux 系统上的可分发引擎托管在 `ext*` 格式的文件系统上。

对于多平台项目，如果项目团队将使用可分发的构建版，则应通过 `CMAKE_INSTALL_PREFIX` 将每个可分发的构建版放置在不同的目录中。
{{< /note >}}

## 概述

为项目开发人员创建一个可分发的构建，同时将核心引擎开发人员保留在一个单独的路径中，这涉及以下主要步骤：

1. 使用 `install` 目标构建本地 O3DE SDK
1. 配置您的项目以使用本地 SDK 进行构建
1. 构建项目
1. 创建可分发的引擎构建
1. 向项目开发人员提供可分发的构建版

最后一步所涉及的具体流程因内部开发实践而异，不过我们建议使用具有良好 blob 支持的版本控制来进行构建发布。对于使用开源解决方案的团队，我们推荐 [git LFS](https://git-lfs.github.com/)。

在创建 O3DE 核心工具的新可发布构建版时，应在每次创建新构建版时执行每个步骤，以确保正确接收任何影响引擎或项目配置的引擎变更。

## 创建本地 SDK

创建可分发构建的第一步是从引擎源代码生成本地二进制文件，用于构建项目和创建最终的可分发引擎构建。

### 构建`INSTALL`目标

从 O3DE 源代码目录（本例中为 `C:\o3de-source`）执行以下步骤：

1.  使用唯一的 `O3DE_INSTALL_ENGINE_NAME` CMake 缓存设置生成工具链项目文件。该值是 **Project Manager** 和 O3DE 注册系统使用的引擎名称。为安装布局赋予一个与源代码引擎不同的引擎名称，可使引擎并行注册。
    ```cmd
    cmake --preset windows-default -DO3DE_INSTALL_ENGINE_NAME=MyO3DE
    ```

    可以考虑在这一行设置的其他缓存变量（使用 `-D` 选项）包括

    * `LY_3RDPARTY_PATH` : 自定义可下载软件包目录的路径，也称为 “第三方路径”。指定软件包目录路径时，请勿使用斜线。
    * `CMAKE_INSTALL_PREFIX`: 包含可分发二进制文件的 `bin` 目录的父目录。你可以在子目录 `bin\Windows\profile\Default` 中找到项目管理器、编辑器和其他二进制文件。如果不指定此选项，引擎 SDK 二进制文件将被构建到`ENGINE_SOURCE>\install\bin\Windows\profile\Default`。
    * `LY_DISABLE_TEST_MODULES` : 设置为 `ON` ，则不构建可选测试模块，并减少整个 SDK 的文件大小和数量。 

    {{< note >}}
    Visual Studio 2019 使用`Visual Studio 16` 作为生成器，Visual Studio 2022 使用 `Visual Studio 17` 作为生成器。有关各支持平台的常用生成器的完整列表，请参阅 [配置项目](../configure-and-build/#configuring-projects)。
    {{< /note >}}

    建议在分布式构建时使用 **profile** 配置，因为它能以最小的性能代价提供额外的日志记录和引擎自省功能，在项目开发过程中非常有用。**debug** 构建主要用于引擎开发。

1.  构建`INSTALL`目标。

    ```cmd
    cmake --build --preset windows-install --config profile
    ```

    二进制文件将被放置到 CMake 缓存变量 `CMAKE_INSTALL_PREFIX` 指定的可分发安装目录，或默认的源代码的 `install` 子目录。

从可分发的安装目录运行以下步骤：

1. 安装引擎所需的 Python 版本和模块。
   
   ```cmd
   python\get_python.bat
   ```

### 注册引擎

创建引擎工具的可分发版本的下一步是注册本地安装的引擎。每次更改 `O3DE_INSTALL_ENGINE_NAME` 时，都必须重新执行此注册步骤。

使用 `o3de` 脚本在本地注册引擎。从您的可分发安装目录运行以下命令：
    
```cmd
scripts\o3de.bat register --this-engine
```

{{< note >}}
如果系统中现有的 `o3de_manifest.json` 文件存在冲突，引擎注册将失败，而 `o3de` 输出将提供如何修复错误的信息。我们建议通过脚本自动执行此过程（适用于需要快速迭代新版本的团队），或为每个新的可分发版本使用唯一的引擎名称（适用于需要稳定性、在源控制之外回滚到以前的版本或不经常更新可分发版本的团队）。
{{< /note >}}

此时，已安装的引擎构建已在 O3DE 配置目录（默认为 `%USERPROFILE%\.o3de`）中的 `o3de_manifest.json` 文件中注册。

## 项目关联

此时，您可以创建一个新项目用于分发，或者将现有项目与用于创建可分发项目的安装构建重新关联。

### 创建新项目

从可分发的安装目录运行以下命令：
   
```cmd
scripts\o3de.bat create-project -pp W:\MyProject
```

### 更新现有项目

如果要更新现有项目而不是创建一个新项目，则执行项目更新的用户有责任确保任何更改在任何远程资源（如源控制）中都是可用的，因为在引擎关联更新期间至少要修改 `project.json`。

使用 `o3de` 脚本在安装构建时重新注册项目。这将用于为项目创建可分发的构建。
   
```cmd
scripts\o3de.bat register -pp W:\MyProject
```

`project.json` 文件中的 `engine` 字段现在将是配置时设置为 `O3DE_INSTALL_ENGINE_NAME` 的引擎名称。

## 执行项目构建

无论您是创建了一个新项目还是更新了一个现有项目，为了创建最终的可发布版本，您都需要构建项目和最终的引擎二进制文件，以便检查到源代码控制。

在项目目录中，配置并构建项目。
    
```cmd
cmake -B build/windows -S .
cmake --build build/windows --config profile
```

## (可选）创建并加载初始关卡

为了帮助将要使用这些工具（但不是构建这些工具）的创意团队，我们建议创建一个初始关卡，作为项目工作的第一步，供他们加载。

1. 启动编辑器的本地构建并加载项目。
    
    ```cmd
    C:\o3de\bin\Windows\profile\Default\Editor.exe --project-path W:\MyProject
    ```
1. 在 “资产处理器”（Asset Processor）完成并加载 “编辑器”（Editor）后，如果您的项目尚未配置加载关卡，您将看到一个对话框，指示您加载或创建一个关卡。在本例中，它被命名为 “MyLevel”。
1. 退出编辑器。
1. 为你的项目创建或修改注册表文件，文件名为`<project-location>\Registry\autoexec.game.setreg`. 在本例中，它将位于 `W:\MyProject\Registry\autoexec.game.setreg`.

   在该文件中添加以下内容：
    ```
    {
        "O3DE": {
            "Autoexec": {
                "ConsoleCommands": {
                    "LoadLevel": "MyLevel"
                }
            }
        }
    }
    ```

要验证游戏启动时是否加载了关卡，请运行项目的游戏启动器：

```cmd
W:\MyProject\build\windows\bin\profile\MyProject.GameLauncher.exe
```

## 创建可分发的构建文件

创建二进制文件以分发给项目开发人员的最后一步是创建一套安装二进制文件，让他们可以在不从源代码构建引擎的情况下使用。就像为引擎创建本地二进制文件一样，现在也要为项目创建二进制文件。

在项目目录下运行以下步骤：

1. 创建项目的安装构建。
    
    ```cmd
    cmake --build build\windows --config profile --target INSTALL
    ```

   这会在项目的 `install` 子目录下创建二进制文件。在我们的例子中，子目录是 `W:\MyProject\install`。

## 发布您的构建

此时，创建的 `install` 目录就是您的最终可分发文件集。您的团队可以自行决定在哪里托管构建，但我们建议使用支持 blob 的源代码控制系统。

{{< important >}}
.gitignore 规则会自动添加供 git 忽略的文件夹。`.gitignore` 文件位于项目创建后的项目根目录中。如果使用的是其他源码控制系统，请确保项目的下列子目录被忽略且未签入：
    
    ```
    \user
    \build
    \Cache
    ```

这些文件是在构建过程中本地生成的，将它们检入可能会给开发人员带来问题。请参考项目根目录下的`.gitignore` 文件，确认要忽略的目录和文件列表。
{{< /important >}}

至此，您就可以让团队为引擎开发或项目开发进行本地设置了。

### 引擎开发设置

引擎开发人员将处理 O3DE 源代码并创建新的可分发版本。要启用他们的工作流程，请在他们的工作站上执行以下步骤：

1. 从托管位置获取修改后的引擎源代码。
1. 安装引擎所需的 Python 运行时和模块。在包含引擎源代码的目录下运行以下命令：

    ```cmd
    python\get_python.bat
    ```
1. 在本地注册引擎。在包含引擎源代码的目录下运行以下命令：

    ```cmd
    scripts\o3de.bat register --this-engine
    ```
1. 从托管位置获取项目。
1. 在**本地开发引擎**中注册项目。在包含引擎源代码的目录下运行以下命令：
   
   ```cmd
   scripts\o3de.bat register -pp W:\MyProject
   ```
1. 按照 [项目构建说明](https://o3de.org/docs/user-guide/build/) 生成任何特定于项目的构建，例如启动器。

### 项目开发人员设置

项目开发人员将处理特定项目的源代码，或使用 O3DE 创意工具添加项目内容。为了启用他们的工作流程，项目开发人员需要_只_从托管位置调用项目。

无论何时在项目上工作，项目开发人员都应启动作为可分离引擎构建版签入的工具。这些工具位于项目的 `install\bin\Windows\profile\Default` 子目录中。

### 许可证文件生成

许可证归属文件（通常称为 NOTICES 文件）可在项目开发过程中生成，以正确归属导入的任何代码或软件包。要扫描项目目录中的许可证，项目开发人员可以运行位于引擎的 `scripts\license_scanner` 子目录中的脚本。

在本例中，将引擎、项目和 `C:\o3de-source\3rdParty` 作为可下载包文件夹，在引擎源文件夹中运行以下脚本：
   ```cmd
   python\python.cmd scripts\license_scanner\license_scanner.py ^
     --config-file scripts\license_scanner\scanner_config.json ^
     --license-file-path build\windows_vs2019\NOTICES.txt ^
     --package-file-path build\windows_vs2019\spdx-packages.json ^
     --scan-path C:\o3de-source W:\MyProject C:\o3de-source\3rdParty
   ```
这将使用 `scanner_config.json` 文件中的默认配置扫描引擎、项目和 `C:\o3de-source\3rdParty` 文件夹中的许可证文件，然后在构建输出文件夹中生成 `NOTICES.txt`。此外，如果在`C:\o3de-source\3rdParty`中检测到`PackageInfo.json`文件，脚本将扫描该文件，并将每个软件包的许可证清单添加到名为`spdx-packages.json`的摘要文件中。

{{< note >}}
扫描器脚本将根据 `scanner_config.json` 文件的配置执行文件名扫描，但不保证能找到所有许可证文件，也不能捕捉递归依赖关系。为确保您的项目有足够的属性，我们推荐使用 [Scancode Toolkit](https://github.com/nexB/scancode-toolkit) 和 [Fossology](https://www.fossology.org) 等工具来进一步验证您的依赖关系。
{{< /note >}}
