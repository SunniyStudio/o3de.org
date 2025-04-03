---
linktitle: Project Manager
title: Project Manager
description: 基于 GUI 的 Project Manager 工具的参考，使您能够在 Open 3D Engine （O3DE） 中创建、构建和配置项目。
weight: 100
---

**Project Manager** 是一个独立的应用程序，可帮助您创建、构建和配置 **Open 3D Engine （O3DE）** 项目。它为 CMake 构建系统提供了一个基于 GUI 的前端，并替代了 [O3DE CLI 中的许多命令](../cli-reference).

使用 Project Manager，您可以执行以下操作：

* 创建项目或添加现有项目。
* 构建一个项目。
* 在 O3DE 编辑器中打开一个项目。
* 添加 [Gem](/docs/user-guide/appendix/glossary#gem) 存储库。
* 使用 **Gem Catalog** 在项目中添加或删除 Gem。
* 配置项目图标。
* 配置 O3DE 清单设置，例如计算机上的项目、可下载（“第三方”）包和其他 O3DE 对象的默认位置。

## 启动 Project Manager

要启动 Project Manager，请执行下列操作之一：

* 打开安装过程中放置在桌面上的 **Project Manager** 快捷方式。
* 从插件或项目构建目录启动`o3de.exe`

`o3de.exe`的位置取决于引擎二进制文件的构建方式：

{{< tabs name="o3de.exe location" >}}
{{% tab name="Source engine" %}}

在构建源引擎时，您需要指定一个构建目录，例如 `build/windows` 或 `build/linux`。二进制文件位于 build 目录的子目录中。此子目录的路径名取决于您选择的构建配置。例：`bin/profile`.

要启动 Project Manager，请执行以下操作：

1. 在计算机上打开文件浏览器或命令行窗口，然后导航到包含构建目录的 O3DE 引擎或项目目录。

1. 找到并启动 Project Manager 应用程序， `o3de.exe`, 位于 `<BUILD_DIRECTORY>/bin/<BUILD_CONFIGURATION>`.

例如:

```cmd
build\windows\bin\profile\o3de.exe
```

{{% /tab %}}
{{% tab name="Installed or pre-built SDK engine" %}}

要启动 Project Manager，请执行以下操作：

1. 在您的计算机上打开文件浏览器或命令行窗口，然后导航到您的 O3DE 安装目录。

1. 找到并启动 Project Manager 应用程序， `o3de.exe`, 位于 `<INSTALL_DIRECTORY>/bin/<PLATFORM>/<BUILD_CONFIGURATION>/Default`.

Example:

```cmd
bin\Windows\profile\Default\o3de.exe
```

{{< important >}}
如果您使用 `INSTALL`目标从源代码构建引擎，请确保从已安装引擎的构建目录启动 Project Manager，而不是引擎的构建目录。这一点很重要，因为 Project Manager 用于各种操作（例如创建新项目）的引擎是根据 `o3de.exe` 应用程序的位置确定的。
{{< /important >}}

{{% /tab %}}
{{< /tabs >}}

## Project Manager 参考

Project Manager 应用程序包含以下屏幕：

* Projects
* Engine
* Project details
* Project settings
* Configure gems

### Projects

**Projects** 选项卡是 Project Manager 的主屏幕。在这里，您可以找到所有已注册的 O3DE 项目。

![Projects tab with legend](/images/user-guide/project-config/project-manager/projects-tab-legend.png)

在您的计算机上注册一个或多个项目后，窗口右上角将出现 **New Project...** 菜单 **（1）**，其中包含以下选项：

| 操作                       | 描述                                                                                             |
|--------------------------|------------------------------------------------------------------------------------------------|
| **Create New Project**   | 启动新项目工作流。                                                             |
| **Add Existing Project** | 打开 **Select Project Directory** 浏览对话框，您可以从中将现有 O3DE 项目添加到 Project Manager。这也会在 O3DE 清单中注册项目。 |

每个项目都由其项目图标 **（2）** 和项目显示名称 **（3）** 表示。与项目当前状态相关的消息和按钮显示在项目图标矩形内。每个项目的图标下方是项目上下文下拉菜单 **（4）**，其中包含可对该项目执行的操作。

{{< note >}}
将鼠标悬停在项目图标下方的项目名称上，以工具提示的形式显示项目的绝对路径。
{{< /note >}}

项目上下文菜单包含以下操作：

| 操作                       | 描述                                                                                             |
| - | - |
| **Edit Project Settings** | 打开 project settings 屏幕，您可以从中更改选定的项目设置并配置为项目启用的 Gem。|
| **Configure Gems** | 跳过项目设置屏幕，打开项目的 Configure Gems （配置 Gem） 页面。 |
| **Build** | 生成项目。 |
| **Open CMake GUI** | 将 CMake 程序的 GUI 界面打开到您的项目目录。|
| **Open Project folder** | 在计算机上的 File Explorer 窗口中打开项目文件夹。 |
| **Create Editor desktop shortcut** | 创建一个桌面快捷方式，该快捷方式将在 **O3DE 编辑器 ** 中打开您的项目，跳过 Project Manager。 |
| **Duplicate** | 在您选择的目录中创建项目的副本（不含 build 文件夹）。它还会在用户文件夹的 O3DE 清单中注册新项目。请注意，复制项目的项目显示名称将与原始项目相同。您可以在 Project Settings 中更新显示名称。|
| **Remove from O3DE** | 从 Project Manager 和 O3DE 清单中删除项目，但不从磁盘中删除项目。 |
| **Delete this Project** | 从 Project Manager 中删除项目，并从 O3DE 清单中删除项目_and从 disk_ 中删除项目。 |

### Engine

**Engine** 选项卡包含引擎清单和 O3DE 清单中的设置。默认文件夹位置可在此屏幕上进行编辑。

{{< note >}}
所有默认文件夹的默认位置都位于您的用户文件夹中。如果用户文件夹中的驱动器空间有限，请考虑更改其中一些默认文件夹位置，尤其是“第三方软件文件夹”，在构建您的第一个项目后，它将包含数 GB 的下载包。
{{< /note >}}

![Engine tab with default values](/images/welcome-guide/project-manager-engine-settings-adjusted.png)

Engine （引擎） 选项卡包含以下 O3DE 设置：

| 设置                                   | 说明                                                                                                      | 默认值                     |
|--------------------------------------|---------------------------------------------------------------------------------------------------------|-------------------------|
| **Engine Name**                      | （只读）启动 Project Manager 时，从与 Project Manager 关联的引擎的`engine.json` 清单中读取的 O3DE 引擎的名称。                      |                         |
| **Engine Version**                   | （只读）启动 Project Manager 时，从与 Project Manager 关联的引擎的`engine.json` 清单中读取 O3DE 引擎版本。                        |                         |
| **Engine Folder**                    | （只读）启动 Project Manager 时与 Project Manager 关联的 O3DE 引擎的位置。单击文件夹图标将在计算机上的 File Explorer 窗口中打开 engine 文件夹。 |                         |
| **3rd Party Software Folder**        | 定义 O3DE 及其组件使用的可下载包的位置。                                                                                 | `<user>/.o3de/3rdParty` |
| **Default Projects Folder**          | 定义项目的默认文件夹。除非在新项目工作流期间指定了不同的路径，否则将在此文件夹中创建新项目。                                                          | `<user>/O3DE/Projects`  |
| **Default Gems Folder**              | 定义 Gem 的默认文件夹。将在此文件夹中创建新 Gem，除非在创建 Gem 时指定了不同的路径。                                                       | `<user>/O3DE/Gems`      |
| **Default Project Templates Folder** | 定义项目模板的默认文件夹。除非在创建项目模板时指定了不同的路径，否则将在此文件夹中创建新的项目模板。                                                      | `<user>/O3DE/Templates` |

### Project details

**Enter Project Details** screen 是 “New Project” 工作流程的一部分。在此屏幕上，您可以在计算机上设置项目的名称和位置。您还可以选择项目模板，该模板定义在新项目中启用的初始 Gem 集。您可以使用 **Configure Gems** 按钮进一步优化初始 Gem 集。

![Create a New Project - Project Details screen](/images/welcome-guide/project-manager-create-project.png)

### Project settings

项目上下文菜单中的 **Edit Project Settings** 菜单操作将打开 **Edit Project Settings** 屏幕。在此屏幕上，您可以更改项目的显示名称并更新项目的图标。这些设置和其他信息存储在位于项目目录根目录的 `project.json` 文件中。您还可以使用 **Configure Gems** 按钮更改为项目启用的 Gem 集。

![Edit Project Settings screen](/images/user-guide/project-config/project-manager/project-settings.png)

Edit Project Settings 屏幕包含以下项目设置：

| 设置                   | 说明                                                                                                                                                                                                                                                                                 |
|----------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Project Name**     | 项目的显示名称。这是 Project Manager 中 Projects 选项卡上的项目图标下显示的名称。                                                                                                                                                      |
| **Project Location** | 项目在计算机上的位置。如果使用文件夹按钮选择其他文件夹，Project Manager 会将项目移动到新位置，并使用新路径更新 O3DE 清单。以这种方式移动项目后，需要重新构建项目。|
| **Project Preview**  | 项目图标图像的路径。文件名将始终为`preview.png`。如果使用文件夹按钮选择其他图像，Project Manager 会将图像复制到您的项目文件夹并将其重命名为`preview.png`。                                                |

### Configure Gems

**Configure Gems** 界面允许您更改项目中启用的 Gem 的集合。您可以在新项目工作流期间，从 **Enter Project Details** 屏幕或现有项目的 **Edit Project Settings** 屏幕打开此屏幕。您可以使用 **Configure Gems** 屏幕中的 Gem Catalog 为您的项目查找和启用新 Gem。 有关使用配置 Gem 的说明，请参阅 [在项目中添加和删除 Gem](./add-remove-gems) 。

![Configure Gems screen](/images/user-guide/project-config/add-remove-gems/configure-gems-screen.png)

## 使用 Project Manager 创建项目

本教程介绍了在 Open 3D Engine （O3DE） 中进行项目配置和构建。此处和视频中的说明将指导您完成以下步骤：

* 配置 **Project Manager** 引擎设置。
* 创建新的 O3DE 项目。
* 构建 O3DE 项目。

在本教程结束时，您将拥有一个基于默认项目模板的新 O3DE 项目，可以在 **O3DE 编辑器 ** 中打开。

{{< bilibili-width id="BV1MWPWeJE66" title="Installing O3DE for Windows" >}}

### 先决条件

以下说明假定您已：

* O3DE 作为 [SDK 引擎](/docs/user-guide/appendix/glossary#sdk-engine) 在您的计算机上安装或构建。有关帮助，请参阅 [设置 Open 3D 引擎](/docs/welcome-guide/setup).
* 满足 [O3DE 系统要求](/docs/welcome-guide/requirements) 中列出的所有硬件和软件要求.

{{< note >}}
如果您从 GitHub 设置 O3DE 并选择了 [源引擎](/docs/user-guide/appendix/glossary#source-engine) 构建类型，则需要从命令行创建一个项目并构建它，然后才能拥有适用于您的新项目的 **Project Manager** 版本。按照 [使用 CLI 创建项目](/docs/welcome-guide/create/creating-projects-using-cli)中的说明为您的源引擎创建项目。
{{< /note >}}

### 启动 Project Manager

1. 从桌面上的图标启动 Project Manager，或者在计算机上打开文件浏览器或命令行窗口并导航到 O3DE 引擎目录。找到并启动 O3DE **Project Manager** 应用程序， `o3de.exe`, 位于 `<install-directory>/bin/<platform>/profile/Default`.

    {{< important >}}
如果您使用`INSTALL`目标从源代码构建了 SDK 引擎，请确保从 **install** 目录（而不是引擎根目录中的 build 目录）启动 Project Manager 和其他工具。例如，Windows 安装目录通常以 `/bin/Windows/profile/Default` 结尾。
    {{< /important >}}

### 配置引擎设置

如果这是您第一次使用 Project Manager 创建项目，请在创建和构建项目之前检查并配置 O3DE 引擎设置。

1. 选择 Project Manager 顶部附近的 **Engine**（引擎）选项卡。

1. 查看默认位置并根据需要进行更新，同时考虑到 **第三方软件文件夹** 和位于 **默认项目文件夹** 中的典型项目都需要 10 - 20 GB 的可用计算机空间。默认情况下，O3DE 在所有位置使用您的用户目录。

    如果您从 GitHub 设置和构建 O3DE，则可以将 **第三方软件文件夹** 设置为您在构建引擎时创建的相同包目录（例如，`C:\o3de-packages`），以避免在项目构建时再次下载相同的包。

    ![Default Projects Folder and 3rd Party Software Folder updated in engine settings](/images/welcome-guide/project-manager-engine-settings-adjusted.png)

1. 更新完引擎设置后，返回到 **Projects** 选项卡。

### 创建新的 O3DE 项目

1. 在 Project Manager 中，选择 **Create a Project**。这将指导您从默认项目模板开始创建新项目。它还将注册 O3DE 引擎（如果尚未注册）。

    ![Choose "Create a Project"](/images/welcome-guide/project-manager-no-projects.png)

    如果您已经注册了一个或多个项目，请打开 **New Project** 下拉菜单，然后选择 **Create New Project**。

    ![Or choose "New Project - Create New Project"](/images/welcome-guide/project-manager-menu-create-new-project.png)

1. 在 **Project name** （项目名称） 下，为您的项目命名。您最多可以使用 64 个字母、数字、下划线('_')或连字符 （'-'）。不允许使用空格。

1. 如果要更改项目位置，请在 **Project Location** 下选择文件夹图标，然后使用 **Browse** 对话框选择新位置。如果需要，您可以创建一个新的项目文件夹。您选择的文件夹将成为项目根目录。

    ![Create a New Project - Project Details screen](/images/welcome-guide/project-manager-create-project.png)

1. 在 **Select a Project Template**（选择项目模板）下，您可以选择具有预配置 Gem 选择的项目模板。您可以通过选择 **Configure Gems** （配置 Gems） 按钮来修改为项目启用的 Gem 列表。本教程使用默认模板及其预配置的常见 Gem 选择。

1. 选择 **Create Project** （创建项目） 以在您选择的项目位置创建项目文件。这还会在 O3DE 清单中注册您的项目，该清单位于`<user-folder>/.o3de/o3de_manifest.json`.

### 构建 O3DE 项目

现在，您可以从 Project Manager 构建项目。

1. 在项目的图标框中，打开 **Build Project** 下拉菜单，然后选择 **Build Now**。

    ![Choose Build Project](/images/welcome-guide/project-manager-build-project.png)

1. 在下一个对话框中选择 **Yes** 以确认您已准备好构建项目后，构建将开始。

    {{< note >}}
如果需要下载所需的第三方软件包，则第一个版本可能需要一些时间才能完成。
    {{< /note >}}

    构建完成后，您可以在项目目录的`build/<platform>/bin/profile`下找到项目二进制文件。
    
    {{< note >}}
在 Windows 上，如果安装了多个版本的 Visual Studio，则 Project Manager 将使用检测到的最高版本进行构建。要指定 Visual Studio 的版本，请使用 CMake 环境变量 `CMAKE_GENERATOR_PLATFORM` 和 [CMake 生成器列表中的值](https://cmake.org/cmake/help/latest/manual/cmake-generators.7.html#visual-studio-generators).
    {{< /note >}}

1. 要在 Editor 中打开构建的项目，请将指针移动到项目的图标框内，然后选择 **Open Editor**。

有关项目配置和构建的更多信息，请参阅用户指南的 [项目配置](/docs/user-guide/project-config) 和 [构建](/docs/user-guide/build) 部分。
