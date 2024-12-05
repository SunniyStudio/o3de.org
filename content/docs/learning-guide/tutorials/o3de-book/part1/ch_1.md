---
linkTitle: 第1章 在Windows上安装O3DE
title: 第1章 在Windows上安装O3DE
description: 第1章 在Windows上安装O3DE
---
# 第1章 在Windows上安装O3DE
# 介绍
本章将涵盖以下主题：
* 为 O3DE 准备 Visual Studio 2019（或 2022）。
* 获取 O3DE。
* 安装 O3DE。（在撰写本书时，O3DE 21.11.2 版本已经可用。

## 准备Visual Studio
{{< note >}}
您可以在此处找到安装 O3DE 的官方指南：
https://www.o3de.org/docs/welcome-guide/setup/
{{< /note >}}

O3DE 是一个 C++ 游戏引擎，因此它需要一些本机软件开发工具包和 Visual Studio 的各种工具（如果您在 Windows 操作系统上构建）。默认情况下，Visual Studio 可能没有其中一些要求，因此我将介绍您确实需要安装的组件。

{{< note >}}
请务必参考有关此主题的官方参考资料：
https://www.o3de.org/docs/welcome-guide/requirements/
{{< /note >}}

{{< note >}}
截至 21.11，O3DE 支持的 Visual Studio 版本是 2019，但是，我没有发现同时使用 VisualStudio 2022 和 O3DE 21.11 有任何问题。任一 Visual Studio 版本的安装步骤都是相同的。
{{< /note >}}

以下是步骤：
1. 安装 Visual Studio 2019、Community 或其他版本。
2. 修改其安装以添加 Desktop development with C++ 和 Game development with C++。
3. 从 https://cmake.org/download/ 安装 CMake 3.20.5 或更高版本。在安装过程中，选择将 CMake 添加到系统 PATH 的选项之一。
4. 确认可以从命令行访问 CMake。
```shell
   PS C:\> cmake --version
   cmake version 3.22.2
```
5. 从您最喜欢的位置安装 Git，或者 https://git-scm.com/download/win

6. 确认可从命令行访问 Git。
```shell
   PS C:\> git --version
   git version 2.35.1.windows.2
```
安装这些工具后，您就可以开始挑战 O3DE 了。

## 获取 O3DE
O3DE 有两种不同的方式。一个源是直接独立安装程序，另一个源是 O3DE GitHub 存储库。我将从使用安装程序开始。您可以从此处下载安装程序：

https://www.o3de.org/download/#windows

{{< note >}}
我在第 28 章 Wwise for O3DE 和第 31 章 设置多人游戏中介绍了从 O3DE 的 GitHub 存储库构建自己的 O3DE 安装的步骤。
{{< /note >}}
## 运行安装程序
{{< note >}}
此处的说明适用于 PC Windows 平台。
{{< /note >}}
下载安装程序并启动后，您将看到它的启动窗口。

![](/images/learning-guide/tutorials/o3de-book/Part1/o3de_book_1_1.PNG)

O3DE 独立安装程序

您可以通过查看 Options 找到引擎的安装位置。

![](/images/learning-guide/tutorials/o3de-book/Part1/o3de_book_1_2.PNG)

默认安装位置

{{< note >}}
在启动安装程序之前，您必须先安装 CMake，因为安装程序需要执行许多使用 CMake 的初始步骤，例如下载各种依赖项。
{{< /note >}}

安装完成后，确认安装位置包含引擎。默认情况下，位置将为 `C:\O3DE\21.11.2`。在本书的其余部分，我将把这条路径称为引擎路径。如果您选择将其安装在其他位置，请相应地调整说明。

如果引擎安装成功，您将能够通过单击“启动”按钮或直接从`C:\O3DE\21.11.2\bin\Windows\profile\Default\o3de.exe`从安装程序启动 O3DE Project Manager。

下一步是创建我们的第一个 O3DE 项目。
