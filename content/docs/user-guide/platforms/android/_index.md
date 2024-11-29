---
title: Android 对 Open 3D Engine （O3DE） 的支持
linkTitle: Android
description: 概述 Open 3D Engine 对 Android 开发的支持。
weight: 200
---

适用于 Android 的 **Open 3D Engine （O3DE）** 项目依靠 Gradle 构建系统来构建启动和运行 O3DE 项目的最终 APK。O3DE 提供 **Android 项目生成脚本**，该脚本生成构建 Android 版 O3DE 项目所需的 Gradle 构建脚本。Gradle 构建脚本使用 CMake 编译原生 C++ 代码，使用 Java 编译和构建 Android 应用程序，并将构建和打包最终的 APK 和项目资产。


## 必备软件和软件包

### Android SDK
[Android SDK](https://developer.android.com/studio) 包含为 O3DE 项目构建 Android 应用程序所需的 Android 库、程序包、NDK 和其他工具。

您可以使用 [Android Studio](https://developer.android.com/studio) 设置和配置 Android SDK，[Android Studio](https://developer.android.com/studio) 是 Gradle 常用的 IDE，用于开发和构建 Android 应用程序。首次启动 Android Studio 时，请按照步骤下载并安装至少一个 SDK 平台。您还可以从 Android Studio 中设置和配置其他 SDK。设置 Android Studio 后，您可以通过选择 **Tools > SDK Manager** 或选择工具栏中的 **SDK Manager** 图标来找到 Android SDK 主页。您可以在 **Android SDK Location** 下找到该位置。

或者，您可以下载 Android SDK **仅限命令行工具** 包，而无需安装 Android Studio。命令行工具包的安装位置将决定 Android SDK Home 的位置。请参阅 [SDK Manager 说明](https://developer.android.com/tools/sdkmanager) 了解安装套件的位置。

确定 Android SDK 位置后，需要在 Android 配置设置中进行设置 'sdk.root'

{{<note>}}
为了使生成脚本自动下载所需的包，您必须接受代表这些包的许可协议。这可以通过 Android Studio 或通过带有`--licenses`参数的`sdkmanager`命令行工具来完成。
{{</note>}}

### Java Development Kit (JDK)
Android SDK 命令行工具需要 [Java 开发工具包](https://www.java.com/releases/) （JDK） 来编译项目的 Android Java 源代码，并创建用于对 APK 进行签名的 Java 密钥存储 （JKS） 文件。Android 所需的 JDK 版本取决于 [Android SDK 要求](https://developer.android.com/build/jdks)。JDK 安装位置必须在 **JAVA_HOME** 环境变量中设置，或者 JDK 可执行文件的位置必须在 **PATH** 环境中。

{{<note>}}
[Java 开发工具包](https://www.java.com/releases/) 的版本必须与 [SDK 管理器](https://developer.android.com/tools/sdkmanager) 兼容。但是，如果您希望指定旧版本的 Android Gradle 插件，则用于运行 Gradle 构建的 JDK 版本可能会有所不同，并且可能需要设置`JAVA_HOME`环境以匹配旧版本的 SDK，以便成功构建 APK。
{{</note>}}


### Gradle 和 Android Gradle 插件
[Gradle](https://gradle.org/install/) 是旨在构建和创建 Android 应用程序的构建系统。 [Android Gradle Plugin](https://developer.android.com/build/releases/gradle-plugin)向 Gradle 构建系统添加了支持 Android 构建的功能。当前的 **Android 项目生成脚本** 支持 **Android Gradle 插件** 版本 8.1 或更高版本，而这反过来又需要 **Gradle** 版本 8.1 或更高版本。必须通过以下三种方式中的至少一种方式设置 Gradle 安装的位置，以便用户能够发现它：

{{<note>}}
支持旧版本（4.2.0 或更高版本）的 Android Gradle 插件，但不建议这样做。
{{</note>}}


1. Android 配置设置`gradle.home`
2. 在 **GRADLE_HOME** 环境变量中设置。
3. gradle 可执行脚本的位置位于 **PATH** 环境中。

### CMake
[CMake](https://cmake.org/download/#latest) (version {{< versions/cmake >}} or newer) 是 O3DE 用于所有本机 C++ 构建的项目生成器，无论平台如何。与 Gradle 的可发现性要求一样，CMake 安装的位置必须至少通过以下三种方式之一进行设置：

1. Android 配置设置 `cmake.home`
2. 在 **CMAKE_HOME** 环境变量中设置。
3. cmake 可执行脚本的位置位于 **PATH** 环境中。


### Ninja 构建系统
CMake 使用 [Ninja](https://ninja-build.org/) 构建系统为 O3DE 项目构建底层原生 C++ 代码。必须在 **PATH** 环境中设置 **Ninja**。

## 配置环境
**Android 项目生成脚本**需要使用用户指定的设置和必备软件的位置进行配置，以便生成 O3DE Android Gradle 脚本。Android 设置使用 [O3DE 命令行](docs/user-guide/project-config/cli-reference#android-configure) 中的`android-configure`命令进行管理。

以下是可配置的 android 设置列表。

|设置键 |描述 |
| ----------------------------------- | ---------- |
| sdk.root | Android SDK 在构建系统上的根路径。 |
| platform.sdk.api | [Android 平台 SDK API 级别](https://developer.android.com/tools/releases/platforms). |
| ndk.version | [Android NDK](https://developer.android.com/ndk/downloads) 的版本。可以使用文件匹配模式（例如，25.* 将搜索迄今为止最新的主要版本 25）。 |
| android.gradle.plugin | 用于 Gradle 的 [Android Gradle Plugin](https://developer.android.com/reference/tools/gradle-api) 的版本。 |
| gradle.home | 本地安装的 Gradle 版本的根路径。如果未设置，则将使用 **PATH** 环境中的 Gradle。 |
| gradle.jvmargs | 自定义 [jvm arguments](https://docs.gradle.org/current/userguide/config_gradle.html#sec:configuring_jvm_memory) 在调用 gradle 时设置。 |
| cmake.home | 本地安装的 cmake 版本的根路径。如果未设置，则将使用 PATH 环境中的 Cmake。 |
| signconfig.store.file | 用于创建 [签名配置](见 https://developer.android.com/studio/publish/app-signing) 的密钥存储文件。 |
| signconfig.key.alias | 密钥存储中的密钥别名，用于标识签名密钥。|
| asset.mode | 资产模式，用于确定资产在目标 APK 中的存储方式。有效值为 LOOSE 和 PAK。 |
| strip.debug | 在部署到 APK 之前去除构建的原生库的调试符号的选项。|
| oculus.project | 用于在构建 APK 时设置特定于 oculus 的构建选项的选项。 |
| asset.bundle.subpath | 项目根目录的子路径，用于指定 [捆绑资产](/docs/user-guide/packaging/asset-bundler/) 的生成位置。|
| extra.cmake.args | 可选字符串，用于在 android gradle 构建过程中的本机项目生成期间设置其他 cmake 参数。 |



## APK 签名
所有 APK 都必须经过 [数字签名](https://developer.android.com/studio/publish/app-signing) 才能安装到 Android 设备上。这是通过在 Android Project Gradle 脚本中提供签名配置来完成的。此签名配置适用于调试开发环境，在设计上不安全。您可以通过 [Android Studio](https://developer.android.com/studio/publish/app-signing#generate-key)生成签名配置密钥存储、签名密钥和证书。您还可以使用 JDK [keytool](https://docs.oracle.com/en/java/javase/17/docs/specs/man/keytool.html)命令从命令行创建密钥存储。

（有关 APK 签名以及如何创建密钥库、密钥和证书的更多详细信息，请参见 [此处](https://developer.android.com/studio/publish/app-signing). )

在创建 Android 项目 Gradle 脚本期间，如果设置了任何 `signconfig.*` 值，则生成过程将尝试使用以下值嵌入签名配置信息：

- **Key Store File**

  包含签名密钥和证书的密钥存储文件。 ( `--signconfig-store-file`/`signconfig.store.file`)

- **Signing Key Alias**

  密钥存储文件中签名密钥的别名。( `--signconfig-key-alias`/`signconfig.key.alias`)

- **Key Store Password**

  密钥存储文件的密码。(`signconfig.store.password`)

- **Signing Key Password**

  执行签名操作所需的签名密钥的密码。(`signconfig.key.password`)

{{<note>}}
您不能通过`android-configure` 或 `android-generate`命令传入这两个密码中的任何一个。当您运行`android-generate`但缺少任何签名密码时，该命令将显示标准密码集并确认输入以嵌入生成的项目。如果您想设置将用于签名配置的密码而不提示输入密码，则需要使用参数`--set-password SETTING`调用 `android-configure`，其中 `SETTING`是 `signconfig.store.password` 或 `signconfig.key.password`。这将显示相同的标准密码集和确认提示，但会将其存储在 android 设置中，以便将来调用`android-generate`。
{{</note>}}

## Android 资产
在生成 O3DE Android 项目之前，必须处理 Android 的资产。您可以通过在项目中使用 **O3DE 编辑器** 和 **Asset Processor**（或 **Asset Processor Batch**）来执行此操作。

特别是对于 Android，您必须配置注册表设置值，以便 Asset Processor 可以处理 Android 平台资产。为此，请更新`<engine-root>\Registry\AssetProcessorPlatformConfig.setreg`文件。

在文件的 `Amazon/AssetProcessor/Platforms` 中，设置 `"android"` 标签为 `"enabled"`。例如：

```
"Amazon": {
    "AssetProcessor": {
        ...
        "Platforms": {
            //"pc": "enabled",
            "android": "enabled"
            //"ios": "enabled",
            //"mac": "enabled",
            //"server": "enabled"
         }
    ...
}
```

启动 Asset Processor Batch 时，您必须传入`--platforms=android` 参数。请参阅 [在 Windows 上生成 Android 项目](/docs/user-guide/platforms/android/generating_android_project_windows/#steps) 中的步骤 3。

处理完 Android 的资产后，您可以继续生成 Android 项目。

## 旧版 Android 项目生成脚本

原始的 android 生成脚本仍然可以在 `<engine-root>/cmake/Tools/Platform/Android/generate_android_project.py`中找到，但仅支持 Android Gradle Plugn 版本 4.x。

`generate_android_project.py` 脚本包含以下参数：

| 参数| 说明 | 默认值 | 注意 |
| --- | --- | --- | --- |
| `--engine-root` | 引擎文件夹根目录的路径。 | \<required\> |     |
| `--build-dir` | 创建项目的`build`文件夹的路径。 | \<required\> | 此路径可以是相对于引擎根的路径，也可以是绝对路径。 |
| `--third-party-path` | O3DE 指定的第三方文件夹的路径。 | \<required\> | 这与 O3DE 的`LY_3RDPARTY_PATH` CMake 变量相关。 |
| `--android-sdk-path` | Android SDK 文件夹的路径。 | \<required\> | 此路径通常在配置 Android Studio 时设置。 |
| `-g`, `--project-path` | 要为其生成 Android 项目的 O3DE 项目的路径。 | \<required\> |     |
| `--android-sdk-platform` | 构建所基于的 Android API 级别。 | `--android-sdk-path`值中最新安装的 API 级别| API 级别需要与目标 Android 设备的版本匹配。（有关 API 级别的描述，请参阅 [https://source.android.com/setup/start/build-numbers](https://source.android.com/setup/start/build-numbers) )。<br><br>目前，支持的最低 API 级别为 28。 |
| `--android-sdk-build-tool-version` | 覆盖 Android 构建工具版本。 | 在`--android-sdk-path`值中安装的最新 Android 构建工具 |     |
| `--gradle-plugin-version` | 要为 build 指定的 Android Gradle 插件版本。 | `4.2.0` | 支持的最低版本为 `4.2.0` (默认值)。有关详细信息，请参阅[https://developer.android.com/studio/releases/gradle-plugin?buildsystem=ndk-build](https://developer.android.com/studio/releases/gradle-plugin?buildsystem=ndk-build). <br><br>目前，仅支持 `4.2.0`。可以更新此脚本以支持将来的版本。 |
| `--android-ndk-version` | 构建所基于的特定 NDK 值`.` | `21.4.7075529` | 这些值基于 NDK 的完整修订号：<br>-   `16.1.4479499`    <br>-   `17.2.4988734`    <br>-   `18.1.5063045`    <br>-   `19.2.5345600`    <br>-   `20.0.5594570`    <br>-   `20.1.5948944`    <br>-   `21.0.6113669`    <br>-   `21.1.6352462`    <br>-   `21.2.6472646`    <br>-   `21.3.6528147`    <br>-   `21.4.7075529`    <br>-   `22.0.7026061`    <br>-   `22.1.7171670`    <br><br>您还可以使用 `*` 通配符选择最新版本。例如， `21.*`会选择最新版本的 NDK21。 |
| `--android-native-api-level` | NDK 支持的 API 级别，用于原生 CMake Android 支持。| 由`--android-sdk-platform`参数设置的 API 级别 | - NDK 的最低 API 级别为 `24`. |
| `--enable-unity-build` | （可选）使用 unity build 编译源代码。 | 已禁用 Unity 版本。 |      |
| `--include-apk-assets` | （可选）在 APK 中包含游戏资产。|APK 中未包含的资产。 |      |
| `--asset-mode` | 配置如何在 APK 中包含资产。Asset Mode 值可以是 `VFS`, `PAK`, 或 `LOOSE`. | `LOOSE` | - `LOOSE` (默认值): 将资源解压缩并存储为包含在资源缓存中的单个文件。<br>- `PAK`: 将资源打包到 PAK 文件中。<br>- `VFS`: 通过 Virtual File System 从目标设备远程引用资源。|
| `--asset-type` | 要包含在 APK 中的资产类型 | `android` | 将来，此参数将被弃用，并且仅支持 `android`类型的资产。 |
| `--gradle-install-path` | 如果 Gradle 命令不在 PATH 环境中，或者您想要指定备用 Gradle 文件夹，则此参数允许您覆盖已安装的 Gradle 版本以用于项目生成。|在 PATH 环境中安装的 Gradle 的路径。 | - Gradle 的最低版本是`6.7.1`，它与 Android Gradle 插件的最低版本`4.2.0`兼容。  <br>- 如果不指定此参数，并且 PATH 环境中未安装 Gradle，则会出现错误。|
| `--cmake-install-path` | 如果 CMake 命令不在 PATH 环境中，或者您想要指定备用 CMake 安装，则此参数允许您覆盖已安装的 CMake 版本以用于项目生成。|作为 PATH 环境一部分的 CMake 安装。| - CMake 的最低版本为 `3.21`, 支持 Android Gradle 插件版本 `4.2.0.`. <br>- 如果不指定此参数，并且 CMake 未安装在 PATH 环境中，则会发生错误。 |
| `--ninja-install-path` | 如果 Ninja 命令不在 PATH 环境中，或者您想要指定备用 Ninja 安装，则此参数允许您覆盖已安装的 Ninja 版本以用于项目生成。|Ninja 构建工具安装是 PATH 环境的一部分。 | - 如果不指定此参数，并且 Ninja 未安装在 PATH 环境中，则会发生错误。 |
| `--signconfig-store-file` | （可选）一个 Android JKS KeyStore，用于在需要构建项目并将其部署到 Android 设备时进行 APK 签名。如果省略此选项，则必须稍后在 Android Studio 中设置它，以便部署到 Android。否则，项目只能生成未签名的 APK。（有关更多信息，请参阅[Android Key Store](#android-keystore) 部分。 | None | - 如果未指定，则生成的 Android 项目将不包含任何签名信息。您仍然可以构建 Android 项目，但不能部署到任何设备。<br>- 您可以通过 Android Studio 在项目生成后添加签名密钥信息。 |
| `--signconfig-store-password` | Android JKS KeyStore 文件所需的密码（如果由`--signconfig-store-file`指定）。| None | - 如果您使用 `--signconfig-store-file`指定 JKS，则为必需。 |
| `--signconfig-key-alias` | Android JKS KeyStore 文件中签名密钥的必需别名（如果由`--signconfig-store-file`指定）。 | None | - 如果您使用 `--signconfig-store-file`指定 JKS，则为必需。 |
| `--signconfig-key-password` | 在 Android JKS KeyStore 文件中使用签名密钥所需的密码（如果由`--signconfig-store-file`指定）。 | None | - 如果您使用 `--signconfig-store-file`指定 JKS，则为必需。 |
| `--overwrite-existing` | （可选）覆盖目标构建文件夹中的现有脚本（如果它们已存在）。|不覆盖。 |     |


## Android 部署脚本
在 `<engine-root>/cmake/Tools/Platform/Android/deploy_android.py`中找到用于部署 Android 项目的脚本。

`deploy_android.py` 脚本 包含以下参数：

| 参数 | 说明 | 默认值 | 注意 |
| --- | --- | --- | --- |
| `-h`, `--help` | 显示帮助消息并退出。 |     |     |
| `-b`, `--build-dir` | 要从中部署的项目的 build 目录。 |     |     |
| `-c`, `--configuration` |要从中部署的已构建二进制文件的构建配置。|     |     |
| `--device-id-filter` | 要筛选部署的已连接 Android 设备 ID 的逗号分隔列表。|所有连接的 Android 设备。| 仅当连接了多个设备并且您想要筛选哪些设备将成为部署的目标时使用。|
| `-t`, `--deployment-type` | 要部署的类型 (`APK`, `ASSETS`, 或 `BOTH`):<br/><br/>**`APK`**<br/>仅部署 APK，不部署任何外部资产(见`generate_android_project.py`中的`--include-apk-assets`选项)。APK 中已包含的任何资产也将被部署。<br/><br/>**`ASSETS`**<br/>仅部署 APK 的资产。仅在您在项目生成期间未设置 `--include-apk-assets` 时，此选项才有效。<br><br>**`BOTH`**<br> 部署 APK 和资产，而不考虑 `--include-apk-assets` 选项。| `BOTH` |分别部署 APK 和资产可以优化不同的工作流程。例如，仅更新代码的开发人员应使用`APK`来提高其部署效率（前提是至少在初始阶段部署了资产）。在另一个示例中，仅更新资源的技术艺术家应使用`ASSETS` (前提是在项目生成期间未设置`--include-apk-assets`).|
| `--clean` | 在部署之前，通过卸载任何预先存在的项目来清理目标 Android 设备。 |     | 卸载项目还会删除所有已部署的资产。 |
| `--kill-adb-server` |  在部署结束时终止 adb server 的选项。 |     |     |
| `--debug` | 用于启用调试消息的选项。 |     |     |


## 其他主题

+ [在 Windows 上生成 Android 项目](/docs/user-guide/platforms/android/generating_android_project_windows)

