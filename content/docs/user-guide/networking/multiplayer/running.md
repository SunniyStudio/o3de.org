---
linkTitle: 运行 Multiplayer 项目
title: 运行 Multiplayer 项目
description: 如何在 Open 3D Engine （O3DE） 中运行支持多人游戏的项目。
weight: 300
---

使用 Open 3D Engine （O3DE） 中的 **Multiplayer Gem** 运行项目需要您正确构建项目并设置配置选项以运行项目。

## 构建你的项目

在构建项目时，请务必执行以下操作：

* 确保项目[已经正确配置](./configuration)。
* 构建项目的 `GameLauncher` 和 `ServerLauncher` 目标。

## 运行你的项目

接下来，在独立模式下运行游戏，使用 ImGui 选项进行托管，或使用 **O3DE 编辑器**。

### 在 Editor 中测试

您可以使用 **Ctrl+G** 在 **O3DE 编辑器** 中运行多人游戏项目。有关更多信息，请参阅 [在 O3DE 编辑器中测试多人游戏项目](./test-in-editor).

### 使用 ImGui 选项在本地运行

1. 从Visual Studio或Build目录打开 `ServerLauncher`。
2. 按下 `HOME` 键启用调试菜单。
3. 选择`Host` 选项。
4. 选择开始托管时要加载的关卡。
5. 使用`Launch Local Client`选项以自动启动连接到 服务器的客户端。

### 独立运行服务器和客户端启动器

#### 使用应用程序的控制台进行手动配置

您可以手动启动 Client 和 Server 的可执行文件，并使用应用程序的控制台命令行对其进行配置。

1. 启动 `ClientLauncher` 和 `ServerLauncher`。
2. 在 `ServerLauncher` 中按 `~` 打开命令行提示符。
3. 运行命令 `host` 以开始托管。
4. 运行命令 `LoadLevel <path to level>` 加载一个关卡。
5. 在 `ClientLauncher` 中按 `~` 打开命令行提示符。
6. 运行命令 `connect <IP Address:Port>` 连接到服务器。如果本地运行，`connect` 将默认为 localhost。

#### 使用预定义的配置文件

您可以手动启动 Client 和 Server 的可执行文件，并将预定义的配置文件传递给每个可执行文件，以便它们在启动时执行。这些 cfgs 文件中的命令按列出的顺序执行。

1. 在项目目录的根目录中创建 `launch_client.cfg` 和 `launch_server.cfg`。

2. 打开 `launch_server.cfg`并将其编辑为如下所示：

    ```txt
    host
    LoadLevel <path to Level>
    ```

3. 打开 `launch_client.cfg` 并将其编辑为如下所示：

    ```txt
    connect <IP Address:Port>
    ```

    或者，您可以执行以下操作，以针对在默认端口上运行的 localhost 服务器进行测试。

    ```txt
    connect
    ```

4. 使用 `MultiplayerSample.ServerLauncher.exe --console-command-file=launch_server.cfg`运行服务器

5. 使用 `MultiplayerSample.ClientLauncher.exe --console-command-file=launch_client.cfg`运行客户端

必须先运行 Server，以便 Client 有要连接的主机。

上述命令可以从 build 目录中的命令行运行，也可以通过在首选执行方法中设置命令行参数来运行。
