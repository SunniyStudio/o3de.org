---
linktitle: 故障排除
title: O3DE 中的打包疑难解答
description: 用于解决打包 Open 3D Engine （O3DE） 项目的问题的提示、技巧和建议。
weight: 500
---

本指南可以帮助您排查和解决在打包 Open 3D Engine （O3DE） 项目时可能遇到的一些常见问题。

请注意，您可能会遇到项目特有的情况，此处未提及。如果您在此处没有找到您的问题，请尝试搜索 [我们的论坛](https://github.com/o3de/o3de/discussions) 或在 [O3DE Discord](https://{{< links/o3de-discord >}}) 中提问。

如果您认为您的打包问题是由于 O3DE 中的错误引起的，请检查 [现有错误报告](https://github.com/o3de/o3de/issues) 和 [提交问题](https://github.com/o3de/o3de/issues/new/choose) 如果可以的话！

要查找错误日志或内存转储？请参阅 [打开 3D 引擎日志文件](/docs/user-guide/appendix/log-files) 了解位置。


## 故障排除技术

以下是可用于帮助您调试在构建要发布的项目时可能遇到的任何问题的技术。


### 在禁用优化并启用调试符号的情况下进行编译

在 Visual Studio 中，您可以通过将 CMake 变量 `O3DE_BUILD_WITH_DEBUG_SYMBOLS_RELEASE` 设置为 `ON` 进行配置，在禁用优化和启用调试符号的情况下进行编译。这样，Visual Studio 的调试工具就可以生成更多有用的信息，从而帮助你进行调试。


### 创建 `profile` 版本

要调试非整体式或整体式构建及其`.pak`文件，您可以使用`profile`配置进行构建。`profile` 配置会创建日志文件，这些文件提供的信息可以帮助您进行调试。有关 O3DE 日志文件的信息，请参阅 [打开 3D 引擎日志文件](/docs/user-guide/appendix/log-files)。

使用 CMake 调用 Visual Studio 以使用`profile` 配置构建您的项目。

```cmd
cd C:\MyProject
cmake --build <build> --target INSTALL --config profile
```

将 `<build>` 替换为以下任一值：
- `build\windows` -- 对于非整体式构建。
- `build\windows_mono` -- 对于整体式构建。
