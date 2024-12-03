---
linktitle: 系统要求
title: O3DE 系统要求
description: 查看使用 Open 3D Engine （O3DE） 进行开发的系统要求。
weight: 300
toc: true
---

**Open 3D Engine (O3DE)** 具有一组最低的开发和软件开发要求，如以下部分所述。在 [软件先决条件和配置](#software-prerequisites) 部分中，列出了每个先决条件以及所需的任何特定配置步骤。

## 硬件要求

以下部分指定了安装或构建 O3DE 编辑器和相关工具以及开发 O3DE 项目所需的最低和推荐系统硬件。 RAM 和可用磁盘空间要求取决于您在 O3DE 中配置项目时选择的选项。 性能将根据项目关卡的复杂程度而有所不同。 

{{< note >}}
除了系统的运行要求外，从源构建引擎还需要 2GB _/线程_。 但是，可以限制分配给生成进程的线程数。  

与从源构建相比，使用 [预构建的安装程序](https://o3debinaries.org/download/windows.html) 安装引擎需要更少的磁盘空间和 RAM。
{{< /note >}}

### 最低硬件规格

| 硬件组件 |最低要求规格 |
|-----| - |
| CPU | 四核（4 核）支持 64 位 x86 的 Intel 或 AMD 处理器，2.5 GHz，带有 SSE 4.1 SIMD 指令集 |
| 内存 | 16 GB RAM（如果您限制用于编译的线程数，则 8GB RAM 可能是可以接受的） |
| GPU | - DirectX 12 或 Vulkan 兼容显卡 <br> - 2 GB VRAM <br> - Shader Model 6.2（或使用 Shader Model 6.3 的光线跟踪功能） <br> - 建议使用 NVIDIA GeForce GTX 1060 驱动程序版本 471.11 或更高版本 <br> - AMD Radeon Pro 560 或 <br> - Intel HD 630 <br> |
| 硬盘  | 40 GB （使用 [预构建的安装程序](https://o3debinaries.org/download/windows.html)）_或_ 100+ GB（取决于项目配置）的可用磁盘空间 |
| 显示器 | 1366 x 768 像素屏幕分辨率 |

### 建议的硬件规格

| 硬件组件 |最低推荐规格 |
|------| - |
| CPU  | 六核（6 核）支持 64 位 x86 的 Intel 或 AMD 处理器，2.5 GHz，支持 SSE 4.1 SIMD 指令集 |
| 内存   | 32 GB RAM |
| GPU  | - DirectX 12 或 Vulkan 兼容显卡 <br> - 6 GB VRAM <br> - Shader Model 6.2（或使用 Shader Model 6.3 的光线跟踪功能） <br> - NVIDIA GeForce GTX 16 系列，建议使用驱动程序版本 471.11 或更高版本 _ 或 _<br> - AMD RX 5000 系列 |
| 硬盘   | 具有 1 TB 可用磁盘空间的 SSD |
| 显示器  | 1366 x 768 像素屏幕分辨率 |

## 软件先决条件和配置 {#software-prerequisites}

创建新项目或使用 O3DE 的高级开发功能需要多个软件组件，具体取决于您使用的操作系统。

## Microsoft Windows

目前，Microsoft Windows 是使用 O3DE 的主要平台
编辑器和用于构建源。具体而言，需要 **Windows 10 版本 20H2 （10.0.19042）** 或更高版本。

### Microsoft Visual Studio

支持以下版本的 Visual Studio：

+ [Microsoft Visual Studio 2019](https://docs.microsoft.com/en-us/visualstudio/releases/2019/release-notes) 版本 **16.11.x**.
+ [Microsoft Visual Studio 2022](https://docs.microsoft.com/en-us/visualstudio/releases/2022/release-notes) 版本 **17.3.x**.

您可以使用任何 Microsoft Visual Studio 许可证，包括社区版。

此外，还需要 10.0.19041.0 或更高版本的 **Windows 10 SDK** 版本。

#### Visual Studio 配置

默认的 Visual Studio 安装可能不包括 O3DE 所需的所有功能。以下步骤介绍如何确保启用必要的 Visual Studio 功能：

1. 启动 **Visual Studio 安装程序**。

1. 在您将用于 O3DE 的 Visual Studio 版本上选择 **修改**。
   {{< note >}}
   确保您正在修改 Visual Studio [社区版， 企业版， 专业版] 的安装，而不是其他产品，例如 Visual Studio Build Tools。
   {{< /note >}}

1. 在 **Workloads** 选项卡上：
   + 选择 **使用 C++ 的游戏开发**。
      + 在右侧的 **安装详细信息** 面板中，选择 **10.0.19041.0** 或更高版本的 **Windows 10 SDK** 版本。
   + 选择 **使用 C++ 的桌面开发**。

1. 完成更改后，选择右下角的 **安装** 按钮，选择您喜欢的下载选项。
  {{< note >}}
  如果您对现有安装进行了更改，您可能会在选项窗口的右下角看到一个 **修改** 按钮。
  {{< /note >}}

安装完成后，系统可能会提示您重新启动系统。

### Microsoft Visual Studio C++ 可再发行组件

如果 Visual Studio 的 [Visual C++ Redistributable for Visual Studio](https://visualstudio.microsoft.com/downloads/#other-family) 不是由 Visual Studio 的 **使用 C++ 的桌面开发** 工作负载安装的，则您还需要最新版本。

安装 C++ 可再发行组件后，系统可能会提示您重新启动系统。

### CMake

[CMake {{< versions/cmake >}} or later](https://cmake.org/download/#latest) 是配置和构建 O3DE 项目所必需的。我们强烈建议您从 CMake 下载页面安装 **最新版本**，而不是 Release Candidate。在安装过程中，选择将 CMake 添加到系统 PATH 的选项之一。这将使您以后不必这样做。

   ![Add CMake to the system PATH during installation](/images/welcome-guide/requirements-cmake-install-add-to-path.png)

#### CMake配置验证

一些 O3DE CLI 脚本要求在命令行窗口中使用`cmake.exe`命令行工具。要查看此工具是否在系统路径上，请打开命令提示符并使用`--version` 命令。

```cmd
cmake --version
```

如果由于找不到 CMake 而未返回当前 CMake 版本，请在 CMake 安装目录中找到 `bin`文件夹，并通过执行以下操作将该文件夹的路径添加到 Windows 系统`PATH`环境变量的前面：

1. 打开Windows开始菜单。

1. 键入 “env” 以搜索环境变量。选择**编辑系统环境变量**。

1. 选择 **环境变量** 按钮。

1. 选择 **系统变量** 下的 **路径**。

1. 选择 **编辑...**按钮。

1. 选择 **新建** 向列表添加新路径，并输入 CMake`bin`文件夹的完整路径。

   ![Manually add CMake to the system PATH](/images/welcome-guide/requirements-cmake-add-to-path-manually.png)

1. （推荐）选择 **上移** 将路径移动到列表顶部。

1. 选择 **确定** 以保存更改并关闭已打开的窗口。

1. 打开新的命令行窗口并再次运行 `cmake --version` ，验证 `cmake` 是否在系统路径上。

## Linux

使用 O3DE 编辑器的主要 Linux 发行版是 Ubuntu {{< versions/ubuntu >}}。

{{< note >}}
对 Ubuntu 24.04 LTS 和 64 位 ARMv8 处理器上的 Ubuntu 的支持处于实验阶段。
{{< /note >}}

以下说明描述了如何通过 Ubuntu 的`apt`命令行实用程序检索和安装所需的软件包。

### CMake {#linux-cmake}

与其他操作系统一样，配置和构建 O3DE 项目需要 [CMake {{< versions/cmake >}} 或更新版](https://cmake.org/download/#latest)。我们强烈建议您安装 CMake 的 **最新版本**，而不是当前 Linux 发行版提供的默认版本。如果 CMake 已安装，但与最低版本不匹配，则需要使用以下命令将其删除。

```shell
sudo apt remove cmake
```

使用已安装的 Ubuntu 版本的说明安装 CMake：

{{< tabs name="CMake install" >}}
{{% tab name="22.04 LTS" %}}

您可以安装适用于 Ubuntu 22.04 LTS 的默认 CMake 版本，即 3.22 版。要安装更新的版本，请参考 CMake [下载页面](https://cmake.org/download/#latest)。

```shell
sudo apt install cmake
```

{{% /tab %}}
{{% tab name="24.04 LTS" %}}

您可以安装适用于 Ubuntu 24.04 LTS 的默认 CMake 版本，即 3.28 版。要安装更新的版本，请参考 CMake [下载页面](https://cmake.org/download/#latest)。

```shell
sudo apt install cmake
```

{{% /tab %}}
{{< /tabs >}}

安装后，验证版本是否满足最低版本标准。

```shell
cmake --version
```

### Clang

O3DE 需要 [Clang {{< versions/clang >}} 或更高版本](https://clang.llvm.org/get_started.html) 和 [GNU C++ 库](https://gcc.gnu.org/onlinedocs/libstdc++/) 来编译所有原生 C++ 代码。

按照已安装的 Ubuntu 版本的说明安装 Clang 和 GNU C++ 库：

{{< tabs name="Clang install" >}}
{{% tab name="22.04 LTS" %}}

您可以安装适用于 Ubuntu 22.04 LTS 的默认 Clang 版本，即 clang-14。您还需要安装相应的 [GNU C++ 库](https://gcc.gnu.org/onlinedocs/libstdc++/)。

```shell
sudo apt install libstdc++-12-dev clang
```

{{% /tab %}}
{{% tab name="24.04 LTS" %}}

您可以安装适用于 Ubuntu 24.04 LTS 的 Clang 的默认版本，即 clang-18。您还需要安装相应的 [GNU C++ 库](https://gcc.gnu.org/onlinedocs/libstdc++/)。

```shell
sudo apt install libstdc++-12-dev clang
```

{{% /tab %}}
{{< /tabs >}}

### Vulkan 支持的视频驱动程序

除了 O3DE 视频卡的最低硬件要求外，Linux 还要求安装并启用视频卡的最新驱动程序。有关如何执行此操作的说明，请参阅视频卡制造商的支持页面。

### 其他库依赖项

O3DE 还需要安装一些额外的库包：

* libglu1-mesa-dev
* libxcb-xinerama0
* libxcb-xinput0
* libxcb-xinput-dev
* libxcb-xfixes0-dev
* libxcb-xkb-dev
* libxkbcommon-dev
* libxkbcommon-x11-dev
* libfontconfig1-dev
* libpcre2-16-0
* zlib1g-dev
* mesa-common-dev
* libstdc++-12-dev
* libunwind-dev
* libzstd-dev
* tix

您可以通过`apt`下载并安装这些软件包。

```shell
sudo apt install libglu1-mesa-dev libxcb-xinerama0 libxcb-xinput0 libxcb-xinput-dev libxcb-xfixes0-dev libxcb-xkb-dev libxkbcommon-dev libxkbcommon-x11-dev libfontconfig1-dev libpcre2-16-0 zlib1g-dev mesa-common-dev libunwind-dev libzstd-dev tix
```

### Ninja 构建系统（可选）

默认情况下，CMake 使用 Unix Makefile 来构建 O3DE。O3DE 支持多种构建配置 （debug、profile 和 release），这些配置是在构建 O3DE 时指定的。Unix Makefile 仅支持单一配置构建，因此您必须在生成项目时确定要构建哪个配置。所有项目构建都基于该配置。为了更改 build 配置，您需要使用不同的配置重新生成项目。此过程限制了 O3DE 对多配置构建的支持，并减慢了构建工作流程。

Ninja 构建系统是 Linux 默认 Unix Makefile 的替代方案。Ninja 构建系统使生成、配置和构建项目变得更加容易。使用 Ninja 构建系统，特别是[Ninja Multi-Config](https://cmake.org/cmake/help/latest/generator/Ninja%20Multi-Config.html)，您可以生成一次项目并确定在构建时要构建的配置。我们建议您使用此生成器进行 O3DE 开发。您可以通过 `apt`安装 Ninja 构建工具。

```shell
sudo apt install ninja-build
```

## macOS

对在 macOS 上开发的支持处于试验阶段。您至少需要macOS Big Sur, Intel x86_64, XCode {{< versions/xcode >}} 或更高版本，以及满足上述硬件要求的 Metal 兼容视频卡。
