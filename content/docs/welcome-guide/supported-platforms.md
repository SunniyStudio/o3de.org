---
linktitle: 支持的平台
title: Open 3D Engine 中支持的平台
description: 获取 Open 3D Engine （O3DE） 支持的主机平台和目标平台的列表。
weight: 250
toc: true
---

**Open 3D Engine (O3DE)** 支持用于创建和开发游戏和模拟的多个主机平台，以及这些项目可以运行的目标平台。

### 主机平台

|主机平台 |特定于平台的文档 |
| --- | --- |
| Microsoft Windows | <ul><li>[Windows 先决条件和配置](requirements/#microsoft-windows)</li><li>[为Windows安装O3DE](setup/installing-windows)</li><li>[在 Windows 上从 CLI 创建项目](create/creating-projects-using-cli/creating-windows/)<sup>1</sup></li><li>[在 Windows 上从源构建 O3DE](setup/setup-from-github/building-windows/)</li></ul> |
| Linux | <ul><li>[Linux 先决条件和配置](requirements/#linux)</li><li>[安装 O3DE for Linux](setup/installing-linux)</li><li>[在 Linux 上通过 CLI 创建项目](create/creating-projects-using-cli/creating-linux/)<sup>1</sup></li><li>[在 Linux 上从源构建 O3DE](setup/setup-from-github/building-linux/)</li></ul> |
| macOS | 对在 macOS 上开发的支持处于试验阶段。请参阅系统要求页面的 [macOS 部分](requirements/#macos)，了解开始使用所需的基本要求。 在 #sig-platform 的 [Discord](https://{{< links/o3de-discord >}}) 上询问最新的设置说明和其他帮助。 |

<sup>1</sup> P有关使用 **Project Manager** 创建项目的独立于 latform 的文档，请参见 [此处](create/creating-projects-using-project-manager)。

有关在主机平台上进行开发的最低硬件要求，请参阅 [系统要求](requirements)。

### 目标平台

|Target 平台 |特定于平台的文档 |
| --- | --- |
| Microsoft Windows | <ul><li>[为 Windows 创建发布布局](/docs/user-guide/packaging/windows-release-builds/)</li></ul> |
| Linux | <ul><li>[Linux 的平台开发支持](/docs/user-guide/platforms/linux/)</li></ul> |
| Android | <ul><li>[Android 的平台开发支持](/docs/user-guide/platforms/android/)</li></ul> |
| macOS | 对 macOS 作为目标平台的支持处于试验阶段。在 #sig-platform 的 [Discord](https://{{< links/o3de-discord >}}) 上询问以获得开发支持。 |
| iOS | 对 iOS 作为目标平台的支持正处于试验阶段。在 #sig-platform 的 [Discord](https://{{< links/o3de-discord >}})上询问以获得开发支持。 |

{{< tip >}}
有关平台功能状态的信息，请在已安装的 O3DE 版本的 *功能网格* 快照中搜索该功能。功能网格快照可在发行说明中找到。（例如：[23.05.0 特征网格](/docs/release-notes/))
{{< /tip >}}
