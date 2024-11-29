---
linkTitle: 手动导出 O3DE Atom Sampleviewer for iOS
title: 手动导出 O3DE Atom Sampleviewer for iOS
description: （实验性）了解如何使用 Atom Sampleviewer 项目手动创建 Xcode 项目，以便针对 iOS 进行构建和部署。
toc: true
weight: 450
---
本指南介绍如何在 iOS 上手动构建和部署游戏。首先，我们将生成一个 Xcode 项目并准备资源。之后，我们将介绍如何确保 Xcode 已正确设置以构建 iOS 应用程序。

在本演示中，我们将使用 O3DE Atom Sample Viewer 项目作为参考点，以确保行为正确。其他项目的结果可能会有所不同。

{{< note >}}
要了解有关项目导出工具的更多信息，请参阅页面 [项目导出 CLI 工具。](/docs/user-guide/packaging/project-export/project-export-cli)
{{< /note >}}
{{< important >}}
iOS 导出功能仅在 macOS 上可用，目前在 O3DE 中处于试验阶段。
{{< /important >}}

## Prerequisites
1. Git clone [O3DE Atom Sample Viewer 项目](https://github.com/o3de/o3de-atom-sampleviewer)到你的电脑。
2. 使用O3DE注册 [O3DE Atom Sample Viewer 项目](https://github.com/o3de/o3de-atom-sampleviewer)。您可以使用以下方法注册项目：
```
<O3DE_ENGINE>/scripts/o3de.sh register -pp /path/to/o3de-atom-sampleviewer
```
3. 在您的计算机上安装了有效的 Xcode 副本，以及与该 IDE 关联的有效 Apple Developer ID。要设置此类 ID，请转至[developer.apple.com](https://developer.apple.com)。要使用您想要的 Apple ID，请在 Xcode 中转到 `Xcode -> Settings`。在“设置”窗口中，转到“帐户”选项卡，然后单击左侧面板底部的“+”图标以添加您的帐户。选择 Apple ID 继续，然后按照屏幕上的说明进行操作。
4. 您还需要确保 Xcode 安装了适用于 macOS 和 iOS 的标准 SDK，并且您首选的 iOS 测试设备与 Xcode 兼容。如果您不确定，可以查阅 [此页 安装 Simulator Runtimes](https://developer.apple.com/documentation/xcode/installing-additional-simulator-runtimes).
5. 确保为 iOS 启用了 o3de 引擎引导注册表。为此，请转到您的 O3DE 引擎安装并编辑`$O3DE_ENGINE_PATH/Registry/bootstrap.setreg`中的文件，以便 `assets` 字段使用 `ios`。这样：

```
"assets": "ios",
```
6. 编辑`$O3DE_ENGINE_PATH/Registry/AssetProcessorPlatformConfig.setreg`，注释掉第 67行:
```
//"ios": "enabled", -> "ios": "enabled",
```
这将指示 Asset Processor 确保在构建项目时缓存 iOS 资源。

## 导出流程
### 运行 export 命令
假设您只是使用 O3DE Atom Sampleviewer 项目，那么在项目的构建文件夹中生成必要的 Xcode 项目文件所需的全部命令应该是您所需要的：
```
export PROJECT_PATH=/path/to/project
cd $PROJECT_PATH

# Build the o3de tools if you haven't already, otherwise use tools you already have
cmake -B build/tools -G "Xcode"
cmake --build build/tools --target Editor AssetProcessor AssetBundler AssetProcessorBatch AssetBundlerBatch --config profile

# Run the asset processor
./build/tools/bin/profile/AssetProcessorBatch --platforms=ios --project-path $PROJECT_PATH

# Generate the Xcode project
cmake -B build/game-ios -G "Xcode" -DCMAKE_TOOLCHAIN_FILE=$O3DE_PATH/cmake/Platform/iOS/Toolchain_ios.cmake -DLY_MONOLITHIC_GAME=1 
```
{{< note >}}
确保将`$O3DE_ENGINE_PATH`变量设置为 O3DE 源位置的绝对路径，并将 `$PROJECT_PATH` 设置为项目文件夹位置的绝对路径。
{{< /note >}}

{{< note >}}
O3DE Atom Sampleviewer 项目不打算导出用于发布和 pak 配置。它是 Atom 渲染器功能的开发展示。因此，此项目不支持导出以供发布。要使用 Pak 文件的资产打包器，请使用另一个项目。
{{< /note >}}

假设底层 CMake 构建系统没有发现你的引擎或项目安装有问题，你应该有相应的 Xcode 项目文件，该文件将位于 `$PROJECT_PATH/build/game_ios` 。对于 Atom SampleViewer，它应该称为 `AtomSampleViewer.xcodeproj`。

### 使用 Xcode
双击文件 `AtomSampleViewer.xcodeproj` 以在 Xcode 中打开它。这应该会加载在 iOS 设备上构建应用程序所需的一切。

部署到 iOS 设备需要代码签名。确保您具有用于代码签名的 provisioning 配置文件设置。

接下来，我们将打开项目配置详细信息。单击 Explorer 树状视图（Xcode IDE 的左侧）中的 `AtomSampleViewer` 项目图标。在“General（常规）”下的左列中，选择构建配置 `AtomSampleViewer.GameLauncher`。生成的配置页面应如下所示：

{{< image-width "/images/user-guide/packaging/project-export-ios/atom-sampleviewer-configuration-page.png" "700">}}

转到“签名和功能”选项卡，并设置供应配置文件以链接到您的 Apple ID。自定义捆绑标识符（添加所需的任何字符串，例如：附加短语“test”）。启用 “Automatically manage signing”（自动管理签名）。

{{< image-width "/images/user-guide/packaging/project-export-ios/signing-and-capabilities-page.png" "700">}}

单击标有“AtomSampleViewer.GameLauncher”的区域中的栏，然后滚动下拉列表以单击“Edit Scheme”。

{{< image-width "/images/user-guide/packaging/project-export-ios/edit-scheme.png" "700">}}

设置所需的构建配置，并在 arguments 部分中设置相关的 CLI 参数。示例配置如下所示：

{{< image-width "/images/user-guide/packaging/project-export-ios/scheme-configuration-debug-build.png" "700">}}
{{< image-width "/images/user-guide/packaging/project-export-ios/scheme-configuration-cli-params.png" "700">}}

{{< note >}}
对于您希望在多个导出中保留的 CLI 参数设置，您可以通过代码或使用`autoexec.client.setreg`指定要在发布版本启动时运行的命令。您可以在 [设置起始关卡](/docs/user-guide/packaging/windows-release-builds/#set-the-starting-level) 中找到一个示例。
{{< /note >}}

确保将您的 iOS 设备连接到计算机。Xcode 应该能识别您的设备，并且它应该与项目兼容。如果没有，请按照 Xcode 错误提示对问题进行故障排除，并且可能需要更新。一旦这一切正确无误，按下 play 按钮来构建和部署项目。

如果生成和部署成功完成（假设您使用了示例配置），您应该在 iOS 设备上获得以下内容：

{{< image-width "/images/user-guide/packaging/project-export-ios/o3de-atom-sampleviewer-ios-device-result.png" "700">}}
