---
linkTitle: 第2章 创建新项目
title: 第2章 创建新项目
description: 第2章 创建新项目
---
# 第2章 创建新项目
## 介绍
{{< note >}}
本章的源代码和资源可以在 GitHub 上找到：
https://github.com/AMZN-Olex/O3DEBookCode2111/tree/ch02_new_project
{{< /note >}}

本章将介绍：
* 如何创建新项目。
* 构建项目。
* 什么是 Asset Processor？
* 在 O3DE 编辑器中打开一个关卡。

{{< note >}}
您可以在以下位置找到官方入门指南：
https://www.o3de.org/docs/welcome-guide/create/creating-projects-using-project-manager/
{{< /note >}}

## O3DE 命令行界面
尽管可以使用 O3DE Project Manager 创建新项目，但我将向您展示如何使用命令行工具来完成它，因为我相信它将来会为您提供更多功能。此 CLI 可在`C:\O3DE\21.11.2\scripts\o3de.bat`中找到。
```shell
PS C:\git\book> C:\O3DE\21.11.2\scripts\o3de.bat -h
usage: o3de.py [-h]
```

这是创建新项目的命令行：
```shell
o3de.bat create-project -pp C:\git\book\MyProject
```
这将在`C:\git\book\MyProject`处创建一个新项目。

{{< note >}}
参数 `-pp` 代表项目路径。该脚本会将项目名称推断为 MyProject。
{{< /note >}}

有更多选项和命令可供 o3de.bat但这对于我们来说已经足够了。

确认已创建 `C:\git\book\MyProject` 并包含各种项目文件。

{{< note >}}
我建议你立即将你的项目置于 Git 源代码控制之下。提醒一下，我在 `c:\git\book` 文件夹下创建项目。如果您正在阅读本书，则可以使用以下命令克隆随附的源代码：
```shell
mkdir c:\git\
cd c:\git
git clone https://github.com/AMZN-Olex/O3DEBookCode2111 book
cd book
git checkout origin/ch02_new_project
```
随着您阅读本书的进度，切换到相应的分支。
{{< /note >}}

## 链接项目和 O3DE
如果您从全新的 O3DE 安装开始，并使用上述步骤创建了一个新项目，则该项目将已配置为使用安装在`C:\O3DE\21.11.2`中的 O3DE 引擎。但是，如果您安装了 O3DE 的多个副本，或者您正在不同的 O3DE 版本之间切换，那么了解项目如何声明它们使用的引擎是很有价值的。

涉及两个重要文件。第一个是 `project.json` 在你的项目中找到的 `C:\git\book\MyProject\project.json` 中。第二个是` C:\Users\<user>\.o3de
\o3de_manifest.json`。

`project.json` 指定要与 `engine` 属性一起使用的 O3DE 引擎安装的名称。
```json
{
  "project_name": "MyProject",
  ...
  "engine": "o3de-sdk"
}
```

`o3de_manifest.json` 指定所有引擎安装及其相应的名称。
```json
"engines_path": {
  "o3de-sdk": "C:/O3DE/21.11.2"
}
```

如果您曾经移动引擎安装或希望尝试新引擎，则需要相应地更新这些条目。

{{< note >}}
引擎和项目的位置完全取决于您，只要上述两个文件一致即可。
{{< /note >}}

## 使用CMake和Visual Studio构建
现在我们有一个新项目，我们可以配置和构建它。第一步是创建一个构建文件夹。位置由您决定，但我建议保持简短，因为如果由于 Windows 上的构建文件夹路径较长而超出最大文件路径，则可能会出现构建错误。

我为本书的其余部分选择了 C:\git\book\build。进入文件夹后，执行以下 CMake 指令来配置项目：
```
C:\git\book\build> cmake -S C:\git\book\MyProject\ -B .
```

参数 -S 指定项目的源位置，而 -B 指定生成文件夹位置。build 文件夹被设置为当前文件夹，即 C:\git\book\build。

{{<tip>}}
我经常使用的另一个参数是 -DLY_UNITY_BUILD=OFF。这会关闭将多个源文件编译为一个源文件的 CMake unity 文件。虽然这可以加快您的第一次初始编译速度，但我发现在项目内部进行增量小更改时，禁用 Unity 构建会更快。
{{</tip>}}

此步骤可能需要一段时间，具体取决于您的 Internet 下载速度。许多第三方软件包将被下载并安装到 C:/Users/<user>/.o3de/3rdParty 中。

配置完成后，将在 C:\git\book\build\MyProject.sln 生成 Visual Studio 解决方案。从此时起，您可以从 Visual Studio 或命令行构建项目，例如：
```
PS C:\git\book\build> cmake --build . --config profile
```

项目二进制文件将放置在 C:\git\book\build\bin\profile 下。

## Asset Processor
在我们开始研究启动我们的游戏项目之前，我们需要了解在 O3DE 中设计和运行游戏所涉及的四个主要可执行文件：编辑器、Asset Processor、游戏和服务器启动器。
1. Editor.exe位于 O3DE 安装的二进制文件夹 C:\O3DE\21.11.2\bin\Windows\profile\Default\Editor.exe 中。它用于设计您的游戏和游戏资产。它还能够在 Editor 中运行游戏。它由 O3DE 安装提供。
2. AssetProcessor.exe 位于 O3DE 与 Editor.exe 的安装中。其目的是处理游戏资产并将其提供给 Editor 和游戏启动器。
3. MyProject.GameLauncher.exe 位于 Project 的 build 文件夹中。它是一个独立的启动器，可以在没有编辑器的情况下运行游戏。它由项目的 Visual Studio 解决方案编译。
4. MyProject.ServerLauncher.exe 是该项目的专用服务器启动器。
   每当您启动 Editor 时，Asset Processor 都会在后台启动（如果它尚未运行）。我建议先单独运行 Asset Processor，以确认项目设置良好并且所有资产都在处理中。
