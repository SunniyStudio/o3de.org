---
linktitle: 安装 O3DE for Linux
title: 安装 O3DE for Linux
description: 了解如何安装和设置适用于 Linux 的 Open 3D Engine （O3DE）。
weight: 200
---

要在 Linux 中快速开始使用 O3DE，请下载并安装 deb 软件包。以下说明将指导您完成安装过程。成功安装后，您将拥有引擎及其 Gem 的稳定、预构建版本，并且您将准备好使用 **Project Manager** 工具创建新项目或打开现有项目。

## 先决条件

以下说明假定您已满足 [O3DE 系统要求](/docs/welcome-guide/requirements/#linux) 中列出的所有硬件和软件要求。

## 从 deb 包安装 O3DE

1. 从 [O3DE 下载](https://o3de.org/download/#linux) 页面获取最新版本的 deb 包。

1. 从下载位置运行安装程序。
    ```shell
    sudo apt install <path_to_deb_package>/<debian_package_name>.deb
    ```
    使用以下路径替换：
    * `<path_to_deb_package>`: 下载 deb 包的路径。例： `~/Downloads`.
    * `<debian_package_name>`: 包的名称。例：`o3de_2310_3.deb`.

    {{< known-issue link="https://bugs.launchpad.net/ubuntu/+source/synaptic/+bug/1522675" >}}
安装软件包时，您可能会看到以下错误：
```
N: Download is performed unsandboxed as root as file '/home/<user>/Downloads/o3de_2310_3.deb' 
   couldn't be accessed by user '_apt'. - pkgAcquire::Run (13: Permission denied)
```
这是 apt 中的一个 bug，已经被修复。一旦您当前的 Linux 发行版进行了修复，该错误应该会消失。
在分配进行修复之前，您可以通过使用以下命令修复文件夹权限来解决此问题：
```
sudo chown -Rv _apt:root /var/cache/apt/archives/partial/
sudo chmod -Rv 700 /var/cache/apt/archives/partial/
```
    {{< /known-issue >}}

    {{< known-issue link="https://salsa.debian.org/apt-team/apt/-/merge_requests/177" >}}
如果删除并安装软件包，您可能会看到如下警告：
```
W: Repository is broken: o3de:amd64 (= 24.09) has no Size information
```
这是 apt 中的一个 bug，已经被修复。一旦您当前的 Linux 发行版进行了修复，该警告应该会消失。该问题是警告，不会影响安装或删除。
    {{< /known-issue >}}

    O3DE will be installed in the default location: `/opt/O3DE/<version>`, where `<version>` is the version of the installer. Example: `2310_3`.

1. 在安装过程中，安装程序会根据需要下载其他软件包。该过程可能需要一些时间，具体取决于您的 Internet 连接速度和已安装的软件包。示例输出如下所示（版本和路径可能不匹配）：
    ```shell
    Reading package lists... Done
    Building dependency tree
    Reading state information... Done
    Note, selecting 'o3de' instead of '/usr/home/<user>/Downloads/o3de_2310_3.deb'
    The following packages were automatically installed and are no longer required:
    gir1.2-ibus-1.0 libasound2-dev libblkid-dev libdbus-1-dev libegl1-mesa-dev libgles2-mesa-dev libglib2.0-dev libglib2.0-dev-bin libibus-1.0-5 libibus-1.0-dev libice-dev libmount-dev libpcre16-3
    libpcre2-16-0 libpcre2-32-0 libpcre2-dev libpcre2-posix2 libpcre3-dev libpcre32-3 libpcrecpp0v5 libpulse-dev libpulse-mainloop-glib0 libsdl2-2.0-0 libsdl2-dev libselinux1-dev libsepol1-dev libsm-dev
    libsndio-dev libsndio7.0 libudev-dev libwayland-bin libwayland-cursor0 libwayland-dev libwayland-egl1 libxcursor-dev libxcursor1 libxfixes-dev libxi-dev libxinerama-dev libxrandr-dev libxrender-dev
    libxt-dev x11proto-input-dev x11proto-randr-dev x11proto-xinerama-dev
    Use 'sudo apt autoremove' to remove them.
    The following NEW packages will be installed:
    o3de
    0 upgraded, 1 newly installed, 0 to remove and 0 not upgraded.
    After this operation, 17.9 GB of additional disk space will be used.
    Get:1 /usr/home/<user>/Downloads/o3de_2310_3.deb o3de amd64 2310_3 [5064 MB]
    Selecting previously unselected package o3de.
    (Reading database ... 48992 files and directories currently installed.)
    Preparing to unpack .../Linux/DEB/o3de_2310_3.deb ...
    Unpacking o3de (2310_3) ...
    Unpacking o3de (2310_3) over (2310_1) ...
    Setting up o3de (2310_3) ...
    ```
    
安装完成后，您可以在`<install-directory>/bin/Linux/profile/Default`中找到 **Project Manager** 和其他工具。

从 shell 启动 Project Manager 的示例：
```shell
/opt/O3DE/24.09/bin/Linux/profile/Default/o3de
```

## 从 deb 包中删除已安装的 O3DE

1. 运行 apt 删除 o3de：
    ```shell
    sudo apt remove o3de
    ```
    将显示如下所示的输出：
    ```shell
    Reading package lists... Done
    Building dependency tree
    Reading state information... Done
    The following packages were automatically installed and are no longer required:
      gir1.2-ibus-1.0 libasound2-dev libblkid-dev libdbus-1-dev libegl1-mesa-dev libgles2-mesa-dev libglib2.0-dev libglib2.0-dev-bin libibus-1.0-5 libibus-1.0-dev libice-dev libmount-dev libpcre16-3
      libpcre2-16-0 libpcre2-32-0 libpcre2-dev libpcre2-posix2 libpcre3-dev libpcre32-3 libpcrecpp0v5 libpulse-dev libpulse-mainloop-glib0 libsdl2-2.0-0 libsdl2-dev libselinux1-dev libsepol1-dev libsm-dev
      libsndio-dev libsndio7.0 libudev-dev libwayland-bin libwayland-cursor0 libwayland-dev libwayland-egl1 libxcb-render0 libxcb-render0-dev libxcb-shape0-dev libxcb-xfixes0-dev libxcb-xinput-dev
      libxcb-xkb-dev libxcb-xkb1 libxcursor-dev libxcursor1 libxfixes-dev libxi-dev libxinerama-dev libxkbcommon-dev libxkbcommon-x11-0 libxkbcommon-x11-dev libxkbcommon0 libxrandr-dev libxrender-dev
      libxt-dev x11proto-input-dev x11proto-randr-dev x11proto-xinerama-dev
    Use 'sudo apt autoremove' to remove them.
    The following packages will be REMOVED:
      o3de
    0 upgraded, 0 newly installed, 1 to remove and 0 not upgraded.
    After this operation, 17.9 GB disk space will be freed.
    Do you want to continue? [Y/n] Y
    (Reading database ... 66411 files and directories currently installed.)
    Removing o3de (0.0.0.0) ...
    ```

## 从 Snap 包安装 O3DE

{{< note >}}
Snap 包是实验性的，可能会在某些发行版上遇到问题。这些说明已在 Ubuntu 22.04 LTS 上进行了测试。您需要安装 [Linux 要求](/docs/welcome-guide/requirements/#linux) 中列出的所有依赖项。
{{< /note >}}

1. 从 [O3DE 下载](https://o3de.org/download/#linux) 页面下载 Snap 软件包，或从 [Snap 商店](https://snapcraft.io/o3de) 获取说明。

1. 根据发行版的不同，您需要安装`snapd`才能安装 Snap 包。[请参阅本指南，了解特定于您的发行版的说明](https://snapcraft.io/docs/installing-snapd)。

1. 要从 Snap 存储区安装快照，请使用以下命令安装软件包（这会自动从存储区下载软件包）：
   ```shell
   snap install --classic o3de
   ```
   要安装最新的开发或 beta 频道，请使用以下命令：
   ```shell
   snap install --classic --<channel name> o3de
   ```
   如果您从 O3DE 下载页面下载了 Snap 软件包，请使用以下命令安装该软件包：
   ```shell
   snap install --classic --dangerous <o3de snap package filename>.snap
   ```
   如果成功，将显示以下输出：
   ```shell
   o3de <version> installed
   ```
   其中 `<version>` 是安装程序的版本。例：`24.09`.

O3DE 将安装在默认位置：`/snap/o3de/current/<version>`，其中 `<version>` 是安装程序的版本。示例：`24.09`。

安装完成后，您可以在`<install-directory>/bin/Linux/profile/Default`中找到 **Project Manager** 和其他工具。`snapd` 会将这个 `Default` 目录添加到你的 shell 环境路径中。

从 shell 启动 Project Manager 的示例：
```shell
o3de
```

{{< important >}}
从命令行使用 O3DE 预构建 **Snap** SDK 进行配置或构建时，首先导出`O3DE_SNAP`环境变量，以便 CMake 不会尝试安装 Python pip 要求并失败。要导出`O3DE_SNAP`环境变量，请在运行下面的 CMake 命令之前从命令行运行命令`export O3DE_SNAP`。
{{< /important >}}

## 从 Snap 包中删除安装的 O3DE

1. 运行 `snap` 移除O3DE:
   ```shell
   snap remove o3de
   ```
   如果成功，将显示以下输出：
   ```shell
   o3de removed
   ```
