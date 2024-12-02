---
linktitle: 故障排除
title: O3DE 项目配置疑难解答
description: Open 3D Engine （O3DE） 中项目配置的故障排除指南。
toc: true
weight: 160
---

本主题将帮助您识别和解决在项目配置过程中可能遇到的一些常见问题。如果您在此处没有找到您的问题，请尝试搜索 [我们的论坛](https://github.com/o3de/o3de/discussions)或在 [O3DE Discord](https://{{< links/o3de-discord >}}) 中提问.

如果您认为您的配置问题是由于 O3DE 中的错误引起的，请检查 [现有错误报告](https://github.com/o3de/o3de/issues) 和 [提交问题](https://github.com/o3de/o3de/issues/new/choose), 如果可以的话！

要查找错误日志或内存转储？请参阅 [Open 3D Engine日志文件](/docs/user-guide/appendix/log-files) 了解位置。

## 添加和删除 Gem 的提示

- 当您将新 Gem 注册到项目时，该 Gem 目录的路径将添加到项目的`project.json`配置文件的 `external_subdirectories`下。

- 要检查是否为您的项目启用了 Gem，您可以在项目文件夹的`Code/enabled_gems.cmake` 文件中找到已启用的 Gem 列表。

## CMake 错误：不是现有目录

**问题：** CMake 找不到指定 Gem 的目录。将外部 Gem 添加到项目时，必须将 Gem 的目录注册到项目中。这使您的项目能够找到 Gem。如果 Gem 的目录位置已更改，则可能会出现此问题。

**补救措施：** 检查 Gem 的正确路径是否已注册到您的项目中。您可以在项目的`project.json`配置文件的`external_subdirectories`下找到已注册 Gem 的路径。如有必要，请更新 Gem 的目录路径，或清理任何过时的路径。
