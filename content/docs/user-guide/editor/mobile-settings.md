---
description: ' 使用Project Settings Tool修改 Open 3D 引擎中移动设备游戏的项目设置。 '
title: 修改移动设备游戏的项目设置
draft: true
---

使用**项目设置工具** (Project Settings Tool，PST)，在所有移动平台上对项目设置进行简化更改。项目设置工具**会显示所有项目设置文件的属性，以便您一次性编辑和保存更改。

{{< note >}}
在使用**项目设置工具**之前，您必须安装 O3DE 编辑器，并创建一个活动项目并将其设置为默认项目。

目前，只有 Android 和 iOS 支持特定平台配置。
{{< /note >}}

**主题**
+ [设置文件](#mobile-project-settings-tool-settings-files)
+ [使用项目设置工具](#mobile-project-settings-tool-using)
+ [属性](#mobile-project-settings-tool-properties)

## 设置文件

**项目设置工具**可修改项目设置文件，这些文件位于各自的项目目录中。主设置文件[`project.json`](/docs/user-guide/project-config)位于每个项目的根目录中，包含 PC 和 Android 等平台的跨平台设置。该文件还包含 PC 和 Android 平台的特定设置。

您可以在`project_name\Root\Gem\Resources\PlatformLauncher\Info.plist`中找到 iOS 的项目设置。Plist 文件是一种特殊的 XML 格式，使用字典来存储属性。所有 plist 文件都有一些跨平台通用的属性，但都存储在每个单独的文件中。

有关 `.plist` 文件的更多信息，请参阅[关于Info.plist的键和值](https://developer.apple.com/library/archive/documentation/General/Reference/InfoPlistKeyReference/Introduction/Introduction.html) 和 [核心基础键](https://developer.apple.com/library/archive/documentation/General/Reference/InfoPlistKeyReference/Articles/CoreFoundationKeys.html)。

有关 iOS 特定设置的更多信息，请参阅 [iOS 键](https://developer.apple.com/library/archive/documentation/General/Reference/InfoPlistKeyReference/Articles/iPhoneOSKeys.html)。

## 使用项目设置工具

您可以使用**项目设置工具**设置与当前项目相关的设置。

**打开项目设置工具**

1. 在O3DE编辑器中，选择**File**, **Project Settings**, **Project Settings Tool**。

1. 在**项目设置工具**中，您可以查看和更改设置。有关详细信息，请暂停属性并查看工具提示。您还可以查看 [属性](#mobile-project-settings-tool-properties)。
   + 在**Base Settings**应用到所有平台。
   + 在**Platform Settings** 部分有用于特定平台设置的选项卡。

1. 如果没有保存更改，又想将设置重新载入磁盘（在设置文件中），请选择 **Reload**。

![The Project Settings Tool interface with Base Settings and Platform Settings.](/images/user-guide/mobile/mobile-project-settings-tool-ui.png)

### 图像预览

对于**Icons**和**Splashscreens**等图像设置，**项目设置工具**会使用要用于每个 dpi 或尺寸的图像显示图像预览。

![The Project Settings Tool displays image previews for image settings, such as for Splashscreens and Icons.](/images/user-guide/mobile/mobile-project-settings-tool-using-imagepreview.png)

### 验证

**项目设置工具**会在您输入数值时验证设置，并在所选设置的数值无效时提供反馈。

设置周围会出现红色轮廓，表示值无效。您还可以在设置上暂停，查看说明字段无效原因的错误信息。

![The Project Settings Tool displays a red outline and an error message for incompatible inputs on settings.](/images/user-guide/mobile/mobile-project-settings-tool-using-validation.png)

### 链接属性

您可以将类似的属性相互链接。当你将属性相互链接时，修改其中一个属性会对所有其他链接的属性做出相同的修改。

**使用链接属性**

1. 在**项目设置工具**中，导航到要链接的属性。可以链接的属性有一个链环图标。

1. 暂停链接图标，查看已链接的属性。如果所有相关属性的值相同，则从设置文件加载时会自动链接可以链接的属性。

1. 您可以启用或禁用任何链接。要启用链接，请单击图标。与之链接的所有属性都会以当前值更新。

   当一个属性的链接被启用并链接到一个有效属性时，链接图标会显示为橙色轮廓。

   ![The Bundle Identifier property is linked to the Android Package Name.](/images/user-guide/mobile/mobile-project-settings-tool-using-linked-properties-1.png)

   当属性链接断开或被禁用时，链接图标会显示断开且没有橙色轮廓。

   ![The Package Name property is linked to the iOS Bundle Identifier.](/images/user-guide/mobile/mobile-project-settings-tool-using-linked-properties-2.png)

1. 要禁用链接，请再次单击该图标。

{{< note >}}
有些属性始终是链接的，不能禁用，如 **Base Settings - Project Name** 和 **iOS - Bundle Name**。这可确保要求跨平台具有相同值的属性的准确性。如果**项目设置工具**发现始终链接的属性的设置文件之间存在差异，则以`project.json`值为准。
{{< /note >}}

### 重新配置项目

更改并保存后，** 项目设置工具** 会提示您重新配置项目。

!["For new settings to be applied the project must be reconfigured. Would you like to run configure now?"](/images/user-guide/mobile/mobile-project-settings-tool-using-reconfigure.png)

如果选择运行`configure` 命令，输出结果将显示在窗口底部。当结果显示 “Reconfiguration Finished（重新配置完成）”时，您可以使用**部署工具**将新更改部署到设备上。

{{< note >}}
如果您想立即部署更改，请选择**Yes**。O3DE Editor 不会自动运行`configure`命令，以后也不会提示您这样做。
{{< /note >}}

![The Project Settings Tool displays a successful configure and shows "Reconfiguration Finished."](/images/user-guide/mobile/mobile-project-settings-tool-using-reconfigure-result.png)

## 属性

请参阅**项目设置工具**中的以下属性。

### Base Settings 

**Base Settings** 属性同时适用于 Android 和 iOS。


****

| 属性 | 说明 |
| --- | --- |
| Project Name |  O3DE 用来标识所选项目的名称。此值不应更改。   |
| Product Name |  在可执行文件或应用程序标题中显示的名称。 |
| Executable Name |  运行项目的应用程序的文件名。该值不应更改。  |
| Game Folder |  存储项目所有代码和资源的目录名称。此值不应更改。  |
| Game Dll Name |  用于加载游戏的 DLL 文件名。此值不应更改。  |
| Output Folder |  打包项目构建完成后导出到的目录。  |

### Android 设置 

您可以在**Android**选项卡上的**Platforms Settings**中找到以下属性。

****

| 属性 | 说明 |
| --- | --- |
| Package Name |  Android 应用程序软件包标识符。用于生成特定于项目的 Java 活动类，并在 `AndroidManifest.xml`中使用。 必须采用点分隔格式。  |
| Version Name |  人类可读的版本号。用于设置 `AndroidManifest.xml` 中的 `android: versionName` 标记，并显示在 [Google Play](https://play.google.com/store/apps)中。  |
| Version Number |  内部应用程序版本号。用于设置 `AndroidManifest.xml`中的 `android:versionCode` 标记。 此值必须始终大于先前提交的版本号。否则，向 Google Play 提交将失败。  |
| Orientation |  Android 应用程序的预期方向。用于设置 `AndroidManifest.xml`中的 `android:screenOrientation` 标记。  |
| Public App Key |  Google Play 提供的应用程序许可证密钥。使用 APK 扩展文件或其他 Google Play 服务时需要。  |
| App Obfuscation Salt |  使用 APK 扩展文件时，用于（解除）混淆的特定应用程序盐值。  |
| Rc Job PAK Override |  RC job XML 文件的路径，用于覆盖发行版构建中使用的正常 PAK 文件生成。 路径必须相对于 `dev/Bin64/rc`。  |
| Rc Job APK Override |  RC job XML 文件的路径，用于覆盖发行版构建中使用的正常 APK 扩展文件生成。 路径必须相对于 `dev/Bin64/rc`。  |
| Use Main APK |  指定是否使用主 APK 扩展文件。 |
| Use Path PAK |  指定是否使用补丁APK 扩展文件。 |
| Enable Screen Wake Lock |  启用屏幕唤醒锁。如果启用，在应用程序运行时设备不会进入睡眠状态。  |
| Disable Immersive Mode |  禁用隐藏顶部和底部系统栏。 |
| Icons |  安卓系统的所有图标重载。所有未明确指定的重载均使用**Default**属性。必须使用 PNG 图像。 分辨率必须为 48px、72px、96px、144px 和 192px。  |
| Splashscreens |  Android 的所有闪屏（应用程序首次加载时显示的图像）重载。所有未明确指定的重载均使用**Default**属性。要求使用 PNG 图像。 分辨率值未严格执行，但推荐值为 1024 x 640、1280 x 800、1920 x 1200 和 2560 x 1600。  |

### iOS 设置 

您可以在**iOS**选项卡上的**Platforms Settings**中找到以下属性。



****
1
| 属性 | 说明 |
| --- | --- |
| Bundle Name |  iOS 用于识别捆绑包的内部名称。此值不应更改。  |
| Display Name |  应用商店中显示的用户可见的应用名称。 |
| Executable Name |  捆绑包生成的 XCode 项目的名称。该值不应更改。  |
| Bundle Identifier |  唯一标识 iOS 上的捆绑包。该值应采用反向 DNS 格式。  |
| Version Name |  在 [App Store](https://www.apple.com/ios/app-store/) 中显示的应用程序发布版本字符串。  |
| Version Number |  捆绑包的构建版本号字符串。 此值必须大于上次提交给 App Store 的版本。  |
| Development Region |  应用程序的默认语言和地区。由开发地区和开发过程中使用的主要语言定义。  |
| Requires Fullscreen |  指定应用程序是否需要以全屏模式运行。 |
| Hide Status Bar |  指定应用程序启动时是否初始化隐藏状态栏。  |
| Orientations |  选择要在 iOS 设备上启用的方向：   |
| Icons |  用于 iOS 的所有图标覆盖。需要 PNG 图像。所有分辨率必须与指定的完全一致。  |
| Launchscreens |  用于 iOS 的所有启动屏幕覆盖。需要 PNG 图像。所有分辨率必须与指定的完全一致。 |

### 覆盖图像

在 iOS 设备上，覆盖图像存储在`project_root\Gem\Resources\platformLauncher\Images.xcassets` 目录中。在该目录中，`AppIcon.appiconset` 目录包含图标，`LaunchImage.launchimage` 目录包含闪屏。

**示例**
下图显示了默认的 iOS 图标和闪屏。当你选择覆盖其中一个图像时，它会覆盖当前选定的图像。当你选择**Save**时，覆盖就完成了。此更改无法撤销。

![Default iOS icons.](/images/user-guide/mobile/mobile-project-settings-tool-properties-ios-override-icons.png)

![Default iOS splashscreens.](/images/user-guide/mobile/mobile-project-settings-tool-properties-ios-override-splashscreens.png)
