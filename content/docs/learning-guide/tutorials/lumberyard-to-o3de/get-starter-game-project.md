---
linkTitle: 获取 Starter Game 项目
title: 获取 Starter Game 项目
description: 了解如何从 github 安装 Lumberyard 并使用它打开一个项目。
weight: 100
toc: true
---

本教程将介绍如何下载和运行 **Amazon Lumberyard Engine** 及其 Starter Game 项目。本教程的其他部分介绍如何从源构建 Lumberyard 和使用其他项目。

|O3DE 体验 |完成时间 |功能聚焦 |最后更新 |
|- |- |- |- |
|初级 |15 分钟 |安装 Lumberyard 并打开一个项目| February 27, 2024 |

{{< note >}}
由于 Lumberyard 只能在 Windows 上运行，因此只有 Windows 用户才能遵循本教程
{{< /note >}}

## 克隆 Lumberyard github 存储库

您可以下载 [Lumberyard 存储库](https://github.com/aws/lumberyard) 或将其克隆到本地计算机上。克隆允许您跟踪所做的更改。

- 要下载，请单击绿色的 **Code** 按钮，然后选择 **Download ZIP**。然后将其复制并解压缩到您想要的位置。
- 克隆
  1. 在您希望此存储库所在的目录中打开一个终端或 shell。
  2. 运行命令`git clone https://github.com/aws/lumberyard.git`.

输入文件夹并单击`git_bootstrap.exe`。它将下载引擎和第三方库的预构建二进制文件（下载大约 10GB，解压缩到磁盘上 25GB）。

该过程结束后，它将打开 **Lumberyard Setup Assistant**，如果您不想从源代码构建引擎，可以立即关闭它。

## 打开 StarterGame 项目

现在，您已经拥有了引擎二进制文件，您将需要选择 **default project**。只需转到 `LUMBERYARD_FOLDER\dev\Bin64vc142`并运行`ProjectConfigurator.exe`。确保 StarterGame 项目设置为默认项目（图标应灰显）。如果没有，只需单击名称旁边的图标，然后单击右上角的“**设置为默认值**”按钮。

![Project Configurator](/images/learning-guide/tutorials/lumberyard-to-o3de/project-configurator.png)

在同一文件夹中，您可以运行 `Editor.exe`。Asset Processor 将运行，编辑器将在几秒钟后打开。在 Welcome 弹出窗口中点击 *"Open level..."* 然后选择 *"Levels\Game\SinglePlayer"*。

您可以通过按 **Ctrl+G** 直接玩游戏，或者如果您转到顶部菜单 **Game\Play Game****。

![Play](/images/learning-guide/tutorials/lumberyard-to-o3de/play-game.png)

{{< note >}}
当您退出 Lumberyard 时，Lumberyard 资产处理器不会自动关闭。如果您打算在之后打开 O3DE，请不要忘记通过右键单击通知栏来关闭它，因为它会阻止 O3DE 资产处理器打开。
{{< /note >}}

## （可选） 从源构建引擎

您需要通过 Lumberyard Setup Assistant 并遵循 *“Full Install”* 路径。您可以随时通过 Lumberyard 文件夹中的 'SetupAssistant.bat' 打开此助手。您将需要正确安装 Visual Studio 2019 及其“使用 C++ 进行桌面开发”组件。

设置完成后，您可能希望生成或重新生成 Visual Studio 解决方案。只需从 **dev** 文件夹运行 `.\lmbr_waf.bat configure --enabled-game-projects=SamplesProject,StarterGame`  即可。然后使用 Visual Studio 2019 打开`dev\Solutions\LumberyardSDK_vs2019.sln`。

要运行编辑器，请在右键单击时将 **Sandbox\Editor** 设置为启动项目。

如果遇到构建错误，请检查`LocalFileIO.cpp`并更新`SetAlias`方法，如下所示

```cpp
void LocalFileIO::SetAlias(const char* key, const char* path)
{
    ClearAlias(key);
    char fullPath[AZ_MAX_PATH_LEN];
    ConvertToAbsolutePath(path, fullPath, AZ_MAX_PATH_LEN);
    m_aliases.push_back(AliasType(key, fullPath));
}
```

## （可选）试用其他示例

Lumberyard 的其他项目示例可在线获取，也可在存储库中找到。你可以使用 **ProjectConfigurator** 在它们之间切换，它将自动检测 `dev`文件夹中的任何项目。

- [N.E.M.O project](https://www.youtube.com/watch?v=SNIQjZzif1k): 潜艇的第三人称游戏
- [Dynamic Vegetation](https://www.youtube.com/watch?v=wX7O9K66zbY): 展示植被和天气系统
- Multiplayer Sample: 一个简单的太空中自上而下的射击游戏
- Samples Project: 多个技术演示场景
