---
linkTitle: Bundling Project Assets for Release
title: Bundling Project Assets for a Project Game Release Layout
description: Learn how to manage your assets and package them into asset bundles in Open 3D Engine (O3DE).
weight: 150
---

本主题介绍了如何使用 **Asset Bundler** 为可分发的 *项目游戏发布布局* 捆绑资产。将项目资源管理到捆绑包中有助于减小项目在磁盘上的大小、优化项目的运行时，并组织资源以更轻松地发布将来的资源。准备分发项目时，请使用本主题中的说明来帮助您创建 Asset Bundle。


## 先决条件

使用`release` 配置构建您的项目。这将创建一个 *项目游戏发布布局*，其中包含您可以分发的应用程序和文件。了解如何 [创建适用于 Windows 的项目游戏发布布局](/docs/user-guide/packaging/windows-release-builds).


## 捆绑项目资产

本教程包含以下步骤：

1. 设置 Asset Bundler。
1. 创建游戏资源包。
1. 创建引擎资源包。
1. 将捆绑包添加到发布布局中。


在整个步骤中，将`<engine>` 替换为以下任一值：

- `C:\MyProject` -- 对于源代码引擎。
- `C:\o3de-install` -- 对于预构建的 SDK 引擎。


### 设置 Asset Bundler

要设置和运行 Asset Bundler，请执行以下操作：

1. 在您的 `<engine>` 目录中，使用 CMake 调用 Visual Studio 来构建 Asset Bundler。

    ```cmd
    cmake --build build/windows --target AssetBundler --config profile -- -m
    ```

    此命令包含以下选项：

    - `--target AssetBundler` -- 将构建目标设置为 Asset Bundler 和 Asset Bundler Batch 及其依赖模块。

    - `--config profile` -- 将 build configuration 设置为 profile，这将启用优化并允许调试。


1. 从 `<engine>\build\windows\bin\profile` 目录运行 `AssetBundler.exe`。这将打开带有图形用户界面 （GUI） 的 Asset Bundler。（或者，要使用命令行界面 （CLI），请运行`AssetBundlerBatch.exe`。

    现在，您应该打开了 Asset Bundler，在 GUI 中如下所示：

    {{< image-width "/images/user-guide/packaging/windows-release-build/asset-bundler-default-gui.png" "1000" "An annotated image of O3DE editor's user interface." >}}

<br></br>

{{< known-issue >}}
可能存在有关 “AssetBundler” 和 “AssetSeedManager” 的错误和警告，这些错误和警告在 Asset Bundler 的控制台中列出。您可以放心地忽略它们。
{{< /known-issue >}}


### 为游戏资源创建 bundle

*游戏资源包* 包含项目的关卡及其中的所有资源，例如对象、环境、材质等。在捆绑游戏资源时，捆绑游戏在其关卡中实际使用的资源才很重要。无需在项目目录中包含从未加载到项目中的资源。您可以使用 Asset Bundler 生成关卡所依赖的资源列表。这有助于确保生成的包文件处于最佳大小。


#### Create a new seed asset list

1. 在 Asset Bundler GUI 的 **Seeds** 选项卡上的 **Seed List file** 面板中，单击 **Create new Seed List file**。在此示例中，您可以将文件命名为`GameSeedList`。

1. 从 **Seed List files** 中选择`GameSeedList`文件。这应该突出显示整行，而不是启用复选框。

1. 在 **Product Assets** 面板中，单击 **+ Add Asset**，这将打开 **Add Seed Asset** 对话框。

1. 在平台列表中，选中 **pc** 复选框。

1. 单击 **Browse...** 打开文件资源管理器，然后浏览到`levels`文件夹。

1. 选择您的关卡`.spawnable`文件，然后按 **Open**。

1. 在 **Add Seed Asset** 对话框中，单击 **Add Seed**。

1. 在 Asset Bundler GUI 的 **Product Assets** 面板中，验证您的种子列表是否包含关卡资源。

1. 选中新种子列表文件的复选框，然后在主 **File** 菜单中选择 **Save**。

#### 生成资产列表

1. 在 Asset Bundler GUI 中，从列表中选中新种子列表文件的复选框。

1. 单击 **Generate Asset Lists**，这将打开 **Generate Asset List Files** 对话框。

1. 在平台列表中，选中 **pc** 复选框。

1. 单击 **Browse...**，这将打开文件资源管理器。

1. 在 File Explorer（文件资源管理器）中，输入资产列表的名称，然后单击 **Save**（保存）**。对于此示例，请使用名称 `game.assetlist`。

1. 在 **Generate Asset List Files** 对话框中按 **Create New File**。

