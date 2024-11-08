---
linkTitle: 适用于 Windows 的生成打包
title: 适用于 Windows 的 AWS GameLift Gem 生成包打包
description: 了解如何在 Open 3D Engine （O3DE） 中使用 AWS GameLift Gem 打包 Windows 专用服务器版本。
toc: true
weight: 700
---

本主题介绍如何打包 Windows 专用服务器版本，这是在 Amazon GameLift 上安装和运行它们所必需的。

创建专用服务器包包括以下步骤：
1. 准备一个安装文件夹，并复制资产、运行时二进制文件、级别、设置文件和可再发行组件。
2. 创建安装脚本以处理将游戏版本完全安装到 GameLift 托管服务器上所需的任务。
3. 在本地计算机上测试专用服务器软件包。

{{< note >}}  
不支持调试专用服务器，因为它们需要无法重新分发的调试 Visual Studio 可再发行组件。有关更多详细信息，请参阅 [Visual Studio 2019 的可分发代码](https://docs.microsoft.com/en-us/visualstudio/releases/2019/redistribution) 。
{{< /note >}}

## 先决条件
以下说明假定以下内容：
- 您已经在启用 AWS GameLift Gem 的情况下构建了项目。有关构建项目的更多信息，请参阅 [构建 O3DE 项目](/docs/welcome-guide/create/creating-projects-using-cli/#build-the-o3de-project). 
- 您已经构建了项目的 Server Launcher 的配置文件或发布版本。
- 您已运行 **Asset Processor** 并编译了项目的所有资产。

## 准备安装文件夹

您必须创建一个单独的安装文件夹，以复制所需的资产、运行时二进制文件、注册表设置文件和可再发行组件。在此示例中，安装文件夹表示为“`<package base folder>`”。

### 配置文件构建

1. 在安装文件夹中创建所需的子文件夹。启动器会查找固定的子文件夹位置以查找资源和注册表文件，因此安装文件夹必须具有以下结构：

   - `<package base folder>/bin`
   - `<package base folder>/assets`
   - `<package base folder>/assets/pc`

2. 将以下文件复制到“`/bin`”文件夹：

    -   来自 `o3de/build/windows/bin/profile`的所有`*.exe` 和 `*.dll` 文件
    -   来自 `o3de/build/windows/bin/profile/Registry/`的`Registry`文件夹

3. 将项目的缓存文件夹 `<project folder>/Cache/pc` 中的所有文件（松散资源）复制到`<package base folder>/assets/pc`文件夹中。
   
4. [下载 `VC_redist.x64.exe` 文件](https://aka.ms/vs/17/release/vc_redist.x64.exe) 并将其复制到 Package base 文件夹。

5. 在包基文件夹中创建名为 '`install.bat`' 的构建安装脚本，并将以下内容添加到文件中。有关生成安装脚本的更多信息，请查看 [将自定义服务器生成包上传到 GameLift](https://docs.aws.amazon.com/gamelift/latest/developerguide/gamelift-build-cli-uploading.html)

    ```bash
    VC_redist.x64.exe /q
    ```

### 发布版本

1. 在安装文件夹中创建所需的子文件夹。启动器会查找固定的子文件夹位置以查找资源和注册表文件，因此安装文件夹必须具有以下结构：

   - `<package base folder>/bin`
   - `<package base folder>/assets`
   - `<package base folder>/assets/pc`

2. 将以下文件复制到“`/bin`”文件夹：

    -   在 `o3de/build/windows/bin/release`中的所有`*.exe` 和 `*.dll`文件
  
3. 将`<project folder>/Cache/pc` 下的所有资源以及 `o3de/build/windows/bin/release/Registry/` 中的`Registry`文件夹压缩到一个名为`engine.pak`的包中。然后将`engine.pak`添加到`<package base folder>/assets/pc`文件夹中。

4. 将`VC_redist.x64.exe`文件从`o3de/Tools/Redistributables/Visual Studio 2015-2019`复制到包基文件夹。

5. 在包基文件夹中创建名为 '`install.bat`' 的构建安装脚本，并将以下内容添加到文件中。有关生成安装脚本的更多信息，请查看 [将自定义服务器生成包上传到 GameLift](https://docs.aws.amazon.com/gamelift/latest/developerguide/gamelift-build-cli-uploading.html)

    ```bash
    VC_redist.x64.exe /q
    ```

## 测试服务器包

要测试本地服务器包：

1.  按照 Amazon GameLift 文档[测试您的集成](https://docs.aws.amazon.com/gamelift/latest/developerguide/integration-testing-local.html) 启动 GameLift Local。
2.  运行 CLI 命令以启动您的服务器：
    ```
    <package base folder>\bin\<server-launcher-executable> --engine-path=<package base folder> --project-path=<package base folder> --project-cache-path=<package base folder>\assets -bg_ConnectToAssetProcessor=0
    ```

3.  按照 Amazon GameLift [测试您的集成](https://docs.aws.amazon.com/gamelift/latest/developerguide/integration-testing-local.html) 文档启动客户端并在本地测试服务器行为。服务器日志可以在 `<package base folder>/<unique server process id>/user/log/Server.log`中找到。

{{< caution >}}  
确保将 `<package base folder>` 替换为您之前创建的安装文件夹的路径。
{{< /caution >}}
