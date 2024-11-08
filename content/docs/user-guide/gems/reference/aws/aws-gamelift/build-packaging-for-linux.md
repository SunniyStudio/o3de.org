---
linkTitle: 适用于 Linux 的构建打包
title: 适用于 Linux 的 AWS GameLift Gem 生成包打包
description: 了解如何在 Open 3D Engine （O3DE） 中使用 AWS GameLift Gem 打包 Linux 专用服务器版本。
toc: true
weight: 800
---

本主题介绍如何打包 Linux 专用服务器版本，这是在 Amazon GameLift 上安装和运行它们所必需的。

创建专用服务器包包括以下步骤：
1. 准备一个安装文件夹，并复制资源、运行时二进制文件、关卡、设置文件和其他第三方库。
2. 创建安装脚本以处理将游戏版本完全安装到 GameLift 托管服务器上所需的任务。
3. 在本地计算机上测试专用服务器软件包。

{{< note >}}  
目前不支持使用 Release 配置的 Linux 专用服务器版本。请确保使用 Profile 配置构建您的专用服务器。
{{< /note >}}

## 先决条件
以下说明假定以下内容：
- 您已经在启用 AWS GameLift Gem 的情况下构建了项目。有关构建项目的更多信息，请参阅 [构建 O3DE 项目](/docs/welcome-guide/create/creating-projects-using-cli/#build-the-o3de-project). 
- 您已经构建了项目的服务器启动器的 Profile 版本。
- 您已运行 **Asset Processor** 并编译了项目的所有资产。

## 准备安装文件夹

您必须创建一个单独的安装文件夹，以复制所需的资产、配置文件运行时二进制文件、注册表设置文件和可再发行组件。在此示例中，安装文件夹表示为“`<package base folder>`”。

1. 在安装文件夹中创建所需的子文件夹。启动器会查找固定的子文件夹位置以查找资源和注册表文件，因此安装文件夹必须具有以下结构：

   - `<package base folder>/assets`
   - `<package base folder>/assets/linux`
   - `<package base folder>/lib64`

2. 将以下文件复制到包基文件夹：

   - 来自 '`o3de/build/linux/bin/profile`' 的所有可执行文件和 '`*.so`' 文件。
   - 来自 '`o3de/build/linux/bin/profile/Registry`' 的 '`Registry`' 文件夹。
  
3. 复制项目缓存文件夹中的所有文件（松散资源），从犯 `<project folder>/Cache/linux`到`<package base folder>/assets/linux` 文件夹。

4.  从 `/usr/lib/x86_64-linux-gnu`/ 复制以下第三方库到 `<package base folder>/lib64/`:

    -   libc++*
    -   libxkb*
    -   libxcb*
    -   libX*
    -   libbsd*
    -   libstdc++*

    要复制库文件，请使用以下 CLI 命令：

    ```cmd
    cp -a /usr/lib/x86_64-linux-gnu/<library name>* <package base folder>/lib64/.
    ```

5. 在软件包 base 文件夹中创建名为 'i`nstall.sh`' 的构建安装脚本，并将以下内容添加到文件中。有关生成安装脚本的更多信息，请查看 [将自定义服务器生成包上传到 GameLift](https://docs.aws.amazon.com/gamelift/latest/developerguide/gamelift-build-cli-uploading.html)

    ```bash
    #!/bin/sh

    sudo cp -a ./lib64/libX* /lib64/.
    sudo cp -a ./lib64/libbsd* /lib64/.

    if cat /etc/system-release | grep -qFe 'Amazon Linux release 2'
    then
        sudo yum update -y
        sudo yum groupinstall 'Development Tools' -y
        sudo yum install python3 -y

        echo 'Update outdated make package'
        mkdir make && cd make
        wget http://ftp.gnu.org/gnu/make/make-4.2.1.tar.gz
        tar zxvf make-4.2.1.tar.gz
        mkdir make-4.2.1-build make-4.2.1-install
        cd make-4.2.1-build
        /local/game/make/make-4.2.1/configure --prefix='/local/game/make/gmake-4.2.1-install'
        make -j 8
        make -j 8 install
        export PATH=/local/game/make/gmake-4.2.1-install/bin:$PATH
        sudo ln -sf /local/game/make/gmake-4.2.1-install/bin/make /local/game/make/gmake-4.2.1-install/bin/gmake
        cd /local/game/

        echo 'Installing missing libs for AL2'
        mkdir glibc && cd glibc
        wget http://mirror.rit.edu/gnu/libc/glibc-2.29.tar.gz
        tar zxvf glibc-2.29.tar.gz
        mkdir glibc-2.29-build glibc-2.29-install
        cd glibc-2.29-build
        /local/game/glibc/glibc-2.29/configure --prefix='/local/game/glibc/glibc-2.29-install'
        make -j 8
        make -j 8 install
        sudo ln -sf /local/game/glibc/glibc-2.29-install/lib/libm.so.6 /local/game/lib64/libm.so.6
        cd /local/game/
    fi
        
    echo 'Install Success'
    ```

## 测试服务器包

要测试本地服务器包：

1.  按照 Amazon GameLift 文档[测试您的集成](https://docs.aws.amazon.com/gamelift/latest/developerguide/integration-testing-local.html) 启动 GameLift Local。 
2.  运行 CLI 命令以启动您的服务器：
    ```
    <package base folder>/<server-launcher-executable> --engine-path=<package base folder> --project-path=<package base folder> --project-cache-path=<package base folder>/assets -bg_ConnectToAssetProcessor=0 -rhi=null
    ```

3.  按照 Amazon GameLift [测试您的集成](https://docs.aws.amazon.com/gamelift/latest/developerguide/integration-testing-local.html)文档启动客户端并在本地测试服务器行为。服务器日志可以在`<package base folder>/<unique server process id>/user/log/Server.log`.

{{< caution >}}  
确保将 '`<package base folder>` 替换为您之前创建的安装文件夹的路径。 
{{< /caution >}}

{{< note >}}  
如果您使用 virtualbox 构建 linux 服务器，您将无法测试主机 PC 和 virtualbox 之间的网络连接。相反，在启动服务器后，您可以使用 AWS cli 在服务器端测试 Gamelift 会话。（详见 [测试游戏服务器](https://docs.aws.amazon.com/gamelift/latest/developerguide/integration-testing-local.html#integration-testing-local-server))
{{< /note >}}