```
   C:\O3DE\21.11.2\bin\Windows\profile\Default\AssetProcessor.exe
```

在启动 Asset Processor 之前，您需要为 O3DE 配置默认项目。否则，您将看到以下错误。

![](/images/learning-guide/tutorials/o3de-book/Part1/o3de_book_1_3.PNG)

Asset Processor 在没有设置默认项目的情况下失败

Asset Processor 尝试读取默认项目，但未找到在 C:\Users\<user>\.o3de\o3de_manifest.json 中指定的项目。您应该使用 --project-path C:\git\book\MyProject 将默认项目传递给 Asset Processor，或者使用 O3DE 命令行界面设置默认项目：

```shell
C:\O3DE\21.11.2\scripts\o3de.bat set-global-project -pp C:\git\book\MyProject
```

{{<tip>}}
您可以通过查看 C:\Users\<user>\.o3de\Registry\bootstrap.setreg 来确认上述作是否成功。它应该project_path设置为 C:\git\book\MyProject。您可以直接修改此文件，也可以使用 o3de.bat。

示例 2.1. C:\Users\<user>\.o3de\Registry\bootstrap.setreg
```json
{
  "Amazon": {
    "AzCore": {
      "Bootstrap": {
        "project_path": "C:/git/book/MyProject"
      }
    }
  }
}
```
{{</tip>}}

使用此配置，可以从命令行启动 Asset Processor，而无需指定项目路径。

{{<note>}}
您的项目只能启动一个 Asset Processor。如果您尝试启动另一个实例，它将引发错误“Asset Processor 的另一个实例可能已在端口 45643 上运行”。如果另一个实例已在运行，则无需启动另一个实例。Editor 和游戏启动器将连接到 Asset Processor（如果已在运行）。
{{</note>}}

{{<tip>}}
Asset Processor 可能很狡猾。如果它是由编辑器或游戏启动器启动的，它将在后台启动，您必须在 Windows 系统托盘中找到它。但是，如果直接从命令行启动它，它将在前台打开。
{{</tip>}}

Asset Processor 处理完窗口左上角的所有资源后，它应显示以下内容：
* Status: Idle... - 否则，它仍在处理资产，您应该等到所有资产都完成。
* Project: C:/git/book/MyProject - 确认它正在为正确的项目运行。
* Root: C:/O3DE/21.11.2 - 确认 Asset Processor 正在使用预期的引擎。

#### 示例 2.2. Asset Processor 已完成对游戏资源的处理
Asset Processor 将处理后的游戏资源放在 C:\git\book\MyProject\Cache 下，以便在运行时使用。

![](/images/learning-guide/tutorials/o3de-book/Part1/o3de_book_1_4.PNG)


您可以使用以下命令启动 Editor：
```shell
C:\O3DE\21.11.2\bin\Windows\profile\Default\Editor.exe
```

或者，通过构建并运行解决方案文件夹中的 Editor 项目，从 Visual Studio O3DE_SDK解决方案中。

![](/images/learning-guide/tutorials/o3de-book/Part1/o3de_book_1_5.PNG)

{{<note>}}
请勿在使用 O3DE 时关闭 Asset Processor。许多 O3DE 工具（包括 Editor）都需要它。相反，请最小化 Asset Processor 并让它在 Windows 系统托盘中闲置。
{{</note>}}

{{<tip>}}
最常见的陷阱之一是由于 Asset Processor 锁定您的游戏二进制文件而出现构建错误。解决此问题的方法是在每次重新编译项目之前停止 Asset Processor。然而，这让人厌烦。当您关闭 Editor 时，Asset Processor 仍在后台运行。简化作的一种方法是告诉 Asset Processor 在 Editor 或最后一个游戏启动器退出时自行关闭。您可以通过将此开关添加到 C:\git\book\MyProject\game.cfg 来实现此目的。
```shell
ap_tether_lifetime=1
```
{{</tip>}}

## 创建关卡
我们创建的新项目没有关卡，但我们可以从现有的 O3DE 模板创建一个新关卡。编辑器启动后，您将看到 Welcome to O3DE 对话框。使用 Create new... 创建新关卡按钮。

![](/images/learning-guide/tutorials/o3de-book/Part1/o3de_book_1_6.PNG)

点击 'Create new...'

在这本书中，我将它称为 MyLevel。

## 小结

{{<note>}}
本章的源代码和资源可以在 GitHub 上找到：
https://github.com/AMZN-Olex/O3DEBookCode2111/tree/ch02_new_project
{{</note>}}

此时，您应该会在 Editor 中看到该关卡。

图 2.1.编辑器：新建关卡

![](/images/learning-guide/tutorials/o3de-book/Part1/o3de_book_1_7.PNG)

我们的下一个主题将是实体。您可以在 Entity Outliner 中看到它们。如果不可见，您可以从 Editor 的菜单（Tools）→ Entity Outliner 中调出它。

现在我们准备开始研究 O3DE 的最基本方面：实体及其组件。这将是第 3 章 实体和组件简介的重点。
