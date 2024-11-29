---
description: ' 了解使用 Open 3D Engine 捆绑资产时使用的比较操作。'
title: '资产列表比较操作'
weight: 500
---

资源列表比较是提供给`AssetBundlerBatch.exe`工具的规则，用于确定应在最终捆绑包资源列表中包含或排除哪些文件。资源列表文件具有后缀`.assetlist`，并包含资源文件的路径和名称的简单列表。

{{< note >}}
虽然比较运算使用集合论中的术语，但它们与实际的集合运算并不完全相同。对于 [delta comparison 操作](#asset-list-delta-comparison-operation)尤其如此，如果其中一个文件在第二组中被修改，则在两个集中都包含一个文件条目。此外，文件模式匹配不是 set 操作。
{{< /note >}}

## 资产列表 Delta 比较操作

此操作需要两个资源列表文件来为仅包含您需要在发布包中提供的资源的捆绑包创建一个资源列表。使用此操作可创建用于增量更新的捆绑包，例如级别的增量补丁。要使用此操作，请运行AssetBundlerBatch.exe，并将 `--comparisonType` 参数值设置为 `0` 或 `delta`。

要执行增量比较操作，请打开命令提示符，然后运行以下命令：

```
AssetBundlerBatch.exe compare ^
--comparisonType delta ^
--firstAssetFile firstAssetList_pc.assetlist ^
--secondAssetFile secondAssetList_pc.assetlist ^
--output deltaAssetList.assetlist
```

下图显示了 delta 比较操作。

![Diagram showing the inputs and results of a delta comparison operation.](/images/user-guide/assetbundler/delta-comparison-operator.png)

## 资产列表联合比较操作

此操作需要两个资源列表文件来为组合两个列表中的所有资源的捆绑包创建一个资源列表。它仅包含输出资源列表中文件的修改版本，而不包括第一个资源列表中的原始文件。当您有两个不再需要单独且应合并为单个捆绑包的捆绑包时，请使用此操作。 要使用此操作，请运行AssetBundlerBatch.exe，并将`--comparisonType`参数值设置为`1` 或 `union`。

要执行联合比较操作，请打开命令提示符，然后运行以下命令：

```
AssetBundlerBatch.exe compare ^
--comparisonType union ^
--firstAssetFile firstAssetList_pc.assetlist ^
--secondAssetFile secondAssetList_pc.assetlist ^
--output unionAssetList.assetlist
```

下图显示了联合比较操作。

![Diagram showing the inputs and results of a union comparison operation.](/images/user-guide/assetbundler/union-comparison-operator.png)

## 资产列表交集比较操作

此操作需要两个资源列表文件来创建一个捆绑资源列表，其中仅包含两个资源列表中的项目。要使用此操作，请运行AssetBundlerBatch.exe，并将`--comparisonType`参数值设置为`2` 或 `intersection`。

要执行交叉点比较操作，请打开命令提示符并运行以下命令：

```
AssetBundlerBatch.exe compare ^
--comparisonType intersection ^
--firstAssetFile firstAssetList_pc.assetlist ^
--secondAssetFile secondAssetList_pc.assetlist ^
--output intersectionAssetList.assetlist
```

下图显示了交集比较操作。

![Diagram showing the inputs and results of a intersection comparison operation.](/images/user-guide/assetbundler/intersection-comparison-operator.png)

## 资产列表交集计数比较操作

此操作需要任意数量的资源文件来创建捆绑包资源列表，其中仅包含在所有资源列表中出现给定次数的项目。此比较类型不能用作一系列比较规则的一部分，并且需要使用 `--intersectionCount`参数。要使用此操作，请运行`--comparisonType`值为 `5` 或 `intersectionCount` 的 AssetBundlerBatch.exe。

要执行交叉点计数比较操作，请打开命令提示符并运行以下命令：

```
AssetBundlerBatch.exe intersectionCount ^
--comparisonType intersectionCount ^
--intersectionCount 3 ^
--firstAssetFile firstAssetList_pc.assetlist,secondAssetList_pc.assetlist,thirdAssetList_pc.assetlist ^
--output intersectionCountAssetList.assetlist
```

下图显示了交集比较操作。

![Diagram showing the inputs and results of a intersection count comparison operation.](/images/user-guide/assetbundler/intersection-comparison-count-operator.png)

## 资产列表补码比较操作

此操作需要两个资源列表文件来创建一个捆绑资源列表，其中每个项目都位于第二个资源列表中，但不在第一个列表中。它的工作方式类似于 delta 比较，不同之处在于它不检查文件哈希，并且不会包含两个列表中的文件的修改版本。要使用此操作，请运行`--comparisonType`值为 `3` 或 `complement` 的 AssetBundlerBatch.exe。

要执行补码比较操作，请打开命令提示符，然后运行以下命令：

```
AssetBundlerBatch.exe compare ^
--comparisonType complement ^
--firstAssetFile firstAssetList_pc.assetlist ^
--secondAssetFile secondAssetList_pc.assetlist ^
--output complementAssetList.assetlist
```

下图显示了交集比较操作。

![Diagram showing the inputs and results of a intersection comparison operation.](/images/user-guide/assetbundler/complement-comparison-operator.png)

## 资产列表文件模式操作

此操作需要一个资产列表文件和一个要应用的文件模式。在资源列表中与此模式匹配的任何文件都将包含在输出资源列表中。要使用此操作，请运行`--comparisonType`值为 `4` 或 `filepattern` 的 AssetBundlerBatch.exe。

要执行文件模式比较操作，请打开命令提示符并运行以下命令：

```
AssetBundlerBatch.exe compare ^
--comparisonType filepattern ^
--filePatternType 0 ^
--filePattern "*.xml" ^
--firstAssetFile assetList_pc.assetlist ^
--output filePatternAssetList.assetlist
```

{{< note >}}
前面的命令查找具有`.xml`后缀的文件以供包含。您可以将其替换为要用于比较的任何基于通配符或正则表达式的文件模式。
{{< /note >}}

下图显示了文件模式比较操作。

![Diagram showing the inputs and results of a file pattern comparison operation.](/images/user-guide/assetbundler/file-pattern-operation.png)

## 如何执行多个资产列表比较操作

以下指南显示了为仅包含修改和更新的文本\(`.txt`\) 文件内容的游戏补丁创建资产列表的过程。在此过程中，它还会根据作为交集比较结果生成的包含列表来验证资产列表内容。

下图显示了此示例的比较过程和输出。

![Diagram that shows the use of multiple comparison rules to include only specific assets in your release bundle.](/images/user-guide/assetbundler/patch-bundle-example.png)

### 先决条件

要完成本教程中的过程，请确保您已进行以下设置：
+ 已安装和配置的 Open 3D Engine 版本。
+ 一个 O3DE 项目，准备好构建和编译。

### 设置

创建您将在本教程中使用的文件。

1. 在游戏项目的根文件夹中，创建以下空文件：
   + `FileA.txt`
   + `FileB.txt`
   + `FileC.txt`
   + `FileD.txt`
   + `FileE.cfg`
   + `FileF.cfg`
   + `do-not-add-me.txt`

1. 通过运行 [Asset Processor](/docs/user-guide/assets/asset-processor/).

### 为比较操作创建资产列表文件

创建要在比较中使用的`.seed` 和 `.assetlist`文件。

1. 打开命令提示符，导航到输出资产列表文件的目录，然后运行以下命令以创建初始种子列表：

   ```
   AssetBundlerBatch.exe seeds ^
   --addSeed FileA.txt,FileB.txt,FileC.txt,FileD.txt,FileE.cfg,FileF.cfg ^
   --seedListFile include-list.seed
   ```

1. 运行以下命令以从种子列表创建资产列表：

   ```
   AssetBundlerBatch.exe assetlists ^
   --seedListFile include-list.seed ^
   --assetListFile include-list.assetlist
   ```

1. 运行以下命令以创建 v1 种子文件：

   ```
   AssetBundlerBatch.exe seeds ^
   --addSeed FileA.txt,FileB.txt ^
   --seedListFile mygame_v1.seed
   ```

1. 运行以下命令以创建 v1 资产列表文件：

   ```
   AssetBundlerBatch.exe assetlists ^
   --seedListFile mygame_v1.seed ^
   --assetListFile mygame_v1.assetlist
   ```

1. 运行以下命令以创建 v2 种子文件：

   ```
   AssetBundlerBatch.exe seeds ^
   --addSeed FileA.txt,FileB.txt,FileC.txt,fileE.cfg,fileF.cfg,do-not-add-me.txt ^
   --seedListFile mygame_v2.seed
   ```

1. 运行以下命令以创建 v2 资产列表文件：

   ```
   AssetBundlerBatch.exe assetlists ^
   --seedListFile mygame_v2.seed ^
   --assetListFile mygame_v2.assetlist --print
   ```

最后一个命令应生成如下所示的输出，由 --print 标志启用：

```
Printing assets for Platform ( pc ):
- filea.txt
- fileb.txt
- filec.txt
- filee.cfg
- filef.cfg
- do-not-add-me.txt
Total number of assets for Platform ( pc ): 6.
```

现在，您已经创建了`.seed` 和 `.assetlist`文件，您需要启动比较操作并组合最终的捆绑包资产列表。

{{< note >}}
此时，您可以选择接下来的两个步骤中的任何一个来完成本教程。要单独运行每个比较命令，请参阅 1.6.4 节中的步骤。要在一个命令中运行所有比较操作，请参阅第 1.6.5 节中的步骤。
{{< /note >}}

### 运行各个步骤比较命令

1. 打开命令提示符并导航到输出资产列表文件的目录。运行以下命令：

   ```
   AssetBundlerBatch.exe compare ^
   --comparisonType delta ^
   --firstAssetFile  mygame_v1_pc.assetlist ^
   --secondAssetFile mygame_v2_pc.assetlist ^
   --output multistep_delta.assetlist ^
   --print
   ```

   在此步骤中，v1 资产列表中的文件`fileA.txt` 和 `fileB.txt`将根据`comparisonType 0`（增量比较类型）删除。您的命令输出应如下所示：

   ```
   Printing assets from the comparison result multistep_delta_pc.assetlist.
   ------------------------------------------
   - filee.cfg
   - do-not-add-me.txt
   - filec.txt
   - filef.cfg
   Total number of assets (4).
   ---------------------------
   Saving results of comparison operation...
   Save successful!
   ```

1. 通过从提示符运行以下命令，从资产列表中删除不在包含列表中的任何文件：

   ```
   AssetBundlerBatch.exe compare ^
   --comparisonType intersection ^
   --firstAssetFile include_pc.assetlist ^
   --secondAssetFile multistep_delta_pc.assetlist ^
   --output multistep_include.assetlist ^
   --print
   ```

   对于此步骤，文件 `do-not-add-me.txt` 被删除，因为它不在生成的包含列表中，基于指定的交集比较\(`comparisonType 2`\)。您的命令输出应如下所示：

   ```
   Printing assets from the comparison result multistep_include_pc.assetlist.
   ------------------------------------------
   - filec.txt
   - filee.cfg
   - filef.cfg
   Total number of assets (3).
   ---------------------------
   Saving results of comparison operation...
   Save successful!
   ```

1. 运行此命令以从资产列表中删除任何不是文本\(`.txt`\) 文件的内容：

   ```
   AssetBundlerBatch.exe compare ^
   --comparisonType filepattern ^
   --filePatternType 0 ^
   --filePattern *.txt ^
   --firstAssetFile multistep_include_pc.assetlist ^
   --output multistep_filepattern.assetlist ^
   --print
   ```

   在此步骤中，根据文件模式比较 \(`comparisonType 4`\) 删除两个 `.cfg` 文件，因为它们与文件模式不匹配。您的命令输出应如下所示：

   ```
   Printing assets from the comparison result multistep_filepattern_pc.assetlist.
   ------------------------------------------
   - filec.txt
   Total number of assets (1).
   ---------------------------
   Saving results of comparison operation...
   Save successful!
   ```

### 运行多步比较命令

您也可以在单个命令中执行所有这些命令。多步骤命令等效于单独运行前面步骤中的所有命令，并使用您指定的变量来存储比较的中间结果。

在此示例中，多步比较命令按顺序运行 3 次比较，并将结果存储在后续步骤中使用的变量中。v1 和 v2 之间的增量比较存储在 `$delta_all`变量中，然后对包含列表与资产列表`$delta_all`运行交集比较，并将结果存储在 `$delta_include` 变量中。最后，对存储在`$delta_include`变量中的所有文本文件运行文件模式比较。此最终命令的输出存储在文件 `mygame_v1tov2_patch.assetlist`中。

要尝试此方法，请打开命令提示符，然后导航到输出资产列表文件的目录。运行以下命令：

```
AssetBundlerBatch.exe compare ^
--comparisonType delta,intersection,filepattern ^
--filePatternType 0 ^
--filePattern *.txt ^
--firstAssetFile mygame_v1_pc.assetlist,include_pc.assetlist,$delta_include ^
--secondAssetFile mygame_v2_pc.assetlist,$delta_all ^
--output $delta_all,$delta_include,mygame_v1tov2_patch.assetlist ^
--print
```

该命令应生成如下所示的输出：

```
Printing assets from the comparison result mygame_v1tov2_patch_pc.assetlist.
------------------------------------------
- filec.txt
Total number of assets (1).
---------------------------
Saving results of comparison operation...
Save successful!
```

您还可以通过在文本编辑器中打开`mygame_v1tov2_patch.assetlist` 并检查它是否仅包含您希望看到的文件来确认操作是否成功。

### 多重比较的工作原理

运行多个步骤命令时，请为每个相关参数使用逗号分隔的列表。命令中的各个步骤与它们在此逗号分隔的参数值列表中的位置相匹配。

前面的示例使用了三个比较运算。前两个比较引用第一个和第二个资产列表文件，最后一个比较引用文件模式和第一个资产文件。分解为多个组成部分的 multistep 命令如下所示：


**分解单个多重比较命令的流程**

|命令参数 |第 1 步 |步骤 2 |第 3 步 |
| --- | --- | --- | --- |
| comparisonType | delta | intersection | filepattern |
| firstAssetFile | mygame\_v1\_pc.assetlist | include\_pc.assetlist (temp file) | $delta\_include |
| secondAssetFile | mygame\_v2\_pc.assetlist | $delta\_all | N/A |
| filePatternType | N/A | N/A | Wildcard (parameter value 0) |
| filePattern | N/A | N/A | \*.txt |
| output | $delta\_all | $delta\_include | mygame\_v1tov2\_patch.assetlist |

水平阅读此表，查看在流程的每个步骤中提供给每个比较的数据。垂直阅读此表，查看每个步骤的每个命令使用的参数。
