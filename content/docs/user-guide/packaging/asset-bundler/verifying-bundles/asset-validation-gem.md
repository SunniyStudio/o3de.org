---
description: ' 在 O3DE 中使用 Asset Validation Gem 的种子模式，以确保您的游戏资产已正确捆绑。 '
title: 使用 Asset Validation Gem 验证种子
weight: 200
---

在构建种子列表之后，但在捆绑之前，您可以使用 Asset Validation Gem 来验证资产加载是否映射回种子。Asset Validation Gem 将一组与种子相关的命令添加到 O3DE 控制台命令窗口。您可以使用这些命令来确保具有要捆绑的所有资产的种子。

其中一个命令 *seedmode 使种子模式处于活动状态。当种子模式处于活动状态且加载资产文件时，O3DE 会遍历新加载的资产的依赖关系图，直到找到资产的种子。如果找不到资产的种子，则会显示一条错误消息。它会警告您，如果您尝试使用为当前验证会话提供的种子进行捆绑，则不会捆绑加载的文件。

在开发过程中，使用种子模式来确保在添加资源时，它们被正确捆绑并包含在游戏的发布版本中。

{{< note >}}
如果您已经有要测试的捆绑包，则可以使用 bundle 模式而不是种子模式。有关详细信息，请参阅 [使用 Bundle Mode 测试捆绑包](/docs/user-guide/packaging/asset-bundler/verifying-bundles/bundle-mode).
{{< /note >}}

## 先决条件

使用 Project Manager 在游戏项目中启用 Asset Validation Gem。

## 种子模式命令

当您使用 O3DE 编辑器或启动器运行游戏时，可以使用以下控制台命令：
+ **seedmode** - 启用或禁用报告系统。
+ **addseedpath** *<资产的相对缓存路径>* - 将指定的资产及其所有依赖项作为种子添加到依赖项关系图中，以便它们不再报告为缺失。`addseedpath`命令允许您测试应该将哪些资源添加到您的种子列表中以进行打包。

  **示例**

  ```
  addseedpath levels\startergame\level.pak
  ```
+ **removeseedpath** *<资产的相对缓存路径>* - 从依赖关系图中删除资产。
+ **listknownassets** - 列出当前依赖项关系图中的所有资产。
+ **addseedlist** *<种子列表的相对源路径>* - 将在指定种子列表中找到的所有种子添加到已知的依赖项关系图中。由于该路径是源路径，因此它是相对于项目目录的。

  **示例**

  ```
  addseedlist Engine\SeedAssetList.seed
  ```
+ **removeseedlist** *<种子列表的相对源路径>* - 从图表中删除种子列表中的所有资产。
+ **printblacklisted** - 启用或禁用在系统中显示已批准的资产。某些资产（如着色器）是在运行时加载的，不会显示在依赖项关系图中。根据设计，着色器打包在其自己的 `.pak`文件中，不在依赖关系图中找到，也不需要由系统报告。但是，您可以使用 `printblacklisted`命令强制将着色器或其他已批准的资产类型包含在依赖关系图中。

## 使用种子模式

以下过程说明如何使用种子模式对缺少资源的关卡进行故障排除。

**要测试种子模式**

1. 在 O3DE 控制台中，输入命令 `seedmode`。资产验证开始。

   ```
   seedmode
   [CONSOLE] Executing console command 'seedmode'
   (AssetValidation) - Asset Validation is now on
   ```

1. 进入游戏模式。在控制台中，种子模式报告多个 Asset not found in seed graph 错误。

![Asset not found in seed graph errors in the O3DE console window.](/images/user-guide/assetbundler/asset-bundler-asset-validation-gem-1.png)

1. 退出游戏模式。

1. 输入`addseedpath`命令以添加缺少的资源文件。此示例使用命令 `addseedpath levels\milestone2\level.pak`.

1. 进入游戏模式。的 Asset not found 错误不再显示。

![Using the addseedpath command in the O3DE console window.](/images/user-guide/assetbundler/asset-bundler-asset-validation-gem-2.png)

### 处理缺失资源错误

如果种子模式报告缺少资产，则该资产可能是以下资产之一：
+ 您尚未添加到图表中的列表的一部分。
+ 必须添加为关卡中已找到的其他资产的依赖项的资产。
+ 本身必须是种子的资产。
