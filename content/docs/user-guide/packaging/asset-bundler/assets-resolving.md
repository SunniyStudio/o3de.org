---
description: ' 解决 O3DE 游戏项目中缺少的资源。 '
title: 解决缺失资产
weight: 200
---

在构建和打包 O3DE 游戏后，您需要经常验证您的包是否包含它们需要的所有资产。有关验证 Asset Bundle 的信息，请参阅 [验证 Bundle 是否包含必需的资源](/docs/user-guide/packaging/asset-bundler/verifying-bundles/).

当您确定可能缺少的资源时，您需要将其包含在内，以便下一个捆绑游戏包中不再缺少该资源。

捆绑包中缺少的资源可能是以下资产之一：
+ **缺少产品依赖项** - 引用其他资产的资产，但未将其声明为产品依赖项，或者在 [资产列表比较过程中](/docs/user-guide/packaging/asset-bundler/list-operations/) 删除了引用的资产。
+ **硬编码文件加载** - 在 C++ 中按路径或资产 ID 加载资产。
+ **误报** - 资源似乎在捆绑包中缺失，但实际上并未使用。例如，您可能有一个似乎缺少的仅限编辑器的资源，该资源从未在游戏的启动器中加载或使用。

要解决缺少的资产，请依次检查每种可能性。

## 缺少产品依赖项

缺少的资产可能已作为来自 O3DE 的引用或您的游戏代码与其他资产的交互加载。在这些情况下，您可以通过发出新的 product 依赖项来解决问题。

### 查找 Asset 引用

要查找资产引用的源，请尝试以下方法：
<!-- 
Missing topic. 

+ Use the Asset Processor Batch's [missing dependency scanner](/docs/user-guide/packaging/asset-bundler/verifying-bundles/missing-dependency-scanner/). -->
+ 使用以下方法调试文件加载：
  + 设置断点（如果可能）
  + 添加额外的 `print` 命令
  + 尝试以不同的方式与游戏交互，以查看是什么触发了缺失资产的加载。

作为开始调试位置的提示，请注意，大多数文件加载都会通过[`AzFramework::IO::Archive`](/docs/api/frameworks/azframework/class_a_z_1_1_i_o_1_1_archive.html) 进行路由。设置断点或添加额外的调试逻辑 \(如 `printf` 语句\) 可以帮助确定触发您正在调查的资产负载的原因。

### 查找要更新的生成器

跟踪生成引用缺失资产的资产的作业。然后，您可以更新生成的资产，以将缺少的资产作为产品依赖项发出。如果生成的资产具有与已知生成器关联的文件扩展名，则生成的资产的来源可能很明显。

如果生成的资产的来源不明显，则可以使用 asset database 来查找信息。

**查找生成资产的作业**

1. 在 **Products** 表中，搜索要更新以发出依赖项的资产。以下示例搜索材质使用 [DB Browser for SQLite](https://sqlitebrowser.org/) 来浏览资源数据库：

![Searching for an asset in the Products table.](/images/user-guide/assetbundler/asset-bundler-assets-resolving-1.png)

1. 在 **Jobs** 表中查找 `JobPK` 值，如以下示例所示：

![Looking up a value in the asset database Jobs table.](/images/user-guide/assetbundler/asset-bundler-assets-resolving-2.png)

1. 在代码中查找 `JobKey` 或 `BuilderGuid`。

### 更新 Builder 以发出依赖项

确定发出缺少依赖项的产品的生成器后，请更新生成器。有关详细信息，请参阅 [声明产品依赖项](/docs/user-guide/packaging/asset-bundler/overview/#why-use-product-dependencies).

## 硬编码文件加载

要解决硬编码文件加载中缺少的资源问题，请查找文件在代码中的加载位置，然后选择解决策略。

### 查找文件加载

我们建议使用以下技术来跟踪硬编码文件加载：

1. 在代码中使用断点。

2. 向代码库添加其他日志消息。

3. 执行“在文件中查找”搜索引用缺失资产的字符串。

有关更多信息，请参阅本主题前面的 [查找资产引用](#asset-bundler-assets-resolving-finding-the-asset-reference)。

### 解决缺失的资源

要从硬编码文件加载中解决缺少的资源，请尝试以下选项：
+ **删除硬编码加载** - 通过将资产作为产品依赖项从相关构建器发出，您可以使用文件较少且更易于维护的种子列表。
+ **添加为种子** - 如果您不能或不想替换硬编码的资产加载，则可以将引用的文件作为种子添加到游戏的种子列表中。由于添加种子只会更改数据，不需要重新编译游戏，因此此方法在开发后期可能很有用，并且可以最大限度地减少代码更改。有关将引用的文件作为种子添加到游戏的种子列表的信息，请参阅 [O3DE Asset Bundler 命令行工具参考](/docs/user-guide/packaging/asset-bundler/command-line-reference/).
+ **使用通配符依赖关系系统** - 如果您的项目使用相对路径加载或通配符路径加载，则可以在依赖关系文件中声明依赖关系。此技术将在下一节中介绍。

## 误报

在开发过程中，某些资源和资源引用仅在编辑器或启动器中使用。这些资产不用于发布版本。因此，您可以将未在发布版本中使用的捆绑包中缺少的任何资产视为误报。

### 从缺失的资产扫描结果中删除误报

在验证发布版本中未使用资产后，您可以使用文件标记系统对其进行标记，以便它不会显示在将来的扫描中。有关详细信息，请参阅 [使用文件标记系统包含或排除资产](/docs/user-guide/packaging/asset-bundler/file-tagging/)。
