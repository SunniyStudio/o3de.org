---
title: 在 Linux 上创建项目
description: 了解如何在 Linux 上使用命令行界面 （CLI） 从默认项目模板创建和构建新的 Open 3D Engine （O3DE） 项目。
weight: 200
toc: true
---

使用本教程中的说明为 Linux 主机平台创建 O3DE 项目。您可以在与 O3DE 根目录相同的目录中或在此目录之外创建项目目录。本文档将后者称为 “外部项目”。

## 先决条件

以下说明假定您已：

* 满足 [O3DE 系统要求](/docs/welcome-guide/requirements) 中列出的所有硬件和软件要求。
* 在您的计算机上设置 O3DE。有关帮助，请参阅 [设置 Open 3D Engine](/docs/welcome-guide/setup)。
* 在 O3DE 清单中注册 O3DE 引擎。如果您从 GitHub 设置 O3DE，则必须手动注册引擎。如需帮助，请参阅 [注册引擎](/docs/welcome-guide/setup/setup-from-github/building-linux/#register-the-engine)。

## 创建新的 O3DE 项目

要基于标准模板启动项目，请完成以下步骤。

1. 打开终端窗口，然后通过执行以下操作之一更改为 O3DE 引擎目录：

    * 如果你将引擎设置为 [源引擎](/docs/user-guide/appendix/glossary/#source-engine)，请使用 engine 源目录。例：

        ```shell
        cd $HOME/o3de
        ```

    * 如果您安装了 O3DE 或使用`INSTALL`目标将引擎构建为 [SDK 引擎](/docs/user-guide/appendix/glossary/#sdk-engine)，请使用已安装的引擎目录。例：`

        ```shell
        cd $HOME/o3de-install
        ```

1. 要创建一个新项目，请使用 `scripts` 子目录中的 `o3de` 脚本和 `create-project` 命令。使用 `--project-path` 参数提供要创建新项目的目录的绝对或相对路径。路径的最后一个组件将成为项目名称，除非你用 `--project-name` 参数覆盖它。该脚本将使用 **standard** 模板（默认项目模板）创建一个新项目。

    ```shell
    scripts/o3de.sh create-project --project-path $HOME/O3DE/Projects/MyProject
    ```

    在创建项目时，此命令还会进行两个重要的注册：

    * 它通过使用引擎的注册名称在项目的`project.json`清单中注册引擎，将您的项目与引擎相关联。您可以在项目的根目录中找到此清单。名为 `o3de` 的引擎的注册类似于以下示例：

        ```json
        "engine": "o3de",
        ```

        {{< note >}}
如果你更改了引擎的注册名称，或者希望在项目中使用不同的引擎，则需要更新此清单条目。
        {{< /note >}}

    * 它在 O3DE 清单中注册项目，将其添加到已知项目列表中，并让 **Project Manager** 知道您的项目。O3DE 清单位于`$HOME/.o3de/o3de_manifest.json`中。位于`/home/myusername/O3DE/projects/MyProject` 中的项目注册类似于以下示例：

        ```json
        "projects": [
            "/home/myusername/O3DE/projects/MyProject"
        ],
        ```

        {{< note >}}
如果移动项目，则需要更新此清单条目。
        {{< /note >}}

## 创建 Linux 构建项目

使用 **CMake** 为您的 O3DE 项目创建 Linux 构建项目。

{{< important >}}
使用 O3DE 预构建 **Snap** SDK 进行构建时，首先导出`O3DE_SNAP`环境变量，以便 CMake 不会尝试安装 Python pip 要求并失败。要导出 `O3DE_SNAP` 环境变量，请在运行下面的 CMake 命令之前从命令行运行命令`export O3DE_SNAP` 。
{{< /important >}}

1. 在新项目目录中创建 Linux 生成项目。提供构建目录、项目源目录、Ninja Multi-Config 生成器和任何其他项目选项。路径可以是绝对路径，也可以是相对路径。例：

    ```shell
    cd $HOME/O3DE/Projects/MyProject
    cmake -B build/linux -S . -G "Ninja Multi-Config"
    ```

    {{< note >}}
CMake 使用 O3DE 清单中定义的可下载包目录`default_third_party_folder`。您可以通过包含`-DLY_3RDPARTY_PATH`参数来指定要使用的其他目录。例如，如果您在 `$HOME/o3de-packages`中创建了包目录，请在 cmake 命令中包含参数 `-DLY_3RDPARTY_PATH=$HOME/o3de-packages`。

指定 packages 目录的路径时，不要使用尾部斜杠。
    {{< /note >}}

    {{< note >}}
CMake [unity builds](https://cmake.org/cmake/help/latest/prop_tgt/UNITY_BUILD.html) 默认处于开启状态。这是一项 CMake 功能，可以通过将源文件合并为单个编译单元来大大缩短构建时间。如果遇到构建错误，禁用 Unity 构建可能有助于调试问题。要禁用 Unity 构建，请运行前面的带有 `-DLY_UNITY_BUILD=OFF` 参数的 `cmake` 命令来重新生成项目文件。
    {{< /note >}}

## 构建 O3DE 项目

使用 CMake 在 O3DE 项目的 build 目录中构建 Linux 构建项目。

1. 使用您在项目的`build/linux`目录中创建的解决方案构建项目启动器。以下示例显示了 `profile`生成配置。
    ```shell
    cmake --build build/linux --target MyProject.GameLauncher Editor --config profile -j <number of parallel build tasks>
    ```

    {{< important >}}
在为预构建的 SDK 引擎构建项目时，即使您没有构建 **O3DE 编辑器**，我们仍然强烈建议将`Editor`作为构建目标。虽然 GameLauncher 不依赖于 Editor 目标，但某些 Gem 依赖于 Editor。如果您关闭 Editor 目标，则这些 Gem 不会包含在构建中。
    {{< /important >}}

    在为源引擎构建项目时，您还需要构建 **Asset Processor** 和 Project Manager，因为它们是 O3DE Editor 的依赖项。

   `-j`是推荐的构建工具优化。它告诉 Ninja 构建工具将同时执行的并行构建任务的数量。建议使用“number of parallel build tasks”的值与 Linux 主机上可用的内核数匹配。

    例如:

    ```shell
    cmake --build build/linux --target MyProject.GameLauncher Editor --config profile -j 8
    ```

1. 构建完成后，您可以在项目目录的`build/linux/bin/profile`下找到项目二进制文件。要验证项目是否已准备就绪，请通过执行以下操作之一来运行 O3DE Editor：

    * 如果您将引擎设置为 [源引擎](/docs/welcome-guide/setup/setup-from-github/building-linux/#build-the-engine)，请从项目构建目录运行 Editor。

        ```shell
        build/linux/bin/profile/Editor
        ```

        {{< note >}}
如果您的项目构建目录在项目路径之外，则必须在启动 O3DE Editor 时包含项目路径（使用 `--project-path`参数）。
        {{< /note >}}

    * 如果您安装了 O3DE 或使用 `INSTALL` 目标将引擎构建为 [SDK 引擎](/docs/welcome-guide/setup/setup-from-github/building-linux/#build-the-engine)，请从已安装引擎的构建目录运行 Editor。（如果您未提供项目路径，则 **Project Manager** 将启动。项目路径可以是绝对路径，也可以是相对于 engine 目录的路径。

        ```shell
        $HOME/o3de-install/bin/Linux/profile/Default/Editor --project-path $HOME/O3DE/Projects/MyProject
        ```

        {{< important >}}
如果您使用 `INSTALL` 目标从源代码构建引擎，请确保从已安装引擎的构建目录（而不是引擎的构建目录）启动 Editor _和_ 其他工具。Linux 安装目录通常以 `/bin/Linux/profile/Default` 结尾。
        {{< /important >}}

您还可以从同一目录运行 Project Manager (`o3de`) 来编辑项目的设置、在项目中添加或删除 Gem、重新构建项目以及启动 Editor。

{{< caution >}}
当您启动 Editor 时，同一目录中的 **Asset Processor** 也将启动。 要从其他目录启动 Editor，您必须关闭正在运行的所有 **Asset Processor** 任务。
{{< /caution >}}

有关项目配置和构建的更多信息，请参阅用户指南的 [项目配置](/docs/user-guide/project-config) 和 [构建](/docs/user-guide/build) 部分。
