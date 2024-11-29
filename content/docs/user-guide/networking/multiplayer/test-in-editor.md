---
linktitle: 在 Editor 中测试
title: 在 O3DE 编辑器中测试 Multiplayer 项目
description: 了解如何使用 O3DE 编辑器在 Open 3D Engine （O3DE） 中测试和编辑同步多人游戏项目。
weight: 500
---

多人游戏模拟的一个重要部分是了解客户端在连接到服务器时的外观和性能。为了帮助实现这一点，**Open 3D Engine （O3DE）** 允许您自动将客户端连接到服务器。在 **O3DE 编辑器** 中，如果您的联网实体在现场并且您有当前的服务器启动器，您可以进入 Play Mode （**Ctrl+G**） 来本地启动您的服务器。在此过程中，Editor 会将场景中的所有实体一次性发送到您的服务器，以便随后可以加载它们。然后，Editor 作为客户端连接到服务器。

此功能的性质是 Editor 在将控制权交给 Server 之前将世界状态授权给 Server。由于此过程本身存在风险，因此此功能是为 Release 版本编译的。

### 高级配置

**多人游戏 Gem** 公开了各种 cvar，以帮助您配置在 Editor 进入 Play Mode 时如何启动服务器。

|CVar 名称 |类型 |描述 |
|--|--|--|
| `editorsv_enabled` | `bool` | 启用或禁用对在播放模式启动时启动本地服务器的支持。 |
| `editorsv_launch` | `bool` | 当服务器地址为 `localhost` 时，Editor 是否应启动服务器。如果您想从附加了调试器的 IDE 启动服务器，则关闭此选项非常有用。 |
| `editorsv_process` | `string` | 应运行的服务器可执行文件的完整路径。空以使用当前项目的 ServerLauncher。 |
| `editorsv_serveraddr` | `string` | 要连接到的服务器的地址。如果您在进入 Play 模式时希望连接到正在运行的服务器，请更改此项。默认情况下，该值为`127.0.0.1`。 |
| `editorsv_port` | `uint16_t` |多人游戏 Gem 绑定到的流量端口。 |
| `editorsv_isDedicated` | `bool` | Server 是否应作为主机启动。Editor 使用此设置来告知它启动的本地服务器立即开始托管其 Editor 连接，以便 Editor 可以连接。 |
| `editorsv_rhi_override` | `string` | 在启动 Editor 服务器时覆盖默认渲染硬件接口 （rhi）。例如，您可能正在使用 'dx12' 运行编辑器，但希望使用 'null' 启动无头服务器。如果为空，服务器将使用与 Editor 相同的 rhi 启动。 |

Using these features, you can configure the Editor to connect to a compatible remote host if you wanted or a specific server executable. For example, you could configure your profile Editor to connect to a debug server.

![The Editor connected to a dedicated server](/images/user-guide/networking/multiplayer/editor_client_with_dedicated_server_mode.png)

#### C在客户端-服务器模式下配置编辑器

您可以将 Editor 配置为同时充当服务器和客户端。这允许在按 CTRL-G 时立即启动游戏模式。（另一方面，使用上述配置的专用服务器进入游戏模式需要几秒钟。此模式需要将两个 CVar 设置为 true： `editorsv_enabled` 和 `editorsv_clientserver`.

|CVar 名称 |类型 |描述 |
|--|--|--|
| `editorsv_clientserver` | `bool` | Editor 是否应同时充当服务器和客户端，而不启动专用服务器进程。这对于不需要测试仅客户端逻辑的快速迭代工作非常有用。 |

您可以通过查看 Editor 视区右下角的调试文本来判断您处于哪个多人编辑器游戏模式。

![The Editor with editorsv_clientserver set to true](/images/user-guide/networking/multiplayer/editor_clientserver_mode.png)
