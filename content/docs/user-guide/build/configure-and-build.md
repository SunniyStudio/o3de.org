---
title: 配置和构建
description: 了解如何将 CMake 构建工具与 Open 3D Engine (O3DE) 结合使用。
weight: 100
---

使用 CMake 构建**Open 3D Engine (O3DE)**或其任何项目分为两个步骤： 为构建工具链创建**本地项目**，然后调用该工具链构建引擎或项目。

为了保持快速编译，CMake 会对用于生成项目文件的内部值进行缓存，并利用本地编译工具支持的增量编译。首次配置后，除非更改重新创建本地构建项目所需的值，否则无需对 CMake 缓存进行任何更改。在大多数工作流程中，只有在添加源代码时才需要重新生成本地构建项目。

O3DE 中 CMake 的一个重要元素是，相同的命令可用于配置和构建**引擎本身和任何 O3DE 项目**。

## 前提条件

要在 O3DE 中创建和构建项目，您必须按照系统需求主题中[软件先决条件和配置](/docs/welcome-guide/requirements/#software-prerequisites) 部分的描述，配置平台所需的软件。

您需要确定以下内容：

* 主机平台的工具链，用于构建编辑器和工具。
* 目标平台的工具链，用于构建项目。在很多情况下，这将与您的主机平台相同。

### 支持的编译器工具链

O3DE 支持以下平台和工具链的构建：

| 平台 | 支持的工具链 |
| --- | --- |
| Windows 64-位 | Visual Studio ([支持的版本](/docs/welcome-guide/requirements/#microsoft-visual-studio)) |
| Linux | Clang/LLVM |
| macOS, iOS | XCode {{< versions/xcode >}} 或更高版本 |
| Android | Android Clang/LLVM |

{{< note >}}
有关特定平台的其他要求和信息，请参阅 [O3DE 系统要求](/docs/welcome-guide/requirements)、[CMake 设置参考](/docs/user-guide/build/reference)和相关 [平台概述](/docs/user-guide/platforms/)。
{{< /note >}}

## 第一个项目

为了让您开始第一个开箱即用的 O3DE 构建，我们建议您学习入门指南中的 [使用命令行界面创建项目](/docs/welcome-guide/create/creating-projects-using-cli/) 教程。

在这里，您将学习到基础知识，包括如何

* 使用 `o3de create-project` 启动一个新项目。
* 使用 `cmake -G` 生成项目文件。
* 使用 `cmake --build` 构建项目。

本主题的其余章节将更详细地介绍 CMake 的配置和构建，包括在后续构建中需要更改的值。

## 配置项目

配置项目包括为 CMake 缓存定义变量，以及生成构建项目所需的项目文件。您可以使用 CMake CLI 或 CMake GUI 在命令行或终端窗口进行配置。

要使用 CMake 配置 O3DE 项目，您需要以下信息：

* 构建源目录和输出目录的位置。
* 用于创建本地项目文件的生成器。

    | 平台 / 构建系统 | 生成器 |
    | --- | --- |
    | Windows / Visual Studio 2019 | `Visual Studio 16` |
    | Windows / Visual Studio 2022 | `Visual Studio 17` |
    | Linux / Ninja | `Ninja Multi-Config` |

* 可下载软件包（也称为第三方库）的位置。Windows 下的默认位置是 `<user>/.o3de/3rdParty`，Linux 下的默认位置是 `$HOME/.o3de/3rdParty`。

### 使用 CMake CLI 配置

用于配置项目的典型 CMake 命令如下：

{{< tabs name="CMake配置示例" >}}
{{% tab name="Windows" %}}

```cmd
cmake -B build/windows -S . -G "Visual Studio 16" -DLY_3RDPARTY_PATH=<downloadable-packages-directory>
```

{{% /tab %}}
{{% tab name="Linux" %}}

{{< important >}}
在使用 O3DE 预编译 **Snap** SDK 构建时，首先导出 `O3DE_SNAP` 环境变量，这样 CMake 就不会尝试安装 Python pip 要求而失败。要导出 `O3DE_SNAP` 环境变量，请在运行下面的 CMake 命令之前，在命令行中运行 `export O3DE_SNAP` 命令。
{{< /important >}}

```shell
cmake -B build/linux -S . -G "Ninja Multi-Config" -DLY_3RDPARTY_PATH=<downloadable-packages-directory>
```

{{% /tab %}}
{{< /tabs >}}

* `-B` : 构建目录的位置，用于放置生成的文件。
* `-S` : 源代码目录，即 CMakeLists.txt 根目录。从源代码目录运行 `cmake` 时为可选项。
* `-G` : 用于创建本地项目文件的生成器。

另一个参数是 O3DE 使用的构建脚本自定义定义 (`-D`)：

* `LY_3RDPARTY_PATH` : [O3DE 软件包](./packages)的**绝对路径**。如果在配置过程中缺少软件包，它们将被下载到此位置。

{{< note >}}
CMake [unity builds](https://cmake.org/cmake/help/latest/prop_tgt/UNITY_BUILD.html) 默认开启。这是 CMake 的一项功能，通过将源文件合并为单一编译单元，可以大大缩短编译时间。如果遇到编译错误，禁用 unity 编译可能有助于调试问题。要禁用 unity 联编，请使用 `-DLY_UNITY_BUILD=OFF` 参数运行之前的 `cmake` 命令来重新生成项目文件。
{{< /note >}}

### 使用 CMake GUI 进行配置

CMake 还提供了一个基于图形用户界面的直观工具，你可以用它来代替命令行。

1. 使用以下命令启动CMake GUI：

    ```shell
    cd <source-directory>
    cmake-gui .
    ```

1. 启动后在图形用户界面中设置以下值：

    * 设置**Where is the source code:** 文本字段至您的 O3DE 项目目录。
    * 设置**Where to build the binaries:**文本字段到 O3DE 项目的一个子目录，您希望在该目录下生成编译文件。对于 Windows 平台，典型值为 `<project-dir>/build/windows`；对于 Linux 平台，典型值为 `<project-dir>/build/linux`。

1. (可选) 选择**Add Entry**按钮，并为`LY_3RDPARTY_PATH`可下载软件包目录添加缓存条目。此条目使用以下值：

    * **Name:** LY_3RDPARTY_PATH
    * **Type:** STRING
    * **Value:** `<directory where you want CMake to download the packaged libraries>`

    如果不提供此条目，库将下载到默认目录：Windows 为 `<user>/.o3de/3rdParty`，Linux 为 `$HOME/.o3de/3rdParty`。

1. 选择**Configure**来配置您的项目。这也会将任何需要的软件包下载到 `LY_3RDPARTY_PATH` 所设置的路径。

1. 指定项目的生成器。

    * **对于 Windows:** Visual Studio 16 _或_ Visual Studio 17
    * **对于 Linux:** Ninja Multi-Config

1. 检查读入的变量，更新不正确的变量。

    {{< note >}}
   CMake [unity builds](https://cmake.org/cmake/help/latest/prop_tgt/UNITY_BUILD.html) 默认开启。这是 CMake 的一项功能，通过将源文件合并为单一编译单元，可以大大缩短编译时间。如果遇到编译错误，禁用 unity 编译可能有助于调试问题。要禁用 unity 联编，请使用 `-DLY_UNITY_BUILD=OFF` 参数运行之前的 `cmake` 命令来重新生成项目文件。
    {{< /note >}}

1. 选择 **Generate**，在构建目录中生成项目文件。

## 使用 CMake 构建 O3DE 目标

CMake 使您能够通过 `--build` 选项从其生成的项目中调用本地工具链。CMake 构建是为了方便--生成本地项目后，您可以完全在集成开发环境中工作。

### 生成的构建配置

O3DE 支持大量的构建配置，以支持调试、剖析和生成项目版本的开发工作流。每种配置都有一组属性，使其适合执行某些任务，并影响调试符号表、优化级别以及哪些 O3DE 开发工具可用于检查和发送资产到运行中的项目等。

有关编译器为每种构建配置使用的全部标志集，请参阅源代码中的以下 CMake 文件：

* `cmake/Configurations.cmake`
* `cmake/Common/Configurations_common.cmake`
* `cmake/Common/<compiler>/Configurations_<compiler>.cmake`
* `cmake/Platform/<platform ID>/Configurations_<platform ID>.cmake`

下表概括介绍了每种构建配置的功能。

| 配置 | 效果 |
| --- | --- |
| **Debug** | 最大程度的调试支持。将避免包括函数内联在内的优化，从而更容易跟踪代码。某些编译器可能会禁用堆栈溢出或其他内存保护运行时检查，因此这是检查影响堆栈的某些类型错误的最佳构建配置。在支持的集成开发环境中，这还能启用 “编辑并继续 ”支持。 |
| **Profile** | 支持调试符号，但启用了优化（相当于 clang `-O2`，非激进优化）。这是日常工作流程的推荐配置文件，因为它最能代表发布版本的构建，但启用了符号。|
| **Release** | 用于最终发布版本。非激进优化，无调试信息。 |

### 创建 O3DE 编辑器、引擎和工具

使用以下命令仅构建编辑器及其工具依赖项：

{{< tabs name="CMake引擎构建示例" >}}
{{% tab name="Windows" %}}

```cmd
cmake --build build/windows --target Editor --config profile -- -m
```

* `--build` : 编译目录的位置，即编译输出的位置。
* `--target` : 构建目标。可以指定多个，中间用空格隔开。
* `--config` : 构建配置。请参阅上一节 [生成的构建配置](#generated-build-configurations)。
* `-m` : 一种推荐的构建工具优化。它告诉微软编译器（MSVC）在编译过程中使用多个线程，以加快编译时间。

在此示例中，构建产品放在`build\windows\bin\profile`目录下。

{{% /tab %}}
{{% tab name="Linux" %}}

```shell
cmake --build build/linux --target Editor --config profile -j <number of parallel build tasks>
```

* `--build` : 编译目录的位置，即编译输出的位置。
* `--target` : 构建目标。可以指定多个，中间用空格隔开。
* `--config` : 构建配置。请参阅上一节 [生成的构建配置](#generated-build-configurations)。
* `-j` : 建议对构建工具进行优化。它告诉 Ninja 编译工具将同时执行的并行编译任务的数量。'number of parallel build tasks'建议与 Linux 主机上可用的内核数量相匹配。

在此示例中，构建产品被放置在`build/linux/bin/profile`目录中。

{{% /tab %}}
{{< /tabs >}}

### 构建项目

使用以下命令构建项目、编辑器及其工具依赖项：

{{< tabs name="CMake项目构建示例" >}}
{{% tab name="Windows" %}}

```cmd
cmake --build build/windows --target <ProjectName>.GameLauncher Editor --config profile -- -m
```

有关每个参数的解释，请参阅上一节。

在此示例中，构建产品放在`build\windows\bin\profile`目录下。

{{% /tab %}}
{{% tab name="Linux" %}}

```shell
cmake --build build/linux --target <ProjectName>.GameLauncher Editor --config profile -j <number of parallel build tasks>
```

有关各参数的解释，请参阅上一节。

在本例中，编译产品被放置在 “build/linux/bin/profile ”目录下。

{{% /tab %}}
{{< /tabs >}}

## 模拟自动审查运行

要测试您分支中的构建，您可以使用 O3DE 附带的 `ci_build.py` Python 脚本来模拟 Jenkins 构建节点上执行的自动审查 (AR) 运行。在 GitHub 拉取请求合并之前，您对 O3DE 代码库的任何贡献都需要通过该测试运行。

使用以下说明运行 `ci_build.py` 来模拟 AR 运行。该构建脚本位于 `<O3DE_engine>/scripts/build/` 目录中。该脚本要求将环境变量 `LY_3RDPARTY_PATH` 设为可下载软件包（“第三方”）文件夹。

{{< tabs name="模拟 AR 运行示例" >}}
{{% tab name="Windows" %}}

1. 设置 `LY_3RDPARTY_PATH` 环境变量（如果尚未设置）。

    ```cmd
    set LY_3RDPARTY_PATH=<path to your downloadable packages folder>
    ```

   例如，要使用默认的可下载软件包文件夹，请设置以下路径：

    ```cmd
    set LY_3RDPARTY_PATH=%USERPROFILE%\.o3de\3rdParty
    ```

1. 从引擎根文件夹运行以下命令。

    ```cmd
    python\python.cmd -u scripts\build\ci_build.py --platform Windows --type <test_type>
    ```

    对于 `<test_type>`，请使用文件 `scripts\build\Platform\Windows\build_config.json` 中定义的任何测试类型。例如，要运行配置文件配置的测试套件，可以使用 `profile_vs2019`。

    ```cmd
    python\python.cmd -u scripts\build\ci_build.py --platform Windows --type profile_vs2019
    ```

{{% /tab %}}
{{% tab name="Linux" %}}

1. 设置 `LY_3RDPARTY_PATH` 环境变量（如果尚未设置）。

    ```shell
    export LY_3RDPARTY_PATH=<path to your downloadable packages folder>
    ```

   例如，要使用默认的可下载软件包文件夹，请设置以下路径：

    ```shell
    export LY_3RDPARTY_PATH=~/.o3de/3rdParty
    ```

1. 从引擎根文件夹运行以下命令。

    ```shell
    python/python.sh -u scripts/build/ci_build.py --platform Linux --type <test_type>
    ```

   对于 `<test_type>`，请使用文件 `scripts/build/Platform/Linux/build_config.json` 中定义的任何测试类型。例如，要运行配置文件配置的测试套件，可使用 `test_profile`。

    ```shell
    python/python.sh -u scripts/build/ci_build.py --platform Linux --type test_profile
    ```

{{% /tab %}}
{{< /tabs >}}
