---
linkTitle: 内容补丁包
title: 创建内容补丁更新包
description: 了解如何为已发布的项目创建 Open 3D Engine （O3DE） 内容更新补丁包。
toc: true
weight: 300
---

本主题介绍使用 Asset Bundler 为 Open 3D Engine （O3DE） 项目创建内容补丁更新包的过程。这包括一个仅包含资源更新的补丁包，以及一个包含所有游戏资源（包括资源更新）的修补发布包。

## 先决条件

若要继续操作，您需要一个项目，其中包含已通过上一主题 [创建适用于 Windows 的项目游戏版本布局](windows-release-builds) 中的步骤创建的发布版本。

## 本主题中使用的约定

为清楚起见，本主题对项目进行了一些假设：

* 您的项目有一个种子列表文件和一个仅限游戏的资源列表。
* 您正在项目中使用 Source Engine 版本。

本主题使用以下约定来替换您自己的目录和文件名：

* 项目名称和目录： `C:\MyProject`
* Source engine build 目录下： `C:\MyProject\build\windows\bin\profile`
* 游戏资产种子列表： `game_seed_list.seed`
* 游戏资产列表： `game_v1.assetlist`

## 对项目进行资产更改

对项目进行视觉更改以模拟内容更新。出于演示目的，您可以将资源添加到测试更新捆绑包时易于发现的关卡。将更改保存到您的关卡中。

## 创建更新包

创建更新包有四个步骤：

* 确保所有资产都已处理完毕。
* 根据更改生成新的资产列表。
* 创建包含所有游戏资源和更新的修补版资源包。
* 创建仅包含游戏资源更新的补丁资源包。

## 处理所有项目资产

对于大多数更新，只有游戏资产会发生变化。升级到新版本的 O3DE 并推送更新时，您应该重新生成所有现有内容以确保其正确更新，包括引擎资产和辅助内容。

