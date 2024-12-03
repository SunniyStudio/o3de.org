---
linktitle: Installing O3DE for Windows
title: Installing O3DE for Windows
description: Learn how to install and set up Open 3D Engine (O3DE) for Windows using the installer tool.
weight: 100
---

要在 Windows 中快速开始使用 O3DE，请下载并运行安装程序。使用以下视频或书面说明指导您完成安装过程。成功安装后，您将拥有引擎及其 Gem 的稳定预构建版本，并且您将准备好使用 **Project Manager** 工具创建新项目或打开现有项目。

{{< youtube-width id="_BhkXOBDYGA" title="Installing O3DE for Windows" >}}

## 先决条件

以下说明假定您已：

* 满足 [O3DE 系统要求](../requirements)中列出的所有硬件和软件要求。
* 已按照 [Microsoft Windows](../requirements/#microsoft-windows)） 部分。

## 安装 O3DE

1. 从 [O3DE 下载](https://o3de.org/download/#windows) 获取最新版本的 Windows 安装程序。

1. 从下载位置运行安装程序。

{{< note >}} 要创建脱机安装程序，请使用以下命令：**`<installer exe> /layout`**。这会将便携式 MSI 和 CAB 文件下载到安装程序的相对路径。运行 MSI 以启动脱机安装。 {{< /note >}}

1. 在开发人员预览版期间，您可能会遇到来自 Microsoft Defender 的以下警告，将该程序描述为无法识别的应用程序。要继续安装，请选择 **更多信息**，然后选择 **仍然运行**。

    ![Microsoft Defender Windows protection dialog boxes](/images/welcome-guide/installer-defender-protection.png)

1. 默认安装位置为：`C:\O3DE\<o3de_version>`。要在此处安装 O3DE，请选择 **Install**。要更改安装位置，请选择 **Options**。

    ![O3DE welcome and options](/images/welcome-guide/installer-welcome.png)

1. 在安装过程中，将下载其他文件，并且窗口可能会打开和关闭 - 例如，在引擎的`python`目录中安装 Python 运行时时。

    ![O3DE install progress](/images/welcome-guide/installer-install-progress.png)

1. 安装成功后，安装程序会显示消息 **安装成功完成**。要打开 Project Manager，请选择 **Launch**。要退出安装程序，请选择 **Close**。

    ![O3DE install successful](/images/welcome-guide/installer-completed-success.png)

    安装程序在桌面上为常见的 O3DE 应用程序创建两个快捷方式：

    ![O3DE Editor icon](/images/welcome-guide/desktop-icon-editor.png) **O3DE Editor**

    Editor 是 O3DE 创意工具的中心枢纽。为了帮助您实现项目目标，请使用 Editor 放置和分组实体、添加组件、配置属性以及打开支持工具，例如 **Animation Editor** 和 **Script Canvas**。要了解有关 Editor 的更多信息，请观看 [Editor 导览](/docs/welcome-guide/tours/editor-tour)。

    ![O3DE Project Manager icon](/images/welcome-guide/desktop-icon-project-manager.png) **O3DE Project Manager**

    使用 Project Manager 工具创建和自定义项目。要为您的项目添加或删除功能，您还可以启用或禁用 Gem。

    有关项目管理器的介绍和创建第一个项目的帮助，请继续阅读 [使用 Open 3D Engine 项目管理器创建项目](/docs/welcome-guide/create/creating-projects-using-project-manager) 以学习入门教程。
