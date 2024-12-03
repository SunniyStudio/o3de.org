---
linktitle: Troubleshooting the Setup
title: Troubleshooting the Setup of Open 3D Engine
description: Get troubleshooting help for setting up Open 3D Engine (O3DE).
weight: 400
toc: true
---

本指南将帮助您识别和解决在计算机上设置 **Open 3D Engine （O3DE）** 时可能遇到的一些常见问题。请注意，您可能会遇到系统或项目特有的情况，这些情况在此处未解决。此参考仅适用于不受已知 bug 影响或无法轻松解决的最常见设置问题。如果你没有找到这里涵盖的问题，请尝试搜索 [我们的论坛](https://github.com/o3de/o3de/discussions)或在 [O3DE Discord](https://{{< links/o3de-discord >}}) 中提问。

如果您认为您的设置问题是由于 O3DE 中的错误引起的，请检查 [现有错误报告](https://github.com/o3de/o3de/issues) 和 [提交问题](https://github.com/o3de/o3de/issues/new/choose)如果可以的话！

要查找错误日志或内存转储？请参阅 [Open 3D Engine日志文件](/docs/user-guide/appendix/log-files)了解位置。

## 在 PATH 上找不到 CMake

**问题：** 从 O3DE 引擎目录中的命令行运行工具（例如`get_python.bat`）报告在 PATH 上找不到 CMake，并且未定义 LY_CMAKE_PATH。

**补救措施：** 如果未安装，请安装 CMake。如果已安装 CMake 二进制文件，则可能未在操作系统中定义 CMake 二进制文件的路径。请参阅 [CMake](../requirements/#cmake) 部分，以获取有关如何安装和配置 CMake 的说明。

安装 CMake 或在计算机上定义其路径后，请打开一个新的命令行窗口，然后再尝试再次运行该工具。

## CMake 错误：3rdParty 文件夹不存在

**问题：** CMake 找不到 LY_3RDPARTY_PATH 指定的可下载包目录。这通常是在目录不存在时导致的。当传递给 CMake 的值以 `\` 字符结尾时，也可能导致该错误。

```cmd
CMake Error at cmake/3rdParty.cmake:34 (message):
  3rdParty folder: C:/o3de-packages does not exist, call cmake defining a
  valid LY_3RDPARTY_PATH or use cmake-gui to configure it
```

**补救措施：** 执行以下操作之一。

* 创建可下载的软件包目录。
* 更改值以删除尾随的`\`。
* 更改`LY_3RDPARTY_PATH`的格式以使用与平台无关的 `/` 路径分隔符。

## CMake 错误：找不到 MSVC 编译器

**问题：** CMake 工具报告缺少 MSVC 编译器。这将生成类似于以下内容的警告：

```cmd
CMake Error at CMakeLists.txt:15 (project):
  The CMAKE_C_COMPILER:
 
    C:/Program Files (x86)/Microsoft Visual Studio/2019/Professional/VC/Tools/MSVC/14.24.28314/bin/Hostx64/x64/cl.exe
 
  is not a full path to an existing compiler tool.
```

这是在更新或修改 Visual Studio 时引起的，并且 CMake 缓存包含指向上一个编译器安装的信息。

**补救措施：** 此问题通常是在 Visual Studio 更新期间或之后引起的，无需重新生成 O3DE 项目文件。
C 和 C++ 编译器的路径由 CMake 系统在配置时使用 `CMAKE_C_COMPILER` 和 `CMAKE_CXX_COMPILER` 值进行设置。
这些值存储在 CMake 缓存中。通过执行以下操作之一清理缓存并重新配置：

* 删除 CMake 构建目录中的`CMakeCache.txt`文件。
* 完全删除 CMake 构建目录。

清理缓存后，应在 CMake 配置阶段检测到正确的编译器。

## GitHub 凭据在 Git 凭据管理器中不起作用

**问题：** 克隆 O3DE 存储库时，您在 Git Credential Manager 中输入了 GitHub 用户名和密码，但没有任何反应，对话框返回。

**补救措施：** 此问题通常在克隆 O3DE 存储库的过程中发生。Credential Manager （凭据管理器） 对话框请求 https URL 的凭据，如下图所示。

![Git Credential Manager asking for credentials for an https URL](/images/welcome-guide/setup-troubleshooting-git-credential-manager.png)

此 URL 链接到用于下载大型文件的 Git LFS 终端节点。您必须在此处使用 GitHub 个人访问令牌，而不是 GitHub 密码。请参阅 [从 GitHub 设置 O3DE ](/docs/welcome-guide/setup/setup-from-github/#configure-credentials-for-git-lfs) 中有关为 Git LFS 配置凭据的说明。

## 运行 `o3de` 脚本时找不到适用于 Windows 的 Python

**问题：** 您从 scripts 目录运行 o3de 脚本，并收到一条错误消息，指出找不到 Python for Windows，即使您的计算机上安装了 Python。

**补救措施：** scripts 目录中的 `o3de` 脚本需要 <O3DE>/python/runtime 目录中存在特定版本的 Python 运行时。要获取此运行时，请从安装 O3DE 的目录运行以下命令。

```cmd
python\get_python.bat
```

## 更多故障排除帮助

有关更多故障排除帮助，请参阅以下其他 O3DE 故障排除页面：

* [O3DE 项目配置疑难解答](/docs/user-guide/project-config/troubleshooting)
* [在 Open 3D Engine 中构建疑难解答](/docs/user-guide/build/troubleshooting)

提醒一下，您还可以尝试在以下位置搜索您的问题或寻求帮助：

* [O3DE 论坛](https://github.com/o3de/o3de/discussions)
* [O3DE Discord](https://{{< links/o3de-discord >}})
* [现有 bug 报告](https://github.com/o3de/o3de/issues)
