---
title: Open 3D Engine 在 Linux 上
linkTitle: Linux
description: Open 3D Engine 对 Linux 开发的支持概述。
weight: 200
---

**Open 3D Engine (O3DE)** 包括对 Linux 平台的支持。了解如何使用此处包含的说明为受支持的 Linux 版本构建 O3DE 工具和项目。

## 先决条件

以下说明假定您已：

* 在 GitHub 中以项目为中心的 [源引擎](/docs/user-guide/appendix/glossary#source-engine)  配置中设置 O3DE。如需帮助，请参阅 [从 GitHub 设置 O3DE](/docs/welcome-guide/setup/setup-from-github)。
* 在 O3DE 清单中注册了 O3DE 引擎。如果您从 GitHub 设置 O3DE，则必须手动注册引擎。如需帮助，请参阅 [注册引擎](/docs/welcome-guide/setup/setup-from-github/building-linux/#register-the-engine)。
* 满足 [O3DE 系统要求](/docs/welcome-guide/requirements) 中列出的所有 Linux 硬件和软件要求。

{{< note >}}
这些说明使用 `$O3DE_ENGINE` 来引用 O3DE 源代码在本地文件系统上的绝对路径。
{{< /note >}}

## 创建 O3DE 项目

如果您尚未创建 O3DE 项目，请立即使用 `o3de` 脚本进行创建。 $O3DE_PROJECT_PATH指项目的绝对路径，$O3DE_PROJECT_NAME引用项目的名称。

```shell
$O3DE_ENGINE/scripts/o3de.sh create-project --project-path $O3DE_PROJECT_PATH
```

有关创建新 O3DE 项目的更多信息，请参阅 [使用命令行界面创建项目](/docs/welcome-guide/create/creating-projects-using-cli)。

## 生成 build 文件夹

尽管 CMake 在 Linux 上支持 Unix Make Files，但我们建议您使用 Ninja 作为构建系统，以支持生成的构建中的多个配置。这些说明使用 “Ninja Multi-Config” 作为 CMake 生成器。

{{< important >}}
使用 O3DE 预构建 **Snap** SDK 进行构建时，首先导出`O3DE_SNAP`环境变量，以便 CMake 不会尝试安装 Python pip 要求并失败。要导出`O3DE_SNAP`环境变量，请在运行下面的 CMake 命令之前从命令行运行命令 `export O3DE_SNAP`  。
{{< /important >}}

### 多个 Config 构建
以下命令使用 Ninja 作为构建生成器，使用 clang-12 作为编译器，在项目文件夹`$O3DE_PROJECT_PATH`的根目录下生成一个构建文件夹 `build/linux`。


```shell

cd $O3DE_PROJECT_PATH

cmake -B build/linux -S . -G "Ninja Multi-Config"

```

### 单个配置构建

如果你不需要生成多配置构建文件夹，你可以指定 “Unix Makefiles” 作为 CMake 的生成器。您需要在项目生成时指定配置。$BUILD_CONFIG 的有效值包括：`debug`, `profile`, 和 `release`。

```shell

cd $O3DE_PROJECT_PATH

cmake -B build/linux -S . -G "Unix Makefiles" -DCMAKE_BUILD_TYPE=$BUILD_CONFIG

```

## 从命令行构建

生成 build 文件夹后，从命令行构建的过程与其他平台相同。

{{< important >}}
使用 O3DE 预构建 **Snap** SDK 进行构建时，首先导出`O3DE_SNAP`环境变量，以便 CMake 不会尝试安装 Python pip 要求并失败。要导出`O3DE_SNAP`环境变量，请在运行下面的 CMake 命令之前从命令行运行命令 `export O3DE_SNAP`。
{{< /important >}}

### 多个 Config 构建

在构建使用 Ninja Multi-Config 生成器生成的项目时，请在 build 命令中包含构建配置。

```shell

cd $O3DE_PROJECT_PATH

cmake --build build/linux --config profile --target $O3DE_PROJECT_NAME.GameLauncher Editor
```

### 单个配置构建

在构建使用 Unix Makefile 生成器生成的项目时，CMake 将使用您在项目生成期间指定的配置。

```shell

cd $O3DE_PROJECT_PATH

cmake --build build/linux --target $O3DE_PROJECT_NAME.GameLauncher Editor
```

## 高级主题

### 生成编译数据库

要支持代码完成和 IDE （如 Visual Studio）中的其他 IntelliSense 功能，请指示 CMake 生成编译数据库 ([compile-command.json](https://clang.llvm.org/docs/JSONCompilationDatabase.html))文件作为项目生成命令的一部分。

{{< note >}}
IDE 只有在 Unity 版本关闭时才能使用 `compile-command.json` 。由于 Unity 版本在 O3DE 中默认启用，因此您需要使用`-DLY_UNITY_BUILD=OFF`参数显式关闭它。
{{< /note >}}

要启用编译数据库文件的生成，请在工程生成命令中包含以下参数：

```shell
-DCMAKE_EXPORT_COMPILE_COMMANDS=ON -DLY_UNITY_BUILD=OFF
```
