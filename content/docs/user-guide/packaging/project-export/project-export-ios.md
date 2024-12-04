---
linkTitle: 适用于 iOS 的项目导出
title: 适用于 iOS 的项目导出
description: （实验性）了解如何使用 Project Export CLI 自动准备要在 iOS 上发布的项目。
toc: true
weight: 420
---
本指南介绍如何使用项目导出工具在 iOS 上构建和部署游戏。我们将使用 iOS 导出脚本生成 Xcode 项目并准备资源，然后构建项目。导出的最终结果应该是可以安装在 iOS 设备上的 IPA 文件。

{{< note >}}
要了解有关项目导出工具的更多信息，请参阅页面 [项目导出 CLI 工具。](/docs/user-guide/packaging/project-export/project-export-cli)
{{< /note >}}
{{< important >}}
iOS 导出功能仅在 macOS 上可用，目前在 O3DE 中处于试验阶段。
{{< /important >}}

## 先决条件
1. 确保满足 [项目导出 CLI 工具页面](/docs/user-guide/packaging/project-export/project-export-cli)  先决条件。
2. 确保您已创建项目并将其注册到引擎中。
3. 在您的计算机上安装了有效的 Xcode 副本，以及与该 IDE 关联的有效 Apple Developer ID。要设置这样的 ID，请转到 [developer.apple.com](https://developer.apple.com)。要使用您想要的 Apple ID，请在 Xcode 中转到`Xcode -> Settings`。在“设置”窗口中，转到“帐户”选项卡，然后单击左侧面板底部的“+”图标以添加您的帐户。选择 Apple ID 继续，然后按照屏幕上的说明进行操作。
4. 您还需要确保 Xcode 安装了适用于 macOS 和 iOS 的标准 SDK，并且您首选的 iOS 测试设备与 Xcode 兼容。如果您不确定，可以查阅 [此页 安装 Simulator Runtimes](https://developer.apple.com/documentation/xcode/installing-additional-simulator-runtimes).
5. 确保为 iOS 启用了 o3de 引擎引导注册表。为此，请转到您的 O3DE 引擎安装并编辑`$O3DE_ENGINE_PATH/Registry/bootstrap.setreg`中的文件，以便`assets`字段使用`ios`。这样：
```
"assets": "ios",
```
6. 编辑 `$O3DE_ENGINE_PATH/Registry/AssetProcessorPlatformConfig.setreg` 文件，注释掉第 67 行:
```
//"ios": "enabled", -> "ios": "enabled",
```
这将指示 Asset Processor 确保在构建项目时缓存 iOS 资源。
7. 确保您拥有正确的 provisioning 配置文件设置，以便在对 App 进行代码签名时使用。或者，使用自动签名让 Xcode 为您处理。您可以了解有关如何设置的更多信息 [此处.](https://developer.apple.com/help/account/manage-profiles/create-a-development-provisioning-profile). 

## 快速入门
### 运行导出脚本
要使用 IPA 的捆绑 PAK 文件在发布模式下构建，您可以使用以下 export 命令：
```
export O3DE_ENGINE_PATH=/path/to/o3de
export PROJECT_PATH=/path/to/project
cd $PROJECT_PATH
$O3DE_ENGINE_PATH/scripts/o3de.sh export-project -es export_source_ios_xcode.py -pp . -ibp build/game-ios -ll INFO --config release
```

要改用 profile 模式，请将上述代码片段中的`--config`参数更改为使用`profile`。但请注意，这会自动将资源设置为 LOOSE 模式，并且不会将 PAK 文件用于最终的 IPA。

该调用应该是为您的项目创建 IPA 文件所需的全部内容。要在手机上安装 IPA 进行测试，您可以通过 Xcode 手动执行此操作。请查看 [此页面了解更多详情。](https://developer.apple.com/documentation/xcode/distributing-your-app-to-registered-devices#Install-the-app-on-user-devices)

作为导出过程的结果，还生成了生成的 Xcode 工程文件。您可以在`$PROJECT_PATH/build/game-ios`中找到它。对于频繁部署到 iOS 设备的常规迭代开发，建议您直接使用此项目文件与 Xcode 合作。有关详细信息，请参阅 [手动导出页面。](manual-export-atom-sampleviewer-ios#using-xcode)

## iOS导出脚本
O3DE 附带一个 [iOS 导出脚本](https://github.com/o3de/o3de/blob/bb3eafe30d8291f50b69924e5b7a432c8c6f53ca/scripts/o3de/ExportScripts/export_source_ios_xcode.py#L24)，能够生成 Xcode 项目文件以处理 iOS 上 O3DE 项目的标准用例。此脚本仅设计为从 macOS 计算机上的 O3DE 源安装运行。

导出脚本有两个主要部分：函数 [`export_ios_xcode_project`](https://github.com/o3de/o3de/blob/bb3eafe30d8291f50b69924e5b7a432c8c6f53ca/scripts/o3de/ExportScripts/export_source_ios_xcode.py#L24) 和 [仅在脚本由 CLI 调用时运行的启动代码](https://github.com/o3de/o3de/blob/bb3eafe30d8291f50b69924e5b7a432c8c6f53ca/scripts/o3de/ExportScripts/export_source_ios_xcode.py#L200)。关于这两部分的深入讨论，将在以后的 [开发者指南](https://docs.o3de.org/docs/engine-dev/) 中进行。

### 用法
要使用 export 脚本，您可以在运行 `export-project` 命令的同时发出参数。特定于脚本的参数将被推迟到它开始运行。

参数如下：
|参数名称 |描述 |必需？ |
| - | - | - |
| [`--script-help`](https://github.com/o3de/o3de/blob/9b90a24479e2b191d2125d34c1984b013b2cb13f/scripts/o3de/ExportScripts/export_source_ios_xcode.py#L89) |显示专门用于导出脚本的帮助信息。 | no |
| [`--config`](https://github.com/o3de/o3de/blob/f0c150d75722b3302753972b28d73a8036b70b31/scripts/o3de/ExportScripts/export_source_built_project.py#L243) | 定义在构建项目的二进制文件（如 GameLauncher）时的 CMake 构建配置。选项包括`profile` 或 `release`. | no |
| [`--tool-config`](https://github.com/o3de/o3de/blob/f0c150d75722b3302753972b28d73a8036b70b31/scripts/o3de/ExportScripts/export_source_built_project.py#L249) | 构建工具二进制文件时使用的 CMake 构建配置。选项包括`profile` 或 `release`. | no |
| [`--build-assets`](https://github.com/o3de/o3de/blob/f0c150d75722b3302753972b28d73a8036b70b31/scripts/o3de/ExportScripts/export_source_built_project.py#L262-L267) | 覆盖默认行为以包括处理所有资源并运行项目的资源打包器。当`option.build.assets`的 export-project-configure 默认值为 `False` 时，此选项可用。 | no |
| [`--skip-build-assets`](https://github.com/o3de/o3de/blob/f0c150d75722b3302753972b28d73a8036b70b31/scripts/o3de/ExportScripts/export_source_built_project.py#L262-L267) | 覆盖默认行为以跳过项目所有资产的重新处理和重新捆绑。当`option.build.assets`的 export-project-configure 默认值为 `True` 时，此选项可用。 | no |
| [`--fail-on-asset-errors`](https://github.com/o3de/o3de/blob/f0c150d75722b3302753972b28d73a8036b70b31/scripts/o3de/ExportScripts/export_source_built_project.py#L270-L275) | 覆盖默认行为，以在资产处理过程中发生任何错误时使导出过程失败。当 `option.fail.on.asset.errors` 的 export-project-configure 默认值为`False`时，此选项可用。 | no |
| [`--continue-on-asset-errors`](https://github.com/o3de/o3de/blob/f0c150d75722b3302753972b28d73a8036b70b31/scripts/o3de/ExportScripts/export_source_built_project.py#L270-L275) |  覆盖默认行为以忽略资产处理过程中发生的任何错误，并继续进行工程导出。当 `option.fail.on.asset.errors` 的 export-project-configure 默认值为`True`时，此选项可用。 | no |
| [`--seedlist`](https://github.com/o3de/o3de/blob/f0c150d75722b3302753972b28d73a8036b70b31/scripts/o3de/ExportScripts/export_source_built_project.py#L277-L282) | 用于资产捆绑的种子列表文件的路径。您可以为多个种子列表多次指定此项。此参数还允许对路径进行通配符匹配。 | no |
| [`--seedfile`](https://github.com/o3de/o3de/blob/f0c150d75722b3302753972b28d73a8036b70b31/scripts/o3de/ExportScripts/export_source_built_project.py#L284-L289) | 资产捆绑的种子文件的路径。示例种子文件是关卡或Prefab。您可以为多个种子文件多次指定此项。此参数还允许对路径进行通配符匹配。 | no |
| [`--level-name`](https://github.com/o3de/o3de/blob/f0c150d75722b3302753972b28d73a8036b70b31/scripts/o3de/ExportScripts/export_source_built_project.py#L291-L296) | 要导出的关卡的名称。这将在 `<o3de_project_path>/Cache/levels` 中查找以获取正确的级别Prefab。为游戏中的每个级别指定多次。如果级别已在种子列表中定义，则不需要这样做。 | no |
| [`--build-tools`](https://github.com/o3de/o3de/blob/f0c150d75722b3302753972b28d73a8036b70b31/scripts/o3de/ExportScripts/export_source_built_project.py#L319-L326) | 如果引擎不是 SDK，则构建 O3DE 工具链可执行文件。这将构建 AssetBundlerBatch 和 AssetProcessorBatch。当 `option.build.tools`的 export-project-configure 默认值为 `False` 时，此选项可用。| no |
| [`--skip-build-tools`](https://github.com/o3de/o3de/blob/f0c150d75722b3302753972b28d73a8036b70b31/scripts/o3de/ExportScripts/export_source_built_project.py#L319-L326) | 如果 engine.如果您已经拥有可用的工具，这可能很有用。当 `option.build.tools`的 export-project-configure 默认值为 `True` 时，此选项可用。 | no |
| [`--tools-build-path`](https://github.com/o3de/o3de/blob/f0c150d75722b3302753972b28d73a8036b70b31/scripts/o3de/ExportScripts/export_source_built_project.py#L330) | 指定生成 O3DE 工具链的构建文件的位置。如果未指定，则默认为`<o3de_project_path>/build/tools`.  | no |
| [`--max-bundle-size`](https://github.com/o3de/o3de/blob/f0c150d75722b3302753972b28d73a8036b70b31/scripts/o3de/ExportScripts/export_source_built_project.py#L349) | 指定给定 asset bundle 的最大大小。  | no |
| [`--ios-build-path`](https://github.com/o3de/o3de/blob/9b90a24479e2b191d2125d34c1984b013b2cb13f/scripts/o3de/ExportScripts/export_source_ios_xcode.py#L94) | 指定生成项目的 Xcode 项目文件的位置。如果未指定，则默认为`<o3de_project_path>/build/game_ios`. | no |
| [`--quiet`](https://github.com/o3de/o3de/blob/9b90a24479e2b191d2125d34c1984b013b2cb13f/scripts/o3de/ExportScripts/export_source_ios_xcode.py#L98) | 除非发生错误，否则禁止显示日志记录信息。 | no |

以下是此脚本的示例用法：
```
# On Mac
export IOS_OUTPUT_PATH=/path/to/ios/build/path
$O3DE_ENGINE_PATH/scripts/o3de.sh export-project --export-script $O3DE_ENGINE_PATH/scripts/o3de/ExportScripts/export_source_ios_xcode.py --project-path $PROJECT_PATH --ios-build-path $IOS_OUTPUT_PATH
```
`O3DE_ENGINE_PATH`, `O3DE_PROJECT_PATH` 和 `IOS_OUTPUT_PATH` 是环境变量。`O3DE_ENGINE_PATH` 和 `O3DE_PROJECT_PATH` 变量分别指向 o3de 源引擎和 o3de 项目的路径位置。`IOS_OUTPUT_PATH`变量对应于您希望生成的 Xcode 工程文件显示的文件夹路径。
