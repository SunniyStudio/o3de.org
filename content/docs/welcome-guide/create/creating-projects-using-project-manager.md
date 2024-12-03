---
linktitle: 使用 Project Manager 创建项目
title: 使用 O3DE Project Manager 创建项目
description: 了解如何使用 Project Manager 工具从默认项目模板创建和构建 Open 3D Engine （O3DE） 项目。
weight: 100
toc: true
---

本教程介绍了在 Open 3D Engine （O3DE） 中进行项目配置和构建。此处和视频中的说明将指导您完成以下步骤：

* 配置 **Project Manager** 引擎设置。
* 创建新的 O3DE 项目。
* 构建 O3DE 项目。

在本教程结束时，您将拥有一个基于默认项目模板的新 O3DE 项目，可以在 **O3DE 编辑器 ** 中打开。

{{< youtube-width id="_BhkXOBDYGA?start=142" title="Installing O3DE for Windows - Create a Project" >}}

## 先决条件

以下说明假定您已：

* O3DE 作为 [SDK 引擎](/docs/user-guide/appendix/glossary#sdk-engine) 安装或构建在您的计算机上。有关帮助，请参阅 [设置 Open 3D Engine](/docs/welcome-guide/setup)。
* 满足 [O3DE 系统要求](/docs/welcome-guide/requirements)中列出的所有硬件和软件要求。

{{< note >}}
如果您从 GitHub 设置 O3DE 并选择了 [源引擎](/docs/user-guide/appendix/glossary#source-engine) 构建类型，则需要从命令行创建一个项目并构建它，然后才能拥有适用于您的新项目的 **Project Manager** 版本。按照 [使用 CLI 创建项目](../creating-projects-using-cli) 为您的源引擎创建项目。
{{< /note >}}

## 启动 Project Manager

1. 从桌面上的图标启动 Project Manager，或者在计算机上打开文件浏览器或命令行窗口并导航到 O3DE 引擎目录。从`<install-directory>/bin/<platform>/profile/Default`找到并启动 O3DE **Project Manager** 应用程序`o3de.exe`。

    {{< important >}}
如果您使用`INSTALL`目标从源代码构建了 SDK 引擎，请确保从 **install** 目录（而不是引擎根目录中的 build 目录）启动 Project Manager 和其他工具。例如，Windows 安装目录通常以 `/bin/Windows/profile/Default` 结尾。
    {{< /important >}}

## 配置引擎设置

如果这是您第一次使用 Project Manager 创建项目，请在创建和构建项目之前检查并配置 O3DE 引擎设置。

1. 选择 Project Manager 顶部附近的 **Engine**（引擎）选项卡。

1. 查看默认位置并根据需要进行更新，同时考虑到 **第三方软件文件夹** 和位于 **默认项目文件夹** 中的典型项目都需要 10 - 20 GB 的可用计算机空间。默认情况下，O3DE 在所有位置使用您的用户目录。

  如果您从 GitHub 设置和构建 O3DE，则可以将 **第三方软件文件夹** 设置为您在构建引擎时创建的相同包目录（例如，`C:\o3de-packages`），以避免在项目构建时再次下载相同的包。
    ![Default Projects Folder and 3rd Party Software Folder updated in engine settings](/images/welcome-guide/project-manager-engine-settings-adjusted.png)

1. 更新完引擎设置后，返回到 **项目**（**Projects**）选项卡。

## 创建新的 O3DE 项目

1. 在 Project Manager 中，选择 **Create a Project**。这将指导您从默认项目模板开始创建新项目。它还将注册 O3DE 引擎（如果尚未注册）。

    ![Choose "Create a Project"](/images/welcome-guide/project-manager-no-projects.png)

    如果您已经注册了一个或多个项目，请打开 **New Project** 下拉菜单，然后选择 **Create New Project**。

    ![Or choose "New Project - Create New Project"](/images/welcome-guide/project-manager-menu-create-new-project.png)

1. 在 **Project name** （项目名称） 下，为您的项目命名。您最多可以使用 64 个字母、数字、下划线('_')或连字符 （'-'）。不允许使用空格。

1. 如果要更改项目位置，请在 **Project Location** 下选择文件夹图标，然后使用 **Browse** 对话框选择新位置。如果需要，您可以创建一个新的项目文件夹。您选择的文件夹将成为项目根目录。

    ![Create a New Project - Project Details screen](/images/welcome-guide/project-manager-create-project.png)

1. 在 **Select a Project Template**（选择项目模板）下，您可以选择具有预配置 Gem 选择的项目模板。您可以通过选择 **Configure Gems** （配置 Gems） 按钮来修改为项目启用的 Gem 列表。本教程使用默认模板及其预配置的常见 Gem 选择。  其他模板可用。 有关模板以及可用模板的更多信息，请参阅用户指南中有关 [模板](/content/docs/user-guide/build/templates.md) 的文档。

1. 选择 **Create Project** （创建项目） 以在您选择的项目位置创建项目文件。这还会在位于`<user-folder>/.o3de/o3de_manifest.json`的 O3DE 清单中注册您的项目。

## 构建 O3DE 项目

现在，您可以从 Project Manager 构建项目。

1. 在项目的图标框中，打开 **Build Project**下拉菜单，然后选择 **Build Now**。

    ![Choose Build Project](/images/welcome-guide/project-manager-build-project.png)

1. 在下一个对话框中选择 **Yes** 以确认您已准备好构建项目后，构建将开始。

    {{< note >}}
如果需要下载所需的第三方软件包，则第一个版本可能需要一些时间才能完成。
    {{< /note >}}

    构建完成后，您可以在项目目录的`build/<platform>/bin/profile`下找到项目二进制文件。
    
    {{< note >}}
在 Windows 上，如果安装了多个版本的 Visual Studio，则 Project Manager 将使用检测到的最高版本进行构建。要指定 Visual Studio 的版本，请使用 CMake 环境变量 `CMAKE_GENERATOR_PLATFORM`和 CMake 生成器列表中的 [值](https://cmake.org/cmake/help/latest/manual/cmake-generators.7.html#visual-studio-generators)。
    {{< /note >}}

    {{< note >}}
在 Windows 上，如果安装了多个版本的 Visual Studio，则 Project Manager 将使用检测到的最高版本进行构建。要指定 Visual Studio 的版本，请使用 CMake 环境变量 `CMAKE_GENERATOR_PLATFORM`和 CMake 生成器列表中的 [值](https://cmake.org/cmake/help/latest/manual/cmake-generators.7.html#visual-studio-generators)。
    {{< /note >}}

1. 要在 Editor 中打开构建的项目，请将指针移动到项目的图标框内，然后选择 **Open Editor**。

有关项目配置和构建的更多信息，请参阅用户指南的 [项目配置](/docs/user-guide/project-config) 和 [构建](/docs/user-guide/build) 部分。
