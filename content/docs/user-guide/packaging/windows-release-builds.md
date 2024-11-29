---
linkTitle: Windows 版本Build
title: 创建适用于 Windows 的项目游戏发布布局
description: 了解如何创建适用于 Windows 的 Open 3D Engine （O3DE） 项目游戏发布布局。
toc: true
weight: 200
---

本教程将指导您完成为 Windows 计算机手动创建 **Open 3D Engine （O3DE）** *项目游戏发布布局* 的过程。项目游戏发布布局是一个目录结构，其中包含 **Game Launcher** 和在开发人员环境之外运行 Game Launcher 所需的捆绑资产。在构建要发布的项目时，您可以创建一个项目游戏发布布局，称为 *发布版本*。

{{< note >}}
如果您希望从源码使用引擎而不是安装程序，请参考 [从 GitHub 设置 O3DE](/docs/welcome-guide/setup/setup-from-github/)。
此外，您必须使用以项目为中心的配置，这将进一步详细说明 [下文](#create-a-project-game-release-layout)。
{{< /note >}}

发布版本需要 *捆绑内容*，其中包括存储在包 （`.pak`） 文件中的缓存产品资产。缓存的产品资源位于项目的  `Cache\pc`  目录中。Game Launcher 加载构成项目的捆绑内容，例如其关卡、对象、环境和游戏逻辑。

此处的说明将指导您完成以下步骤：

1. 设置起始级别。

2. 处理项目的资源。

3. 创建项目游戏发布布局。

4. 捆绑项目的资源。

5. 从项目游戏发布布局运行项目的 Game Launcher。

6. 分发您的构建。


## 先决条件

以下说明假定您已：

- 在您的计算机上将 O3DE 设置为 *源引擎* 或 *预构建/安装的 SDK 引擎*。如需帮助，请参阅 [为 Windows 构建](/docs/welcome-guide/setup/setup-from-github/building-windows).

- 已创建至少包含一个级别的 O3DE 项目。要构建要发布的项目，您可能需要解决项目中的所有错误。

- 为以下任一项生成了 Visual Studio 项目：
  
  - 您的项目 -- 如果您使用的是源引擎，则必须为您的项目生成一个 Visual Studio 项目。有关帮助，请参阅 [创建 Visual Studio 项目](/docs/welcome-guide/create/creating-projects-using-cli/creating-windows#create-a-visual-studio-project)。

  - 您的引擎 -- 如果您使用的是预构建的 SDK 引擎，则必须为您的引擎生成一个 Visual Studio 项目。如需帮助，请参阅 [构建 INSTALL 目标](/docs/user-guide/build/distributable-engine/#build-the-install-target)中 **引擎和项目分发** 主题下的第一步.

- 将项目注册到引擎。


## 教程细节

本教程在示例中使用以下目录。

- 项目名称和目录: `C:\MyProject`
- 第三方软件包目录: `C:\o3de-packages`
- 源码版引擎: `C:\o3de`
- 预构建的 SDK 引擎: `C:\o3de-install`


## 设置起始关卡
{{< note >}}
如果您在项目的代码库中设置了起始关卡，则此步骤是可选的。开发中更常见的做法是通过关卡管理器或类似系统在项目代码库中设置起始关卡。
{{< /note >}}

**先决条件**：您必须在项目中至少创建一个关卡。请查看 [创建关卡](/docs/learning-guide/tutorials/environments/create-a-level/) 教程以开始使用。

在创建项目游戏发布布局之前，您的项目必须指定起始关卡，以通知 Game Launcher 应首先加载哪个关卡。您可以在项目的设置注册表(`.setreg`)中或通过关卡管理器或团队开发的类似系统在项目的代码库中指定起始关卡。

在此示例中，通过在`C:\MyProject\Registry`目录中创建一个 `autoexec.game.setreg` 文件，在设置注册表中设置项目的起始关卡。然后，添加以下行。

```JSON
{
    "O3DE": {
        "Autoexec": {
            "ConsoleCommands": {
                "LoadLevel": "mainmenu"
            }
        }
    }
}
```

此配置通过将`Loadlevel`设置为关卡名称来指定起始关卡。在此示例中，关卡资源名为`mainmenu`。您无需指定关卡资源的文件扩展名，例如 `main.prefab` 或 `main.spawnable`。您可以在 `C:\MyProject\Levels` 目录中找到项目的关卡。


## 流程资产

使用 **Asset Processor** 或 **Asset Processor Batch** 程序将源资源处理为产品资源，Game Launcher 在运行时使用该资源。Asset Processor 提供图形用户界面 （GUI），而 Asset Processor Batch 提供命令行界面 （CLI）。

### 处理资产的方法

要处理项目的资源，请执行以下操作之一：

- **运行 Asset Processor.** 如果您已经构建了 Asset Processor 并且更喜欢使用 GUI，请执行此操作。

    1. 根据您的引擎类型从 build 目录运行 `AssetProcessor.exe` 。
        - `<project>/build/windows/bin/profile` -- 对于源代码版引擎。
        - `<engine>/build/windows/bin/profile` -- 对于预构建码版引擎。

        {{< image-width "/images/user-guide/packaging/windows-release-build/asset-processor.png" "700" "An image of O3DE Asset Processor." >}}

    1. 等待所有作业完成。您可以在 **Asset Status** 窗格中查看每个资产的状态，并在 **Event Log Details** 窗格中查看每个资产的详细信息。

- **运行 Asset Processor Batch.** 如果您已经构建了 Asset Processor 并且更喜欢使用 CLI，请执行此操作。

    1. 根据您的引擎类型从 build 目录运行 `AssetProcessorBatch.exe` 。
        - `<project>/build/windows/bin/profile` -- 对于源代码版引擎。
        - `<engine>/build/windows/bin/profile` -- 对于预构建码版引擎。

        {{< image-width "/images/user-guide/packaging/windows-release-build/asset-processor-batch.png" "700" "An image of O3DE Asset Processor Batch." >}}

    1. 等待所有作业完成。 您可以在 `<project>\user\log` 目录的`AP_Batch.log`文件中查看输出。

- **运行你的项目的 Asset Processor 工具。** 如果您尚未构建 Asset Processor 和 Asset Processor Batch，请执行此操作。

    1. 使用 **CMake** 调用 Visual Studio 编译 Asset Processor 工具及其依赖。指定 `MyProject.Assets` 目标。

        ```cmd
        cmake --build build\windows --target MyProject.Assets --config profile -- -m
        ```

        首先，此命令构建 Asset Processor 工具。然后，它运行 Asset Processor Batch 并处理资产。除非您在项目中进行更改，否则您不需要重新运行 Asset Processor 或 Asset Processor Batch。

    1. 等待所有作业完成。 您可以在 `<project>\user\log` 目录的`AP_Batch.log`文件中查看输出。

### 验证资产是否已成功处理

通过检查资产的状态来验证 Asset Processor 是否已成功处理您的资产。Asset Processor 可以在有或没有警告或错误的情况下处理您的资产，或者它们可能无法处理。警告和错误表明您的资产可能需要您注意，但它们不会妨碍创建项目游戏发布布局的能力。另一方面，解决因故障而未处理的任何资产至关重要。

有关故障故障排除的帮助，请参阅 [O3DE 中的打包故障排除](/docs/user-guide/packaging/troubleshooting/)。


## 创建项目游戏发布布局

现在您已经准备好了项目，您可以创建项目游戏发布布局。您可以通过不同的方式执行此操作，具体取决于您设置引擎的方式以及是要创建非整体式还是整体式发布版本。

在设置 O3DE 时，您将引擎构建为 *源引擎* 或 *预构建的 SDK 引擎*。您可以使用任一 build 类型来创建项目游戏发布布局。有关构建类型的更多信息，请参阅 [构建引擎](/docs/welcome-guide/setup/setup-from-github/building-windows/#build-the-engine)。

此外，您可以创建*非整体*或*整体*的项目游戏发布布局。非整体式构建使用 Gem 作为动态链接的库 （`.dll`），而整体式构建使用 Gem 作为静态链接的库 （`.lib`）。在整体构建中将 Gem 用作`.lib` 文件可以去除未使用的代码段并删除其中的冗余符号。因此，整体式构建的磁盘总空间小于非整体式构建的磁盘总空间。

{{< note >}}
整体式构建的最终输出可能包含一些`.dll`文件。当第三方库仅提供`.dll`文件时，可能会发生这种情况。
{{< /note >}}

在此步骤中，您将使用 CMake 构建非整体式或整体式项目以进行发布。这将处理多项任务：

1. 创建一个包含项目游戏发布布局的 *安装目录*。默认情况下，安装目录位于项目的目录中（例如，`C:\MyProject\install`）。或者，您可以通过在 `<project>\build\<build>`目录的`CMakeCache.txt`文件中设置`CMAKE_INSTALL_PREFIX`来将安装目录设置为其他位置（例如，`C:\MyProject\build\windows\CMakeCache.txt`）。

1. 通过将`<project>\Cache\pc`目录中的所有产品资源打包到`engine.pak`文件中来捆绑你的游戏内容。

1. 在项目游戏发布布局中构建 Game Launcher 和其他支持文件。

{{< tabs name="release-build-instructions" >}}
{{% tab name="Source engine" %}}

### 源代码版引擎

使用 源代码版引擎，您必须使用以项目为中心的配置来创建项目游戏发布布局。在以项目为中心的配置中，CMake 源目录位于项目构建目录中。因此，O3DE 可执行文件（例如，**O3DE Editor**、Asset Processor 和 Game Launcher）位于您的`<project>\build`目录中。

现在，您将为项目游戏发布布局选择生成类型。

#### 选项 1：整体式（仅限游戏）

最常见的工作流程是仅发布您的游戏，在这种情况下，您应该选择 **monolithic** 版本。这将仅构建您的游戏/服务器启动器和任何必要的依赖项。

1. 使用 CMake 为整体式项目创建 Visual Studio 项目。在项目目录中指定一个新的 CMake 构建目录 （`build\windows_mono`），以将其与非整体式构建目录分开。通过启用`-D`选项来指定整体构建 `LY_MONOLITHIC_GAME`。

    ```cmd
    cd C:\MyProject
    cmake -B build\windows_mono -S . -G "Visual Studio 16" -DLY_3RDPARTY_PATH=C:\o3de-packages -DLY_MONOLITHIC_GAME=1
    ```

    **注意:** Visual Studio 2019 使用 `Visual Studio 16`作为生成器，Visual Studio 2022 使用 `Visual Studio 17` 作为生成器。有关各支持平台的常用生成器的完整列表，请参阅[配置项目](./configure-and-build/#configuring-projects)。

1. 要构建要发布的整体式项目，请使用 CMake 调用 Visual Studio 构建器。指定 `install` 目标和 `release` 配置。

    ```cmd
    cd C:\MyProject
    cmake --build build\windows_mono --target install --config release
    ```

    此命令生成 Game Launcher 和依赖的 `.lib` 文件。它还将位于`<project>\Cache\pc`中的项目产品资产捆绑到`engine.pak`文件中。

结果是位于`<install>\bin\Windows\release\Monolithic`的安装目录中的项目游戏发布布局。在项目游戏发布布局中， `engine.pak` 文件位于`Cache\pc`中。

您的发布版本现已完成！接下来，您需要 [捆绑您的内容](#bundle-content).

#### 选项 2：非整体式（游戏 + 编辑器/工具）

另一种选择是发布您的游戏以及 **O3DE 编辑器** 及其所有工具，这是一个 **非整体** 版本。不建议这样做，因为所有这些工具的大小加起来会比 **整体** 构建大得多。

默认情况下，从源引擎构建支持非整体式项目。如 [先决条件](#prerequisites) 中所述，您应该已经为您的项目创建了一个 Visual Studio 项目，并将您的项目注册到您的引擎。

要构建非整体式项目以供发布，请使用 CMake 调用 Visual Studio MSBuild。指定`install`目标和`release`配置。

```cmd
cd C:\MyProject
cmake --build build\windows --target install --config release
```

您可以通过附加选项 `-DLY_MONOLITHIC_GAME=0` 来指定特定的非整体构建。此命令生成 O3DE 工具（如 Editor、Asset Processor、Game Launcher 和 Server Launcher）和依赖的`.dll`文件。它还将位于 `<project>\Cache\pc` 中的项目产品资产捆绑到 `engine.pak` 文件中。

结果是位于`<install>\bin\Windows\release\Default`的安装目录中的项目游戏发布布局。在项目游戏发布布局中，`engine.pak`文件位于`Cache\pc`中。

您的发布版本现已完成！接下来，您需要 [捆绑您的内容](#bundle-content).

{{% /tab %}}
{{% tab name="Pre-built SDK engine" %}}

### 预构建的 SDK 引擎

使用预构建的 SDK 引擎，要创建项目游戏发布布局，您必须附加非整体式或整体式构件。

通过检查项目的`project.json`文件，验证您的项目是否已注册到预构建的 SDK 引擎。要注册您的项目，请使用预构建的 SDK 引擎中的 O3DE CLI。

```cmd
cd C:\o3de-install
scripts\o3de.bat register --project-path C:\o3de-projects\MyProject
```

现在，您将为项目游戏发布布局选择生成类型。

#### 选项 1：整体式（仅限游戏）

最常见的工作流程是仅发布您的游戏，在这种情况下，您应该选择 **monolithic** 版本。这将仅构建您的游戏/服务器启动器和任何必要的依赖项。

1. 首先，我们需要在 SDK 引擎布局中构建整体式构件。在预构建的 SDK 引擎目录中指定一个新的 CMake 构建目录 （`build\windows_mono`），与非整体式构建目录分开。通过启用`-D`选项来指定整体构建， `LY_MONOLITHIC_GAME`。

    ```cmd
    cd C:\o3de
    cmake --preset windows-mono-default -DO3DE_INSTALL_ENGINE_NAME=o3de-install
    ```

    {{< note >}}
   Visual Studio 2019 使用 `Visual Studio 16`作为生成器，Visual Studio 2022 使用 `Visual Studio 17` 作为生成器。有关各支持平台的常用生成器的完整列表，请参阅[配置项目](./configure-and-build/#configuring-projects)。
    {{< /note >}}

1. 在您的源引擎中，使用 CMake 调用 Visual Studio，以将非整体发布工件附加到预构建的 SDK 布局中。

    ```cmd
    cd C:\o3de
    cmake --build --preset windows-mono-install --config release
    ```

1. 现在，整体式构建工件已添加到 SDK 引擎布局中，您的项目可以针对该布局进行配置以运行整体式构建。切换到项目的源目录，并使用 CMake 为您的项目创建一个针对整体引擎构建的 Visual Studio 项目。

    ```cmd
    cd C:\MyProject
    cmake --B build\windows_mono -S . -DCMAKE_INSTALL_PREFIX=C:\MyProject\MyProjectGameLayout -DLY_MONOLITHIC_GAME=1
    ```

1. 最后，调用 CMake 构建包装器，以使用 `install` 目标构建项目游戏发布布局。

    此命令生成 Game Launcher 和依赖的`.lib`文件。它还将位于`<project>\Cache\pc`中的项目产品资产捆绑到`engine.pak`文件中。

结果是位于`<install>\bin\Windows\release\Monolithic`的安装目录中的项目游戏发布布局。在项目游戏发布布局中，`engine.pak`文件位于`Cache\pc`中。

您的发布版本现已完成！接下来，您需要 [捆绑您的内容](#bundle-content).

#### 选项 2：非整体式（游戏 + 编辑器/工具）

另一种选择是发布您的游戏以及 **O3DE 编辑器** 及其所有工具，这是一个 **非整体** 版本。不建议这样做，因为所有这些工具的大小加起来会比 **整体** 构建大得多。

默认情况下，预构建的 SDK 引擎支持非整体式项目。如 [先决条件](#prerequisites)中所述，您应该已经为该引擎创建了一个 Visual Studio 项目并注册了该引擎。

1. 使用 CMake 为您的 SDK 引擎创建 Visual Studio 项目，以支持非整体式项目。

    ```cmd
    cd C:\o3de
    cmake --preset windows-default -DO3DE_INSTALL_ENGINE_NAME=o3de-install
    ```

    **注意:** Visual Studio 2019 使用 `Visual Studio 16`作为生成器，Visual Studio 2022 使用 `Visual Studio 17` 作为生成器。有关各支持平台的常用生成器的完整列表，请参阅[配置项目](./configure-and-build/#configuring-projects)。

1. 在您的源引擎中，使用 CMake 调用 Visual Studio，以将非整体发布工件附加到预构建的 SDK 布局中。

    ```cmd
    cd C:\o3de
    cmake --build --preset windows-install --config release
    ```

    您可以通过附加选项`-DLY_MONOLITHIC_GAME=0`来指定特定的非整体构建。此命令生成 O3DE 工具（如 Editor、Asset Processor 和 Game Launcher）和依赖的`.dll`文件。它还将位于`<project>\Cache\pc`中的项目产品资产捆绑到`engine.pak`文件中。

结果是位于`<install>\bin\Windows\release\Default`的安装目录中的项目游戏发布布局。在项目游戏发布布局中，`engine.pak`文件位于`Cache\pc`中。

您的发布版本现已完成！接下来，您需要 [捆绑您的内容](#bundle-content).

{{% /tab %}}
{{< /tabs >}}

## 捆绑内容

**Asset Bundler** 和 **Asset Bundler Batch** 程序是允许您创建和优化捆绑内容的工具。在之前创建项目游戏发布布局时，将所有资产捆绑到一个`engine.pak`文件中，该文件包含所有项目和引擎的产品资产。引擎中可能有许多未使用的资产，并且你的项目也可能包含未使用的资产，因此`engine.pak`文件可能比运行游戏所需的要大得多。要优化捆绑内容，请使用 Asset Bundler 并配置所需的资源捆绑方式。

按照 [捆绑项目游戏发布布局的资源](asset-bundler/bundle-assets-for-release) 中的说明进行操作。创建两个捆绑包。强烈建议您在捆绑包文件名中添加初始版本号，例如，`game_v1.pak`用于游戏资产，`engine_v1.pak`用于引擎资产。如果您添加或更新项目的资源，或者升级引擎，将来可能必须创建 [内容补丁包](./content-patch-package)。向 Asset Bundle 添加版本号可简化过程。然后，将捆绑包添加到您的项目游戏版本布局中。

你应该注意到，你的两个捆绑资源加起来都小于你之前创建项目游戏发布布局时自动创建的默认`engine.pak`。

## 许可证文件生成
可以在项目开发过程中生成许可证归属文件（通常称为 NOTICES 文件），以正确归属导入的任何代码或可下载包。要扫描项目和包目录以查找许可证，您可以运行位于引擎的 `scripts\license_scanner`子目录中的脚本。此脚本将查找`PackageInfo.json` 文件，以便创建带有 SPDX 标签的所有软件包许可证的摘要文件，以便于参考。

有关详细信息，请参阅 [引擎和项目分发](/docs/user-guide/build/distributable-engine#license-file-generation) 中的说明。通常，您会将生成的文件复制到游戏发布布局的根目录，以正确归因您的依赖项。

## 运行 Game Launcher

现在，您可以运行项目的 Game Launcher 了。

从项目游戏版本布局中运行`GameLauncher.exe`，该布局位于`<install>\bin\Windows\release\<build>`目录中。例如，路径可以是：

- `C:\MyProject\install\bin\Windows\release\Monolithic` -- 适用于整体式项目。

- `C:\MyProject\install\bin\Windows\release\Default` -- 对于非整体式项目。

## 分发发布版本

要将构建版本分发到其他 Windows 计算机，请执行以下操作：

1. 创建`<install>\bin\Windows\release\<build>`文件夹的 `.zip` 文件，内容应如下所示：

        Monolithic
        |   DevTestProject.GameLauncher.exe
        |   PhysXDevice64.dll
        |   PhysXGpu_64.dll
        |   
        \---Cache
            \---pc
            engine_v1.pak

1. 使用您喜欢的方法（例如文件存储服务）分发`.zip`文件。

现在，其他人可以下载并解压缩`.zip`文件，并在他们的 Windows 计算机上运行 Game Launcher。

{{< note >}}
要在其他 Windows 计算机上运行游戏启动器，它们必须安装 [Microsoft Visual C++ （MSVC） Redistributable](https://docs.microsoft.com/en-us/cpp/windows/latest-supported-vc-redist?view=msvc-160) 。
{{< /note >}}