1. 在 Asset Bundler GUI 中，导航到 **Asset Lists** 选项卡以验证`game.assetlist`中的资产。资产列在 **Asset List** 面板中。

#### 捆绑您的资产

1. 在 Asset Bundler GUI 的 **Asset Lists** 选项卡上的 **Asset List Files** 面板中，选择您的资源列表(`game.assetlist`)。

1. 单击 **Generate Bundle**，这将打开 **Generate Bundle** 对话框。

1. 单击 Output Bundle Name（输出包名称）字段旁边的 **Browse...** 按钮，选择“Output Bundle Name”（输出包名称），这将打开文件资源管理器。您可以将其他默认选项保留原样。

1. 在文件资源管理器中，输入包文件的名称，然后单击 **Open**。对于此示例，请使用名称 `game.pak`。

1. 将其他默认选项保留原样，然后单击 **Generate Bundles**。

对于 Windows，当 Asset Bundler 保存您的捆绑包时，它会将“_pc”附加到捆绑包的名称中。因此，您现在应该有一个资源包`game_pc.pak`。

### 为引擎资源创建捆绑包

接下来，为项目的插件资源创建一个捆绑包。*引擎资源包* 包含加载和运行 Game Launcher 所需的基本文件。

#### 从默认种子列表生成资产列表

1. 在 Asset Bundler GUI 中，导航到 **Seeds** 选项卡。

1. 在 **种子列表文件** 面板的底部，选中 **Default Seed Lists** 复选框。如果 `GameSeedList` 文件是从前面的步骤中选择的，请确保取消选择该文件。

1. 单击 **Generate Asset Lists**，这将打开 **Generate Asset List files** 对话框。

1. 在平台列表中，选中 **pc** 复选框。

1. 单击 **Browse...**，这将打开文件资源管理器。

1. 在 File Explorer（文件资源管理器）中，输入资产列表的名称，然后单击 **Save**。在此示例中，我们将使用名称 `engine.assetlist`。

1. 在 Asset Bundler GUI 中，导航到 **Asset Lists** 选项卡以验证`engine.assetlist`中的资产。资产列在 **Asset List** 面板中。

#### 捆绑您的资产

1. 在 Asset Bundler GUI 的 **Asset Lists** 选项卡上的 **Asset List Files** 面板中，选择`engine.assetlist`文件。

1. 单击 **Generate Bundle**，这将打开 **Generate Bundle** 对话框。

1. 单击 Output Bundle Name（输出包名称）字段旁边的 **Browse...** 按钮，选择“Output Bundle Name”（输出包名称），这将打开文件资源管理器。

1. 在文件资源管理器中，输入包文件的名称，然后单击 **打开**。对于此示例，请使用名称 `engine.pak`。

1. 将其他默认选项保留原样，然后单击 **Generate Bundles**。

对于 Windows，当 Asset Bundler 保存您的捆绑包时，它会将`_pc`附加到捆绑包的名称中。因此，您现在应该有一个资源包`engine_pc.pak`。您可以在 **Completed Bundles list** 下查看您的捆绑包。

### 将捆绑包添加到项目游戏发布布局

下一步，添加 `game_pc.pak` 和 `engine_pc.pak` 文件到您的项目游戏版本布局中，以便 Game Launcher 可以从这些捆绑包加载资产。

1. 浏览至 `<install>\bin\Windows\release\<build>\Cache\pc` 目录中，其中包含项目 Game Release 布局目录中的捆绑内容。例如，默认路径可以是：

   - `C:\MyProject\install\bin\Windows\release\Default\Cache\pc` -- 对于非整体式构建。
   
   - `C:\MyProject\install\bin\Windows\release\Monolithic\Cache\pc` -- 对于整体式构建。

2. 你可能需要删除在创建项目游戏发布布局时自动创建的`engine.pak` 文件。您不再需要它，因为您创建了新的捆绑包。

3. 将新捆绑包`game_pc.pak` 和 `engine_pc.pak`添加到此目录。

现在，您可以运行项目的 Game Launcher，它将使用您的新捆绑包！


## 最后说明

在本教程中，您学习了如何捆绑项目资源，建议将其用于发布版本，因为它可以在您分发项目之前优化项目。将资源管理到两个捆绑包（游戏和引擎资源）中是一种方法。在实践中，您的团队可能会选择将项目资源管理到不同的捆绑包集中。


## 相关主题

|主题 |描述 |
| - | - |
| [创建适用于 Windows 的项目游戏发布布局](/docs/user-guide/packaging/windows-release-builds) | 了解如何创建适用于 Windows 的项目游戏发布布局。 |
