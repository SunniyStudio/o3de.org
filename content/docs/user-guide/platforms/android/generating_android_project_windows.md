---
title: 在 Windows 上生成 Android 项目
linkTitle: 生成 Android 项目
description: 生成和部署适用于 Open 3D Engine （O3DE） 的 Android 项目的分步指南。
weight: 200
---


在本教程中，您将学习如何在 Open 3D Engine （O3DE） 中构建和部署 Android 项目。

以下工作流程介绍了构建 Atom Sample Viewer 项目并将其部署到运行 Android 20 的设备的步骤。您还可以按照以下步骤在任何受支持的 Android 版本上构建自己的项目。

## 先决条件

以下说明假定您已：

1.  步骤在 Windows 主机上执行。
1.  满足软件 [Android 的先决条件](docs/user-guide/platforms/android#prerequisite-software-and-packages)。
1.  [Android Studio](https://developer.android.com/studio)的最新版

1.  [O3DE](https://github.com/o3de/o3de.git) 已在系统上本地克隆。必须初始化 Python(通过`python\get_python.cmd`脚本)，并且路径必须为 [注册](/docs/welcome-guide/setup/setup-from-github/building-windows/#register-the-engine)作为引擎。
1.  [Atom Sample Viewer](https://github.com/o3de/o3de-atom-sampleviewer.git) 已在系统本地克隆，并通过 [O3DE 命令行](docs/user-guide/project-config/cli-reference#register)。
1.  创建一个 [签名配置密钥库](/docs/user-guide/platforms/android#apk-signing)（从 Android Studio 或 keytool 命令行）
1.  必须在引擎根目录的 [AssetProcessorPlatformConfig.setreg](https://github.com/o3de/o3de/blob/324c0317e9cf61428d3bec492c7ba243a08718f9/Registry/AssetProcessorPlatformConfig.setreg#L64-L70) 中启用 **android** 平台：
    ```json
    "Platforms": {
    //"pc": "enabled",
    "android": "enabled"
    //"ios": "enabled",
    //"mac": "enabled",
    //"server": "enabled"
    },
    ```

{{< note >}}
此示例工作流表示在设置 [O3DE from GitHub](/docs/welcome-guide/setup/setup-from-github)
{{< /note >}}


## 设置环境变量

本教程将在示例步骤中使用以下环境变量

- `O3DE_ENGINE_PATH`

  将 [O3DE](https://github.com/o3de/o3de.git)存储库克隆到并注册到的本地路径。

- `O3DE_PROJECT_PATH`

  将 [Atom Sample Viewer](https://github.com/o3de/o3de-atom-sampleviewer.git)  存储库克隆到并注册到的本地路径。

- `O3DE_PROJECT_NAME`

  项目的名称(`AtomSampleViewer`).

- `TARGET_ANDROID_PROJECT_PATH`

  将 Android 项目 Gradle 脚本写入的路径。

- `ANDROID_SDK_HOME`

  Android SDK 设置位置的路径。此路径必须具有指向 sdk manager 命令行的以下子路径：
  ```
  %ANDROID_SDK_HOME%\cmdline-tools\latest\bin\sdkmanager.bat

- `ANDROID_SIGNING_CONFIG_KEYSTORE_FILE`

  用于 APK 签名的 [签名配置](/docs/user-guide/platforms/android#APK_Signing) 的密钥存储文件位置。密钥存储文件可以使用 Java 提供的[keytool](https://docs.oracle.com/en/java/javase/17/docs/specs/man/keytool.html) 实用程序创建。

- `ANDROID_SIGNING_CONFIG_KEY_ALIAS`

  密钥存储文件中将用于 APK 签名的签名密钥的别名。

## 分步说明

1. 为项目构建适用于 Android 的工具和资产。

配置和构建资产处理工具并处理资产。

   ```
   cd %O3DE_PROJECT_PATH%

   cmake -S . -B build/windows -DLY_DISABLE_TEST_MODULES=ON

   cmake --build build/windows --config profile --target AssetProcessorBatch

   cd %O3DE_PROJECT_PATH%\build\windows

   bin\profile\AssetProcessorBatch.exe --platform android

   ```

1. **确保 Android SDK 接受所有许可证**

   ```
   %ANDROID_SDK_HOME%\cmdline-tools\latest\bin\sdkmanager.bat --licenses
   ```
（如有必要，请按照命令接受许可证）

1. **配置 android 环境设置**

   ```
   %O3DE_ENGINE_PATH%\scripts\o3de.bat android-configure --set-value sdk.root="%ANDROID_SDK_HOME%" --global

   %O3DE_ENGINE_PATH%\scripts\o3de.bat android-configure --set-value platform.sdk.api=31 --global

   %O3DE_ENGINE_PATH%\scripts\o3de.bat android-configure --set-value ndk.version=25* --global

   %O3DE_ENGINE_PATH%\scripts\o3de.bat android-configure --set-value android.gradle.plugin=8.1.0 --global

   ```

1. **验证设置和环境并更正报告的任何问题**

   ```
   %O3DE_ENGINE_PATH%\scripts\o3de.bat android-configure --validate
   ```

1. **配置 Signing Config 密钥存储和别名**

   ```
   %O3DE_ENGINE_PATH%\scripts\o3de.bat android-configure --set-value signconfig.store.file="%ANDROID_SIGNING_CONFIG_KEYSTORE_FILE%" --global

   %O3DE_ENGINE_PATH%\scripts\o3de.bat android-configure --set-value signconfig.key.alias=%ANDROID_SIGNING_CONFIG_KEY_ALIAS% --global
   ```

1. **设置 Signing Config 密钥存储密码**

   ```
   %O3DE_ENGINE_PATH%\scripts\o3de.bat android-configure --set-password signconfig.store.password --global
   ```
出现提示时输入 + 确认密钥存储的密码

1. 设置 Signing Config 签名密钥密码
   ```
   %O3DE_ENGINE_PATH%\scripts\o3de.bat android-configure --set-password signconfig.key.password --global
   ```
出现提示时输入 + 确认密钥的密码

1. **运行 Android 项目生成脚本**

   ```
   %O3DE_ENGINE_PATH%\scripts\o3de.bat android-generate -p %O3DE_PROJECT_NAME% -B %TARGET_ANDROID_PROJECT_PATH%
   ```

1. **构建 Android APK**

   ```
   cd %TARGET_ANDROID_PROJECT_PATH%

   gradlew.bat assembleProfile

   ```

1. 部署 Android APK

   使用 Android SDK 中的 [ADB](https://developer.android.com/tools/adb) 工具，连接并列出连接的设备

   ```
   %ANDROID_SDK_HOME%\platform-tools\adb.exe devices
   ```
   
   您应该会看到设备列表（如果有附加的设备）及其附加状态。如果您没有看到任何连接的设备，请检查与设备的 USB 连接，并确保它已获得连接到设备（在设备本身上）的授权。

   如果您看到类似以下内容的内容：
   
   ```
   List of devices attached
   XXXXXXXXXXX     unauthorized
   ```

   这意味着您需要在计算机上为设备授权调试。

   完成所有授权后，您应该会看到如下内容：

   ```
   List of devices attached
   XXXXXXXXXXX     device
   ```

   识别出 Android 设备并授权计算机连接到该设备后，您就可以安装 APK。

   ```
   %ANDROID_SDK_HOME%\platform-tools\adb.exe install -t -r %TARGET_ANDROID_PROJECT_PATH%\app\build\outputs\apk\profile\app-profile.apk
   ```
