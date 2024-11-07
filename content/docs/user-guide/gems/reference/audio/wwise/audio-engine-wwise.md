---
linkTitle: Wwise Audio Engine
title: Wwise Audio Engine Gem
description: Wwise Audio Engine Gem支持在 Open 3D Engine （O3DE） 项目中使用 Audiokinetic Wave Works Interactive Sound Engine （Wwise）。
toc: true
---

Wwise Audio Engine Gem 支持在 Open 3D Engine （O3DE） 项目中使用 Audiokinetic Wave Works 互动声音引擎 （Wwise）。

Wwise Audio Engine Gem 需要 Audio System Gem。

有关详细信息，请参阅 [音频系统概述](/docs/user-guide/interactivity/audio/overview/)。

## 启用Wwise Audio Engine Gem

要启用 Wwise Audio Engine Gem，请执行以下操作：

1. 使用 **O3DE Project Manager** 或命令行将 Wwise Audio Engine Gem 添加到您的项目中。请注意，Wwise Audio Engine 需要 [Audio System Gem](/docs/user-guide/gems/reference/audio/audio-system) 作为依赖项。
 
1. 下载 [Wwise Launcher](https://www.audiokinetic.com/download/)并使用它来安装 Wwise 音频 SDK 版本 2021.1.1.7601 或更高版本。确保在安装过程中选择 **SDK（C++）** 组件。
   
    {{< note >}}
通常，您可以使用比上述版本更新的 Wwise，但某些 SDK 更新需要更改代码。
    {{< /note >}}

1. （推荐）将 CMake 缓存变量 '`LY_WWISE_INSTALL_PATH`' 设置为安装 Wwise 的路径。您可以使用 '`cmake-gui`' 来设置此变量，也可以在运行 '`cmake`' 构建配置命令时设置它。通过使用这个缓存变量，如果将来更新该变量，将自动触发 CMake 工程重新生成。

    您可以使用以下 CMake 构建配置命令从项目目录中设置 '`LY_WWISE_INSTALL_PATH`' 。

    ```cmd
    cmake configure -B build/<platform> -G "Visual Studio 16" -DLY_3RDPARTY_PATH=<o3de-packages> -DLY_WWISE_INSTALL_PATH=<wwise-installation>
    ```

    {{< note >}}
使用“`Visual Studio 16`”作为 Visual Studio 2019 的生成器，使用“`Visual Studio 17`”作为 Visual Studio 2022 的生成器。有关每个受支持平台的常用生成器的完整列表，请参阅 [配置项目](/docs/user-guide/build/configure-and-build/#configuring-projects)。
    {{< /note >}}

1. 使用 Project Manager、Visual Studio 或 CMake 构建您的项目。

    {{< important >}}
每当将 Wwise Audio SDK 更新到较新版本时，如果您有任何使用 Wwise Audio Engine Gem 的现有 O3DE 工程，请确保使用新路径更新“`LY_WWISE_INSTALL_PATH`”。或者，如果您依赖于在 Wwise 安装过程中设置的“`WWISEROOT`”环境变量，则确保使用 “`cmake`” 重新生成 Visual Studio 工程文件（如果您希望它们使用较新的 SDK）。
    {{< /important >}}

在构建工程时，CMake 会按以下顺序检查是否存在特定变量来查找 Wwise 安装：

1. '`LY_WWISE_INSTALL_PATH`' CMake 缓存变量。

1. “`WWISEROOT`”环境变量，在安装 Wwise 时设置。
