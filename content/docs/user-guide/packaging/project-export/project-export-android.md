---
linkTitle: 适用于 Android 的项目导出
title: 适用于 Android 的项目导出
description: 了解如何使用 Project Export CLI 自动准备要在 Android 上发布的项目。
toc: true
weight: 410
---
本指南介绍了如何使用项目导出工具在 Android 上构建和部署游戏。我们将使用 Android 导出脚本生成 Gradle 项目，准备资源，然后构建项目。导出的最终结果应该是可以安装在您的 Android 设备上的 APK 文件，以及在连接的手机上自动安装的额外步骤。

{{< note >}}
要了解有关项目导出工具的更多信息，请查阅页面 [适用于 Windows 和 Linux 的项目导出](/docs/user-guide/packaging/project-export/project-export-pc).
{{< /note >}}
{{< note >}}
要了解有关如何手动导出 Android 项目的更多信息，请参阅页面 [生成 Android 项目](/docs/user-guide/platforms/android/generating_android_project_windows/#prerequisites).
{{< /note >}}

## 先决条件
1. 确保满足 [“适用于 Windows 和 Linux 的项目导出”页面](/docs/user-guide/packaging/project-export/project-export-pc) 先决条件。
2. 确保您已创建项目并将其注册到引擎中。检查您是否可以使用 Editor 成功构建和打开项目。
3. 确保在 O3DE 中使用 Android 的所有先决条件都已满足。您可以 [此处](/docs/user-guide/platforms/android/generating_android_project_windows/#prerequisites)了解软件依赖项和项目设置先决条件 [此处](/docs/user-guide/platforms/android/generating_android_project_windows/#prerequisites)。对于 Android SDK，请确保记录根文件夹路径在硬盘驱动器上的位置，因为设置配置时需要这样做。
4. 确保您配置了正确的密钥库文件以进行签名。您可以了解更多信息 [此处](/docs/user-guide/platforms/android/#apk-signing)。这是一个示例密钥库文件，您可以创建该文件（您需要带有`keytool`的 JDK）:

```
set KEYSTORE_FILE_PATH=C:\path\to\android-key.keystore
set STOREPASS=o3depass
set ALIAS=o3dekey
set KEYPASS=o3depass
set KEYSIZE=2048
set VALIDITY=10000
set DNAME="cn=o3de-android-project, ou=o3de, o=LF, c=US"
keytool -genkey -keystore %KEYSTORE_FILE_PATH% -storepass %STOREPASS%  -alias %ALIAS% -keypass %KEYPASS% -keyalg RSA -keysize %KEYSIZE% -validity %VALIDITY% -dname %DNAME%
```
5. 确保为 Android 启用了 O3DE 引导注册表。为此，请转到您的 O3DE 安装并编辑 `%O3DE_ENGINE_PATH%\Registry\bootstrap.setreg` 中的文件，以便 `assets` 字段使用 `android`。这样：
```
"assets": "android",
```
6. 确保您的项目具有适用于 Android 的正确配置设置。您可以使用`android-configure` CLI 工具来确保此设置已完成设置。要查看当前配置的内容，请运行：
```
$O3DE_ENGINE_PATH/scripts/o3de.bat android-configure -l
```
您还可以使用以下方法验证当前配置：
```
$O3DE_ENGINE_PATH/scripts/o3de.bat android-configure --validate
```
如果您尚未配置 Android，或者遇到 validate 命令问题，则可以运行以下命令：
```
$O3DE_ENGINE_PATH/scripts/o3de.bat android-configure --set-value platform.sdk.api=30
$O3DE_ENGINE_PATH/scripts/o3de.bat android-configure --set-value ndk.version=25.*
$O3DE_ENGINE_PATH/scripts/o3de.bat android-configure --set-value android.gradle.plugin=8.1.0
$O3DE_ENGINE_PATH/scripts/o3de.bat android-configure --set-value sdk.root=C:/path/to/android/sdk
$O3DE_ENGINE_PATH/scripts/o3de.bat android-configure --set-value signconfig.store.file=C:/path/to/android-key.keystore
$O3DE_ENGINE_PATH/scripts/o3de.bat android-configure --set-value signconfig.key.alias=o3dekey
$O3DE_ENGINE_PATH/scripts/o3de.bat android-configure --set-value asset.mode=PAK
```
注意：您必须已经知道 sdk.root 和 signconfig.store.file 在硬盘上的位置，并相应地使用这些路径。要设置 keypass 和 storepass，您必须使用以下命令
```
$O3DE_ENGINE_PATH/scripts/o3de.bat android-configure --set-password signconfig.store.password
$O3DE_ENGINE_PATH/scripts/o3de.bat android-configure --set-password signconfig.key.password
```
确保此处提供的密码与您在 keystore 文件中使用的密码相匹配，并且您记住了这些密码。

## 使用 Project Manager
### 入门
要了解有关 Project Manager 中导出按钮功能和导出设置面板的更多信息，请 [单击此处](project-export-pc/#getting-started)。本指南将仅涵盖特定于 Android 的详细信息。

要启动 Android 导出，该过程与为 Windows 或 Linux 执行的操作相同。在项目卡中，只需打开下拉菜单，然后单击 Export Launchers 子菜单中的 Android 选项。单击后，在确认您已准备好继续后，导出应立即开始。

假设您的构建中没有错误，则生成的 Android 项目将显示在您在导出设置面板中指定 Android APK 构建路径的任何位置。

### Export Settings 面板

要访问导出设置面板，只需单击“打开导出设置...”按钮。单击它后，您应该会看到以下表格。

{{< image-width "/images/user-guide/packaging/project-export-pc/export-settings-android.png" "400">}}

该面板分为两个主要部分。面板的上半部分包含所有 O3DE 支持的平台中通用的设置。下半部分包含由选项卡分隔的平台特定设置。在本指南中，我们将重点介绍“Android”选项卡。

Android 特定功能如下：

* 部署到 Android 设备 - 导出过程完成后，如果您的 Android 设备连接到您的计算机（并且“adb”可以识别该设备），则导出的 Android 应用程序将自动安装到您的设备上。这对于在实际硬件上快速测试非常有用。
* 资产模式配置 - 指定在 APK 文件包中应如何组织游戏资产。LOOSE 意味着所有文件都单独存储，就像原始 zip 存档一样。PAK 表示所有文件都合并到单个 blob 文件中。
* Android APK 构建路径 - 指定生成 Android 项目的位置，这将通知 gradle 在何处构建最终 APK。


{{< note >}}
在某些平台（如 Windows）上，当您尝试导出 Android APK 项目时，您可能会遇到 Gradle 由于以下原因而过早导出失败的问题：
```
...the filename is too long (exceeds 260 characters)
```
如果您遇到此问题，并且 Android APK 构建路径是相对于项目文件夹的，则可能需要将文件夹移动到更靠近硬盘驱动器根目录的位置，例如`C:/o3de-android`。
{{< /note >}}

## 使用 CLI
### 运行导出脚本
要使用捆绑的 PAK 文件在 release 模式下为 APK 进行构建，您可以使用以下 export 命令：
```
set O3DE_ENGINE_PATH=C:\path\to\o3de
set PROJECT_PATH=C:\path\to\project
cd %PROJECT_PATH%
%O3DE_ENGINE_PATH%\scripts\o3de.bat export-project -es export_source_android.py -pp . -abp build\game-android -ll INFO --config release --asset-mode PAK
```

要改用 profile 模式，请将上述代码片段中的 `--config` 参数更改为使用 `profile` 。如果您希望将资产模式设置为 LOOSE，请使用`--asset-mode LOOSE`。

该调用应该是为您的项目创建 APK 文件所需的全部内容。要在手机上安装 APK 进行测试，可以通过在上面的导出代码段中添加`--deploy-to-android` 标志来完成此操作。请注意，要使用此功能，您必须确保 Android 设备已连接到您的计算机，并且 ADB 能够正确访问该设备。

导出过程的结果是生成的 Android Gradle 项目文件夹。您可以在`%PROJECT_PATH%\build\game-android`中找到它。您可以从 Android Studio 中使用此项目文件夹来调整任何特定设置，或运行 Debugger。

## Android 导出脚本
O3DE 附带一个 [Android 导出脚本](https://github.com/o3de/o3de/blob/bb3eafe30d8291f50b69924e5b7a432c8c6f53ca/scripts/o3de/ExportScripts/export_source_android.py#L28)，能够生成 Android Gradle 项目文件夹，以处理 Android 上 O3DE 项目的标准用例。

导出脚本有两个主要部分：函数 [`export_source_android_project`](https://github.com/o3de/o3de/blob/bb3eafe30d8291f50b69924e5b7a432c8c6f53ca/scripts/o3de/ExportScripts/export_source_android.py#L28) 和 [启动代码](https://github.com/o3de/o3de/blob/bb3eafe30d8291f50b69924e5b7a432c8c6f53ca/scripts/o3de/ExportScripts/export_source_android.py#L222)，仅在 CLI 调用脚本时运行。关于这两个部分的深入讨论可以在 [开发者指南](/docs/engine-dev/tools/o3de-cli/project-export) 中找到。

### 用法
要使用 export 脚本，您可以在运行`export-project`命令的同时发出参数。特定于脚本的参数将被推迟到它开始运行。

参数如下：

|参数名称 |描述 |必需？ |
| - | - | - |
| [`--script-help`](https://github.com/o3de/o3de/blob/bb3eafe30d8291f50b69924e5b7a432c8c6f53ca/scripts/o3de/ExportScripts/export_utility.py#L120) | 显示专门用于导出脚本的帮助信息。| no |
| [`--config`](https://github.com/o3de/o3de/blob/bb3eafe30d8291f50b69924e5b7a432c8c6f53ca/scripts/o3de/ExportScripts/export_utility.py#L124) | 定义在构建项目的二进制文件（如 GameLauncher）时的 CMake 构建配置。选项包括`profile` 或 `release`. | no |
| [`--tool-config`](https://github.com/o3de/o3de/blob/bb3eafe30d8291f50b69924e5b7a432c8c6f53ca/scripts/o3de/ExportScripts/export_utility.py#L129) | 构建工具二进制文件时使用的 CMake 构建配置。选项包括 `profile` 或 `release`. | no |
| [`--build-assets`](https://github.com/o3de/o3de/blob/bb3eafe30d8291f50b69924e5b7a432c8c6f53ca/scripts/o3de/ExportScripts/export_utility.py#L186) | 覆盖默认行为以包括处理所有资源并运行项目的资源打包器。当`option.build.assets` 的 export-project-configure 默认值为`False` 时，此选项可用。 | no |
| [`--skip-build-assets`](https://github.com/o3de/o3de/blob/bb3eafe30d8291f50b69924e5b7a432c8c6f53ca/scripts/o3de/ExportScripts/export_utility.py#L188) | 覆盖默认行为以跳过项目所有资产的重新处理和重新捆绑。 当`option.build.assets` 的 export-project-configure 默认值为`False` 时，此选项可用。| no |
| [`--fail-on-asset-errors`](https://github.com/o3de/o3de/blob/bb3eafe30d8291f50b69924e5b7a432c8c6f53ca/scripts/o3de/ExportScripts/export_utility.py#L151) | 覆盖默认行为，以在资产处理过程中发生任何错误时使导出过程失败。当`option.fail.on.asset.errors`的 export-project-configure 默认值为`False`时，此选项可用。| no |
| [`--continue-on-asset-errors`](https://github.com/o3de/o3de/blob/bb3eafe30d8291f50b69924e5b7a432c8c6f53ca/scripts/o3de/ExportScripts/export_utility.py#L153) |  覆盖默认行为以忽略资产处理过程中发生的任何错误，并继续进行工程导出。 当`option.fail.on.asset.errors`的 export-project-configure 默认值为`True`时，此选项可用。 | no |
| [`--seedlist`](https://github.com/o3de/o3de/blob/bb3eafe30d8291f50b69924e5b7a432c8c6f53ca/scripts/o3de/ExportScripts/export_utility.py#L156) | 用于资产捆绑的种子列表文件的路径。您可以为多个种子列表多次指定此项。此参数还允许对路径进行通配符匹配。| no |
| [`--seedfile`](https://github.com/o3de/o3de/blob/bb3eafe30d8291f50b69924e5b7a432c8c6f53ca/scripts/o3de/ExportScripts/export_utility.py#L163) | 资产捆绑的种子文件的路径。示例种子文件是关卡或预制件。您可以为多个种子文件多次指定此项。此参数还允许对路径进行通配符匹配。 | no |
| [`--level-name`](https://github.com/o3de/o3de/blob/bb3eafe30d8291f50b69924e5b7a432c8c6f53ca/scripts/o3de/ExportScripts/export_utility.py#L170) | 要导出的关卡的名称。这将在`<o3de_project_path>/Cache/levels` 中查找以获取正确的级别预制件。为游戏中的每个级别指定多次。如果级别已在种子列表中定义，则不需要这样做。 | no |
| [`--build-tools`](https://github.com/o3de/o3de/blob/bb3eafe30d8291f50b69924e5b7a432c8c6f53ca/scripts/o3de/ExportScripts/export_utility.py#L142) | 如果引擎不是 SDK，则构建 O3DE 工具链可执行文件。这将构建 AssetBundlerBatch 和 AssetProcessorBatch。当`option.build.tools`的 export-project-configure 默认值为`False`时，此选项可用。 | no |
| [`--skip-build-tools`](https://github.com/o3de/o3de/blob/bb3eafe30d8291f50b69924e5b7a432c8c6f53ca/scripts/o3de/ExportScripts/export_utility.py#L145) | 如果 engine.如果您已经拥有可用的工具，这可能很有用。 当`option.build.tools`的 export-project-configure 默认值为`True`时，此选项可用。| no |
| [`--tools-build-path`](https://github.com/o3de/o3de/blob/bb3eafe30d8291f50b69924e5b7a432c8c6f53ca/scripts/o3de/ExportScripts/export_utility.py#L133) | 指定生成 O3DE 工具链的构建文件的位置。如果未指定，则默认为`<o3de_project_path>/build/tools`.  | no |
| [`--max-bundle-size`](https://github.com/o3de/o3de/blob/bb3eafe30d8291f50b69924e5b7a432c8c6f53ca/scripts/o3de/ExportScripts/export_utility.py#L136) | 指定给定 asset bundle 的最大大小。  | no |
| [`--android-build-path`](https://github.com/o3de/o3de/blob/bb3eafe30d8291f50b69924e5b7a432c8c6f53ca/scripts/o3de/ExportScripts/export_source_android.py#L144) |指定生成项目的 Android Gradle 项目文件夹的位置。如果未指定，则默认为 `<o3de_project_path>/build/android`. | no |
| [`--quiet`](https://github.com/o3de/o3de/blob/bb3eafe30d8291f50b69924e5b7a432c8c6f53ca/scripts/o3de/ExportScripts/export_utility.py#L190) | 除非发生错误，否则禁止显示日志记录信息。 | no |

以下是此脚本的示例用法：
```
set ANDROID_OUTPUT_PATH=C:\path\to\android\build\path
%O3DE_ENGINE_PATH%\scripts\o3de.bat export-project --export-script %O3DE_ENGINE_PATH%\scripts\o3de\ExportScripts\export_source_android.py --project-path %PROJECT_PATH% --android-build-path %ANDROID_OUTPUT_PATH%
```
`O3DE_ENGINE_PATH`, `O3DE_PROJECT_PATH` 和 `ANDROID_OUTPUT_PATH` 是环境变量。`O3DE_ENGINE_PATH` 和 `O3DE_PROJECT_PATH`变量分别指向 o3de 源引擎和 o3de 项目的路径位置。`ANDROID_OUTPUT_PATH`变量对应于您希望生成的 Android Gradle 项目文件夹显示的文件夹路径。