要处理资产，请运行 **Asset Processor** 或 **Asset Processor Batch**，按照 Windows 版本布局主题的 [处理资产](./windows-release-builds/#process-assets)部分中的步骤进行操作。

### 创建修补的发布资产列表

创建一个新版本的游戏资源列表，其中包含原始`game_v1.assetlist`中的所有资源以及您保存到关卡中的更改。此新资源列表用作修补版本的资产列表，并在下一节中用于为项目的现有发行版生成补丁。

{{< tabs name="Patched release asset list" >}}
{{% tab name="Asset Bundler GUI" %}}

1. 从源代码引擎build目录运行 `AssetBundlerBatch.exe`。
1. 在 **Seeds** 选项卡中，通过选中左侧的框来选择 `game_seed_list`。列表中的种子将显示在主列表视图中。
1. 在右上角选择 **Generate Asset Lists** 。
1. 在 **Generate Asset List Files** 对话框中，选择 **Browse...**。
1. 在新建文件对话框中，输入 `game_v2.assetlist` 作为名称，点击 **Open**。
1. 在 **Generate Asset List Files** 对话框中，从平台列表中选择 **pc** 。
1. 选择 **Create New File**。

{{% /tab %}}
{{% tab name="Asset Bundler command" %}}

或者，您可以运行此命令来为修补后的版本生成资产列表。

```cmd
AssetBundlerBatch.exe assetLists --seedListFile game_seed_list.seed ^
    --assetListFile game_v2.assetlist
```

{{% /tab %}}
{{% /tabs %}}

片刻之后，将出现一个对话框，通知您已成功创建资产列表。您可以通过选择 **Asset List** 选项卡，从 **Asset list files** 中选择`game_v2`，然后检查主列表窗口中的内容来验证您的更改是否已添加到新的修补的发布资源列表中。

{{< note >}}
您不需要更新 `game_seed_list.seed` 文件，因为它已经包含对您更新的关卡的引用。生成资产列表时，将包含对关卡依赖项的更改。
{{< /note >}}

### 创建补丁资产列表

补丁资产列表应用于现有分配，并且仅包含新的和更新的资产。要创建补丁，您需要将 v2 列表与 v1 列表进行比较，并将增量保存到新的资产列表中。

{{< tabs name="Patch asset list" >}}
{{% tab name="Asset Bundler GUI" %}}

9. 在 Asset Bundler中，选择 **Rules** 选项卡。
10. 选择 **Create new Rules file**.
11. 在文件对话框中，输入 `generate_v1_to_v2_patch.rules` 作为文件名，然后点击 **Open**。
12. 从**Rules**列表选择 `generate_v1_to_v2_patch` 。
13. 从**Comparison Steps**列表选择 **Add Step** 。
14. 为新步骤输入名称，如`Generate V1 to V2 Delta`。
15. 设置 **Comparison Type** 为 `Delta`。
16. 点击在 **Input A** 下的 **Browse...** 选择 `game_v1.assetlist`。
17. 点击在 **Input B** 下的 **Browse...** 选择 `game_v2.assetlist`。
18. 点击 **Run Selected Rule**。
19. 在 **Run Rule** 对话框中，点击 **Browse...**。
20. 在新建文件对话框中，输入 `game_v1_to_v2_patch.assetlist` 作为名称，点击 **Open**。
21. 在 **Generate Asset List Files** 对话框中，为平台列表中选择 **pc**。
22. 点击 **Create New File**。

{{% /tab %}}
{{% tab name="Asset Bundler command" %}}

或者，您也可以运行此命令来生成补丁资产列表。

```cmd
AssetBundlerBatch.exe compare --comparisonType delta ^
    --firstAssetFile game_v1.assetlist ^
    --secondAssetFile game_v2.assetlist ^
    --output game_v1_to_v2_patch.assetlist
```

{{% /tab %}}
{{% /tabs %}}

片刻之后，将出现一个对话框，通知您已成功创建资产列表。您可以通过选择 **Asset List** 选项卡，从 **Asset list files** 中选择`game_v1_to_v2_patch`，然后在主列表窗口中检查内容来验证补丁资源列表是否仅包含新更改。

### 创建 Asset Bundle

现在，您必须为新资产列表创建资产包（.pak 文件）。

{{< tabs name="Create asset bundles" >}}
{{% tab name="Asset Bundler GUI" %}}

要生成修补后的发布资源包，请执行以下操作：

23. 在 **Asset Lists** 选项卡中，选择 `game_v2` 资产列表。
24. 点击 **Generate Bundle**。
25. 在 **Generate Bundles** 对话框中，点击 **Browse...**。
26. 在文件对话框中，输入 `game_v2.pak` 作为文件名，点击 **Open**。
27. 点击 **Generate Bundles**.

要生成补丁资源包，请执行以下操作：

28. 在 **Asset Lists** 选项卡中，选择 `game_v1_to_v2_patch` 资产列表。
29. 点击 **Generate Bundle**。
30. 在 **Generate Bundles** 对话框中，点击 **Browse...**。
31. 在文件对话框中，输入 `game_v1_to_v2_patch.pak` 作为文件名，点击 **Open**。
32. 点击 **Generate Bundles**。

{{% /tab %}}
{{% tab name="Asset Bundler command" %}}

或者，您也可以运行这些命令以从资产列表生成捆绑包。

使用以下命令生成包含所有游戏资产的修补发布包：

```cmd
AssetBundlerBatch.exe bundles --assetListFile game_v2.assetlist ^
    --outputBundlePath game_v2.pak
```

使用以下命令生成仅包含已更改资产的补丁包：

```cmd
AssetBundlerBatch.exe bundles --assetListFile game_v1_to_v2_patch.assetlist ^
    --outputBundlePath game_v1_to_v2_patch.pak
```

{{% /tab %}}
{{% /tabs %}}

在下一部分中，您将使用项目的发布版本测试新捆绑包。

## 测试 Asset Bundle

您可以通过模拟两个用户场景来测试 Asset Bundle。

由于您的发布版本当前具有原始 v1 资源包，因此请首先测试用户安装了您的项目并应用仅包含更新资源的补丁资源包的场景。将补丁资源包复制到您的发布资源目录。

```cmd
copy C:\MyProject\AssetBundling\Bundles\game_v1_to_v2_patch.pak ^
    C:\MyProject\install\bin\Windows\release\Monolithic\Cache\pc\
```

启动发布版本并检查您的更新是否显示在关卡中。

```cmd
C:\MyProject\install\bin\Windows\release\Monolithic\MyProject.GameLauncher.exe
```

如果内容补丁已成功应用，您将在关卡中看到更新的内容。

现在，您可以模拟用户下载已应用内容补丁的新版本。

从游戏版本中删除补丁资源包和 v1 资源包：

```cmd
del C:\MyProject\install\bin\Windows\release\Monolithic\Cache\pc\game_v1_to_v2_patch.pak
del C:\MyProject\install\bin\Windows\release\Monolithic\Cache\pc\game_v1.pak
```

将 v2 资源包复制到发行版：

```cmd
copy C:\MyProject\AssetBundling\Bundles\game_v2.pak ^
    C:\MyProject\install\bin\Windows\release\Monolithic\Cache\pc\
```

重新启动发布版本，并检查您的更新是否显示在关卡中：

```cmd
C:\MyProject\install\bin\Windows\release\Monolithic\MyProject.GameLauncher.exe
```

## 结论

您已经学习了如何生成内容补丁以应用于现有版本。您可以使用这些步骤通过项目的各个版本创建任意数量的更新。
