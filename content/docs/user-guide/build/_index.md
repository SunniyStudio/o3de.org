---
title: 构建
description: 了解 Open 3D Engine 支持 CMake 构建系统的基础知识，并开始首次完整构建 Open 3D Engine 源代码和 Atom 测试项目。
weight: 200
---

为了支持多种本地构建工具链，**Open 3D Engine** (O3DE)使用了[CMake构建工具](https://cmake.org/)。大多数可配置的构建系统都很难跨平台工作，而 CMake 的设计初衷是获取通用配置文件并生成特定于工具链的项目文件，然后执行本地构建。

 一旦您[注册了 O3DE](/docs/welcome-guide/setup) 并[创建了一个 O3DE 项目](/docs/user-guide/project-config/project-manager/#creating-projects-using-project-manager)，您就可以使用这些命令构建您的项目：

{{< tabs name="CMake配置示例" >}}
{{% tab name="Windows" %}}

```cmd
cd <project-directory>
cmake -B build/windows -S . -G "Visual Studio 16" -DLY_3RDPARTY_PATH=<absolute-path-to-packages>
cmake --build build/windows --target <ProjectName>.GameLauncher Editor --config profile -- -m
```

{{< note >}}
Visual Studio 2019 使用 `Visual Studio 16`作为生成器，Visual Studio 2022 使用 `Visual Studio 17` 作为生成器。有关各支持平台的常用生成器的完整列表，请参阅[配置项目](./configure-and-build/#configuring-projects)。
{{< /note >}}

{{% /tab %}}
{{% tab name="Linux" %}}

{{< important >}}
在使用 O3DE 预编译 **Snap** SDK 构建时，首先导出 `O3DE_SNAP` 环境变量，这样 CMake 就不会尝试安装 Python pip 要求而失败。要导出 `O3DE_SNAP` 环境变量，请在运行下面的 CMake 命令之前，在命令行中运行 `export O3DE_SNAP` 命令。
{{< /important >}}

```shell
cd <project-directory>
cmake -B build/linux -S . -G "Ninja Multi-Config" -DLY_3RDPARTY_PATH=<absolute-path-to-packages>
cmake --build build/linux --target <ProjectName>.GameLauncher Editor --config profile
```

{{% /tab %}}
{{< /tabs >}}

使用这些命令创建的构建文件位于`<project-directory>/<build-directory>/bin/profile`目录中。

有关其他构建配置的列表，请参阅 [配置和构建](configure-and-build)。

O3DE 需要 CMake {{< versions/cmake >}} 或更高版本。

## 章节主题

| 主题 | 说明 |
| --- | --- |
| [配置和构建](configure-and-build) | 关于如何配置和构建 O3DE 核心、Gem 和项目的全部详细信息。 |
| [构建生成的源文件](generated-source) | 了解如何使用 AzAutoGen 自动化工具在构建目标时生成源文件。 |
| [引擎和项目分发](distributable-engine) | 说明如何通过创建固定二进制文件分发给项目团队，将引擎开发人员和项目开发人员的工作流程分开。 |
| [O3DE 软件包系统](packages) | 了解用于随 Gem 或项目一起发布二进制文件的 O3DE 软件包系统。 |
| [故障排除](troubleshooting) | 如何调试 CMake 和构建问题并排除故障。 |
| [CMake设置参考](reference) | O3DE 专用的用户可配置 CMake 设置参考。 |
| [仅使用的'Quick-Start'项目](script-only-projects) | 有关仅使用脚本'Quick Start'项目的详细信息。 |
| [模板](templates) | 有关项目、Gem和引擎中其他模板的信息 |

## 相关主题

| 主题 | 说明 |
|---|---|
| [Windows 支持](/docs/user-guide/platforms/windows) | 在 Windows 10 上进行构建的前提条件。|
| [Linux 支持](/docs/user-guide/platforms/linux) | 在 Linux 上进行构建的前提条件。|
