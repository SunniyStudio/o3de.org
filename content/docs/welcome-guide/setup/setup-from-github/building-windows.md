---
title: 为 Windows 构建
description: 了解如何从 Windows 的 GitHub 源构建 Open 3D Engine （O3DE）。
weight: 100
toc: true
---

拥有 O3DE 源的本地副本后，您可以构建引擎，包括关键工具，例如 **Asset Processor**、**O3DE Editor** 和 **Project Manager**。如果您尚未从 GitHub 克隆源代码，请参阅 [从 GitHub 设置 O3DE](../setup-from-github)。

## 先决条件

以下说明假定您已：

* 满足 [O3DE 系统要求](/docs/welcome-guide/requirements) 中列出的所有硬件和软件要求。
* 按照系统要求主题的 [Windows 软件配置](/docs/welcome-guide/requirements/#microsoft-windows) 部分所述配置所需的 Windows 软件。

## 构建引擎

从源代码构建引擎时，根据开发工作的重点和需求，您有几个配置选项：

* 构建类型
* 构建配置

### 构建类型

1. **源代码引擎** - 如果您计划频繁更改引擎源代码，请选择此构建类型。如果您的主要兴趣是引擎开发，则可以自行构建引擎，也可以使用使用您的自定义构建的项目来构建引擎。

1. **预构建的 SDK 引擎** – 如果您主要对为项目开发创建可分发引擎感兴趣，并且您计划仅对引擎源进行不频繁的更改，请选择此构建类型。

{{< tip >}}
如果您不打算对引擎源进行任何更改，请考虑使用 [Windows 安装程序](/docs/welcome-guide/setup/installing-windows/) 下载并安装 O3DE。
{{< /tip >}}

### 构建配置

1. **Debug** - 提供最高级别的调试支持。将避免包括函数内联在内的优化，从而更容易跟踪代码。某些编译器可能会禁用堆栈溢出或其他内存保护运行时检查，使其成为检查影响堆栈的某些类型错误的最佳构建配置。在支持的 IDE 中，这还会启用 “Edit and continue” 支持。

1. **Profile** - 提供调试符号支持，但启用优化（相当于 clang `-O2`，非主动优化）。这是日常工作流程的推荐配置文件，因为它是最具代表性的发布版本，但启用了符号。

1. **Release** - 将此函数用于最终发布版本。包括非主动优化，无调试信息。

### 构建说明

以下说明展示了如何在 profile* 配置中构建 *source engine*。构建预构建的 SDK 引擎稍微复杂一些。有关详细信息，请参阅用户指南中的 [创建可分发引擎版本](/docs/user-guide/build/distributable-engine) 主题。

1. （可选）如果您还没有包目录，请在可写位置创建一个。以下示例使用目录 `C:\o3de-packages`。或者，您可以让构建过程使用 O3DE 安装程序也使用的默认包目录，该目录位于`<user>\.o3de\3rdParty`中。如果您有多个版本的 O3DE，例如从源构建的引擎和一个或多个已安装的引擎版本，这可以节省磁盘空间。

    ```cmd
    mkdir C:\o3de-packages
    ```

    O3DE 包下载器使用此目录来检索引擎所需的外部库。

1. 获取 Python 运行时，它未包含在 GitHub 存储库中。`o3de`脚本（O3DE CLI 的一部分）需要此运行时。您将使用此脚本运行常见的命令行函数。此脚本还要求在设备的路径上安装 **CMake** 并使其易于访问。如果您尚未安装 CMake，或者收到运行脚本时找不到 CMake 的错误，请参阅 [O3DE 系统要求](/docs/welcome-guide/requirements)页面以获取安装说明。

    打开命令提示符并切换到设置 O3DE 的目录，然后运行`get_python` 脚本。

    ```cmd
    # Run from the o3de engine root.
    python\get_python.bat
    ```

1. 使用 CMake 为引擎创建 Visual Studio 项目。提供 build 目录、Visual Studio 生成器、您创建的 packages 目录的路径以及任何其他项目选项。路径可以是绝对路径，也可以是相对路径。或者，您可以使用 CMake GUI 完成此步骤。

    ```cmd
    cmake -B build/windows -S . -G "Visual Studio 16" -DLY_3RDPARTY_PATH=C:\o3de-packages
    ```

    * 使用`Visual Studio 16`作为 Visual Studio 2019 的生成器，使用`Visual Studio 17`作为 Visual Studio 2022 的生成器。有关每个受支持平台的常用生成器的完整列表，请参阅用户指南中的 [配置项目](/docs/user-guide/build/configure-and-build/#configuring-projects)。

    * `LY_3RDPARTY_PATH` 是可选的自定义定义。（自定义定义以`-D`为前缀。使用它来指定可下载包目录的路径，也称为 “third-party path”。指定 packages 目录的路径时，不要使用尾部斜杠。您也可以关闭此选项以使用默认包目录。

1. （可选）使用 CMake 构建源引擎。此步骤是可选的，因为在 “source engine” 构建模型中，引擎是在每个项目内部构建的。如果您计划使用项目，为避免重复构建引擎，请考虑等到您学习如何创建和构建项目，这在 [项目创建](/docs/welcome-guide/create/) 部分进行了介绍。

    以下命令使用 `profile` 构建配置构建引擎，无需项目。

    ```cmd
    cmake --build build/windows --target Editor --config profile -- -m
    ```

   `-m`是推荐的构建工具优化。它告诉 Microsoft 编译器 （MSVC） 在编译期间使用多个线程以加快构建时间。

    `--config` 设置 [构建配置类型](/docs/user-guide/build/configure-and-build.md#generated-build-configurations): `debug`, `profile`, 或 `release`。

    引擎需要一段时间来构建。如果你已经使用了这些步骤中的所有示例命令，那么当构建完成后，你可以在`<engine>\build\windows\bin\profile`中找到引擎工具和其他二进制文件。

    {{< note >}}
CMake [unity builds](https://cmake.org/cmake/help/latest/prop_tgt/UNITY_BUILD.html)默认处于开启状态。这是一项 CMake 功能，可以通过将源文件合并为单个编译单元来大大缩短构建时间。如果遇到构建错误，禁用 Unity 构建可能有助于调试问题。要禁用 Unity 构建，请运行带有`-DLY_UNITY_BUILD=OFF`参数的 `cmake` 项目生成命令以重新生成项目文件。
    {{< /note >}}

1. 首次构建引擎后，请务必使用下一节中的步骤在 O3DE 清单中注册它。

## 注册引擎

注册 O3DE 引擎使 O3DE 项目能够找到该引擎，即使它们位于计算机上的不同位置。注册过程会在您的用户目录中创建（或更新）**O3DE 清单**。

1. 在命令窗口中，使用“o3de”脚本注册引擎。

    ```cmd
    # Run from the o3de engine root.
    scripts\o3de.bat register --this-engine
    ```

    O3DE 清单文件为`<user>\.o3de\o3de_manifest.json`。所有已注册的引擎、项目等的路径都记录在此文件中。

现在，您可以创建工程了。有关项目配置的介绍，请参阅 [使用 Open 3D Engine 创建项目](/docs/welcome-guide/create).
