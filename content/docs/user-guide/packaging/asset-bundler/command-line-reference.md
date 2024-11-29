---
linktitle: CLI 参考
title: Asset Bundler 命令行工具参考
description: Open 3D Engine Asset Bundler 命令行工具 AssetBundlerBatch 的命令参考。此参考涵盖可用命令、其选项和基本使用案例。
weight: 800
---

Open 3D Engine Asset Bundler 由名为`AssetBundlerBatch`的命令行工具驱动。此工具管理种子列表、资源列表、比较和资源包。资源打包器**不是**用于将资源编译为打包器用于分发的格式的 - 这是 Asset Processor 的角色。在运行 Asset Bundler 之前，请确保：
+  启用应捆绑资产的每个平台。通过编辑项目中的`dev/AssetProcessorPlatformConfig.ini` 文件来管理启用的平台。
+ 运行 Asset Processor 以确保资产及其元数据是最新的。

查看 [Asset Bundler 概念和术语](/docs/user-guide/packaging/asset-bundler/concepts/) 或 [术语](/docs/user-guide/appendix/glossary/)了解有关此参考中使用的术语的定义。

## 一般用途

`AssetBundlerBatch` 命令的格式为：

```cmd
AssetBundlerBatch command --parameterWithArgs arg1,arg2 --flagParameter ...
```

`AssetBundlerBatch`可执行文件与`Editor`和`AssetProcessor`可执行文件包含在同一个文件夹中。 如果找不到可执行文件，则可能需要使用`--target AssetBundlerBatch`运行`cmake --build <build path>`命令来构建`AssetBundlerBatch`。

此示例调用中的元素细分为：
+  `command` - 资源打包器要运行的命令。示例包括`seeds` 和 `assetLists`.
+  `--parameterWithArgs` - 一个采用参数的参数。如果一个参数可以接受多个参数，你可以用`,`字符分隔参数，而不使用空格，或者多次给出参数：
  + `--parameterWithArgs arg1,arg2`
  + `--parameterWithArgs="arg1,arg2"`
  + `--parameterWithArgs arg1 --parameterWithArgs arg2`
  + `--parameterWithArgs="arg1" --parameterWithArgs="arg2"`

   这些编写参数的样式可以自由混合和匹配。请注意，并非所有参数都采用多个 input 参数。
+  `--flagParameter` - 一个不接受任何参数的标志。标志表示布尔值，并打开和关闭 Asset Bundler 的功能。

 An example command is:

{{< tabs name="Command line example-General Use" >}}
{{% tab name="Windows" %}}

```cmd
build\windows\bin\profile\AssetBundlerBatch.exe seeds --seedFileList MyProject\AssetBundling\SeedLists\AllDependencies.seed ^
    --addPlatformToSeeds ^
    --platform ios,pc
```

{{% /tab %}}
{{% tab name="Linux" %}}

```shell
build/linux/bin/profile/AssetBundlerBatch seeds --seedFileList MyProject/AssetBundling/SeedLists/AllDependencies.seed \
    --addPlatformToSeeds \
    --platform ios,linux
```

{{% /tab %}}
{{< /tabs >}}


### 选项

本节中的选项对所有 Asset Bundler 子命令都有效。

**`--help` / `-h`**

如果没有命令，则打印出 `AssetBundlerBatch` 的帮助文本。与命令一起使用时，打印该命令的帮助信息。

**`--verbose`**

启用更详细的输出消息。 `Error` 和 `Warning`消息将显示生成消息的文件名和行号。此标志用于查看源文件或生成用于调试目的的输出。

## 种子列表 - `seeds`

`seeds`命令用于管理种子列表，这是 Asset Bundler 工作流程的第一阶段。种子列表文件的扩展名为`.seed`。

此命令需要每个提供的平台的现有资产缓存。要确保缓存是最新的，请更新支持的平台并运行 Asset Processor。

### 选项

**`--seedListFile`**

要修改和保存的种子列表。此文件必须是可写的，并且具有`.seed`扩展名。如果文件不存在，则会创建该文件。参数的值可以是绝对路径或引擎根相对路径。

* *Type:* 单值参数
* *Required:* Yes

**`--addSeed`**

将 seeds 添加到`--platform`值的种子列表中。如果其中一个指定平台不存在种子，则会忽略无效平台。参数的值可以是指向预处理资产的任意数量的缓存相对路径。

尽管 `.slice` 文件已构建并存在于资产缓存中，但它们不是产品资产，并且在添加到种子列表时不会生成依赖项。避免将它们用作种子。

* *Type:* 多值参数
* *Required:* No

**`--removeSeed`**

删除提供的种子的`--platform`值。如果种子对平台不可用，则会忽略它。种子在没有关联的平台之前不会从种子列表中完全删除。

* *Type:* 多值参数
* *Required:* No

**`--addPlatformToSeeds`**

将`--platform`值添加到种子列表中的所有种子。如果种子对提供的平台无效，则会打印一条警告，指示受影响的种子和导致警告的平台。将针对所有其他有效平台更新 seed。

* *Type:* Flag
* *Required: No*

**`--removePlatformFromSeeds`**

从种子列表中的所有种子中删除`--platform`值。如果这将删除现有种子的所有平台，则种子将保持不变并打印警告。使用`--removeSeed`从种子列表中删除种子。

* *Type:* Flag
* *Required:* No

**`--print`**

执行操作后打印种子列表的内容。

* *Type:* Flag
* *Required:* No

**`--platform`**

用于此命令的平台。默认为当前项目支持的所有平台。可以通过修改`AssetProcessorPlatformConfig.ini`来更改支持的平台。

平台名称可以在 `AssetProcessorPlatformConfig.ini` 文件中找到，也可以作为文件夹名称找到`dev/Cache/ProjectName` 。

* *Type:* 多值参数
* *Required:* No

**`--updateSeedPath`**

更新 seed 列表中每个 seed 的相对路径。

* *Type:* Flag
* *Required:* No

**`--removeSeedPath`**

删除种子列表中每个种子的相对路径提示。当您想与第三方共享种子列表时，此参数非常有用。

* *Type:* Flag
* *Required:* No

### 示例

在以下示例中，假设当前项目已启用平台 `pc`, `ios`, 和 `android`。

**示例 : 添加种子**

如果不存在，请创建种子列表 `testFile.seed` ，并将`asset1.pak` 和 `asset2.pak`资产添加为`PC`平台的种子：

{{< tabs name="Command line example-Add seed" >}}
{{% tab name="Windows" %}}

```cmd
build\windows\bin\profile\AssetBundlerBatch.exe seeds --seedListFile testFile.seed ^
    --addSeed cache\asset_path\asset1.pak,cache\asset_path\asset2.pak ^
    --platform pc
```

{{% /tab %}}
{{% tab name="Linux" %}}

```cmd
build/linux/bin/profile/AssetBundlerBatch seeds --seedListFile testFile.seed \
    --addSeed cache/asset_path/asset1.pak,cache/asset_path/asset2.pak \
    --platform linux
```

{{% /tab %}}
{{< /tabs >}}

**示例 : 添加平台**

为`testFile.seed`种子列表中的所有种子添加`ios` 和 `android`平台：

{{< tabs name="Command line example-Add platforms" >}}
{{% tab name="Windows" %}}

```cmd
build\windows\bin\profile\AssetBundlerBatch.exe seeds --seedListFile testFile.seed --addPlatformToSeeds --platform ios,android
```

{{% /tab %}}
{{% tab name="Linux" %}}

```cmd
build/linux/bin/profile/AssetBundlerBatch seeds --seedListFile testFile.seed --addPlatformToSeeds --platform ios,android
```

{{% /tab %}}
{{< /tabs >}}

**示例 ：显示种子列表内容**

显示 `testFile.seed` 文件的内容，包括用作种子的所有资产的绝对路径和相对路径：

{{< tabs name="Command line example-Display seed list contents" >}}
{{% tab name="Windows" %}}

```cmd
build\windows\bin\profile\AssetBundlerBatch.exe seeds --seedListFile testFile.seed --print
```

{{% /tab %}}
{{% tab name="Linux" %}}

```cmd
build/linux/bin/profile/AssetBundlerBatch seeds --seedListFile testFile.seed --print
```

{{% /tab %}}
{{< /tabs >}}

## 资产列表 - `assetLists` 

`assetLists`命令用于管理和创建资产列表。有关如何在资产列表文件名中对平台信息进行编码的信息，请参阅 `--assetListFile` 参数。资源列表文件的扩展名为`.assetlist`。

此命令需要每个提供的平台的现有资产缓存。要确保缓存是最新的，请更新支持的平台并运行 Asset Processor。

### 选项

**`--assetListFile`**

要生成的特定于平台的资产列表文件的基本名称。此文件的路径必须是可写的，并且文件名必须具有`.assetlist`扩展名。参数的值可以是绝对路径或引擎根相对路径。

生成的资产列表文件根据此参数的值和提供的平台进行命名。对于参数值`path/assetFile.assetlist`，每个平台都会获得一个创建为`path/assetFile_Platform.assetlist`的资源文件。例如，平台`pc`的`assetFile.assetlist`被命名为`assetFile_pc.assetlist`。

`assetLists` 调用必须包含 `--assetListFile` 参数或 `--print` 标志。

* *Type:* 单值参数
* *Required:* No

**`--seedListFile`**

用于生成资产列表的种子列表。此参数可以与提供种子的其他参数一起使用。

* *Type:* 单值参数
* *Required:* No

**`--addSeed`**

用于生成资产列表的单个种子。此参数可以与提供种子的其他参数一起使用。

尽管`.slice`文件已构建并存在于资产缓存中，但它们不是产品资产，并且在用作种子时不会生成依赖项。避免将它们用作种子。

* *Type:* 多值参数
* *Required:* No

**`--addDefaultSeedListFiles`**

自动包含 O3DE 引擎、默认项目和所有已启用 Gem 的默认种子列表。作为项目相对路径，定义默认资源的文件包括：
+  **Engine** - `Engine/Engine_Dependencies.xml`
+  **Gems** - `Gems/gem_name/Assets/gem_name_Dependencies.xml`
+  **Project** - `project_name/project_name_Dependencies.xml`

此标志还包括以下位置的默认种子列表文件：
+  **Engine** - `<engine root>/Assets/Engine/SeedAssetList.seed`
+  **Platform** - `<engine root>/Assets/Engine/Platforms/<platform>/*.seed`
+  **Gems** - `<gem name>/Assets/seedList.seed`

此参数可以与提供种子的其他参数一起使用。

* *Type:* Flag
* *Required:* No

**`--skip`**

要忽略的资产列表。这可以包括种子资产和资产处理器选取的任何依赖项。参数值是一个逗号分隔的列表，其中包含已预处理的资产的缓存相对路径。

* *Type:* 多值参数
* *Required:* No

**`--platform`**

用于此命令的平台。默认为当前项目支持的所有平台。可以通过修改 `AssetProcessorPlatformConfig.ini`来更改支持的平台。

启用的平台名称可以在 `AssetProcessorPlatformConfig.ini` 文件中找到，也可以在 `dev/Cache/ProjectName` 下的目录中找到。

* *Type:* 多值参数
* *Required:* No

**`--print`**

输出所有产品依赖项的列表。此参数的行为取决于向 `AssetBundlerBatch argumentList` 命令提供的其他参数：
+ `--assetListFile` 无操作：从磁盘读取已有资产列表，并显示其内容。
+ `--assetListFile` 有操作：生成新的资产列表并显示其内容。
+ 不带 `--assetListFile`: 显示将生成的资产列表的内容。

这些行为中的每一个都在 [assetList 示例](#asset-bundler-command-line-reference-assetlists-examples).

`AssetBundlerBatch assetLists`命令必须包含`--assetListFile`参数以生成新的资产列表，或`--print`标志以将信息写入控制台。

* *Type:* Flag
* *Required:* No

**`--dryRun`**

在不生成新资产列表的情况下运行。

* *Type:* Flag
* *Required:* No

**`--generateDebugFile`**

生成一个文件，其中包含有关资产包含的其他信息，以便进行调试。此文件包含资产列表文件中包含的每个资产的分层列表，以及有关哪些种子将每个特定资产标记为产品依赖项的信息。调试文件存储在与`--assetListFile`相同的路径中，扩展名为`.assetlistdebug`。

此参数需要使用`--assetListFile`。如果使用 `--print` 参数，则生成的文件的输出将显示在控制台中，而不是调试文件的输出。

* *Type:* Flag
* *Required:* No

**`--allowOverwrites`**

允许覆盖现有资源文件。默认情况下，不会重新生成现有资源列表。

* *Type:* Flag
* *Required:* No

### 示例 

在以下示例中，假设存在种子列表`testFile.seed` 并且启用了 `pc`, `ios`, 和 `android` 平台。

**示例 ：显示默认资产**

显示将从 O3DE 引擎的种子列表和项目的默认平台的已启用 Gem 中生成的资产列表：

{{< tabs name="Command line example-Display default assets" >}}
{{% tab name="Windows" %}}

```cmd
build\windows\bin\profile\AssetBundlerBatch.exe assetLists --addDefaultSeedListFiles --print
```

{{% /tab %}}
{{% tab name="Linux" %}}

```cmd
build/linux/bin/profile/AssetBundlerBatch assetLists --addDefaultSeedListFiles --print
```

{{% /tab %}}
{{< /tabs >}}

**示例 ：显示默认平台的资产列表**

使用输入资源列表和项目的默认平台来显示该资源列表的内容：

{{< tabs name="Command line example-Display asset lists for default platforms" >}}
{{% tab name="Windows" %}}

```cmd
build\windows\bin\profile\AssetBundlerBatch.exe assetLists --assetListFile assetListFile.assetlist --print
```

{{% /tab %}}
{{% tab name="Linux" %}}

```cmd
build/linux/bin/profile/AssetBundlerBatch assetLists --assetListFile assetListFile.assetlist --print
```

{{% /tab %}}
{{< /tabs >}}

**示例：从种子列表创建资产列表和调试文件**

从 `testFile.seed` 种子列表中生成资源列表`testList_pc.assetlist` 和调试信息 `testList_pc.assetlistdebug` ：

{{< tabs name="Command line example-Create an asset list and debug file from a seed list" >}}
{{% tab name="Windows" %}}

```cmd
build\windows\bin\profile\AssetBundlerBatch.exe assetLists --assetListFile testList.assetlist ^
    --seedListFile testFile.seed ^
    --platform pc ^
    --generateDebugFile
```

{{% /tab %}}
{{% tab name="Linux" %}}

```cmd
build/linux/bin/profile/AssetBundlerBatch assetLists --assetListFile testList.assetlist \
    --seedListFile testFile.seed \
    --platform linux \
    --generateDebugFile
```

{{% /tab %}}
{{< /tabs >}}

**示例 ：显示平台的资产列表内容**

显示 `testList_pc.assetlist` 文件的内容：

{{< tabs name="Command line example-Display asset list contents for a platform" >}}
{{% tab name="Windows" %}}

```cmd
build\windows\bin\profile\AssetBundlerBatch.exe assetLists --assetListFile testList.assetlist --platform pc --print
```

{{% /tab %}}
{{% tab name="Linux" %}}

```cmd
build/linux/bin/profile/AssetBundlerBatch assetLists --assetListFile testList.assetlist --platform linux --print
```

{{% /tab %}}
{{< /tabs >}}

**示例 ：从种子列表重新生成资产列表**

从`testFile.seed`种子列表重新生成所有资产列表，覆盖`testList_pc.assetlist`文件（如果存在）：

{{< tabs name="Command line example-Regenerate asset lists from a seed list" >}}
{{% tab name="Windows" %}}

```cmd
build\windows\bin\profile\AssetBundlerBatch.exe assetLists --assetListFile testList.assetlist --seedListFile testFile.seed --allowOverwrites
```

{{% /tab %}}
{{% tab name="Linux" %}}

```cmd
build/linux/bin/profile/AssetBundlerBatch assetLists --assetListFile testList.assetlist --seedListFile testFile.seed --allowOverwrites
```

{{% /tab %}}
{{< /tabs >}}

## 比较规则 - `comparisonRules` 

`comparisonRules`命令用于生成比较规则文件。比较规则文件用作 [compare](#asset-bundler-command-line-reference-compare)子命令的输入。比较规则文件是要执行的操作和顺序的预构建描述。有关比较规则的更多信息，请参阅 [打开 3D 引擎资产列表比较操作](/docs/user-guide/packaging/asset-bundler/list-operations/)。

### 选项 

**`--comparisonRulesFile`**

要生成的比较规则文件。

* *Type:* 单值参数
* *Required:* Yes

**`--comparisonType`**

要应用的比较类型，按给定顺序。有效值为：
+ *0* 或 *delta*：增量比较
+ *1* 或 *union*：联合
+ *2* 或 *intersection*：交集
+ *3* 或 *complement*：补码
+ *4* 或 *filePattern*： FilePattern
+ *5* 或 *intersectionCount*：IntersectionCount

有关这些规则如何对输入文件进行操作的更多信息，请参阅[打开 3D 引擎资产列表比较操作](/docs/user-guide/packaging/asset-bundler/list-operations/).

`intersectionCount`比较类型不能作为规则列表的一部分与任何其他比较类型组合。

* *Type:* 多值参数
* *Required:* Yes

**`--filePatternType`**

要对提供的文件模式使用的文件模式匹配的类型。有效值为：
+ *0*：执行通配符匹配 -`*`字符将匹配任意数量的字符
+ *1*：执行正则表达式匹配

* *Type:* 多值参数. `--filePatternType`参数的参数数量必须与`--comparisonType`参数的 FilePattern 参数数量匹配。
* *Required:* No

**`--filePattern`**

用于构建将由相应的 `--comparisonType` 进行比较的文件列表的文件模式。根据相应的`--filePatternType`参数解释模式。

* *Type:* 多值参数. `--filePattern`参数的参数数量必须与`--comparisonType`参数的 FilePattern 参数数量匹配。
* *Required:* No

**`--allowOverwrites`**

允许覆盖现有的比较规则文件。默认情况下，不会覆盖现有文件。

* *Type:* Flag
* *Required:* No

### 示例 

**示例：为 XML 文件生成增量和过滤器**

生成一个比较规则文件，该文件生成增量比较，然后筛选结果以仅包含 XML 文件：

{{< tabs name="Command line example-Generate a delta and filter for XML files" >}}
{{% tab name="Windows" %}}

```cmd
build\windows\bin\profile\AssetBundlerBatch.exe comparisonRules --comparisonRulesFile deltaFilterXML.rules ^
     --comparisonType delta,filePattern ^
     --filePatternType 0 ^
     --filePattern "*xml"
```

{{% /tab %}}
{{% tab name="Linux" %}}

```cmd
build/linux/bin/profile/AssetBundlerBatch comparisonRules --comparisonRulesFile deltaFilterXML.rules \
     --comparisonType delta,filePattern \
     --filePatternType 0 \
     --filePattern "*xml"
```

{{% /tab %}}
{{< /tabs >}}

## 比较 - `compare` 

`compare` 命令用于将资产列表对作为输入，执行比较操作，并将比较结果写入新的资产列表。有关比较操作的详细信息，请参阅 [Open 3D Engine 资产列表比较操作](/docs/user-guide/packaging/asset-bundler/list-operations/)。

### 选项 

**`--comparisonRulesFile`**

要从中加载规则的比较规则文件。规则文件中的比较将在作为参数给出的任何其他比较之前执行，并按创建规则文件的顺序进行评估。

* *Type:* 单值参数
* *Required:* No

**`--comparisonType`**

要应用于输入文件的比较类型。第一个`--comparisonType`参数应用于`--firstAssetListFile` 和 `--secondAssetListFile`的第一个参数，第二个比较应用于这些参数的第二个参数，依此类推。

有效值为：
+ *0* 或 *delta*：增量比较
+ *1* 或 *union*：联合
+ *2* 或 *intersection*：交集
+ *3* 或 *complement*：补码
+ *4* 或 *filePattern*： FilePattern
+ *5* 或 *intersectionCount*：IntersectionCount

有关这些规则如何对输入文件进行操作的更多信息，请参阅[打开 3D 引擎资产列表比较操作](/docs/user-guide/packaging/asset-bundler/list-operations/).

`intersectionCount` 比较类型不能与任何其他比较类型结合使用。

* *Type:* 多值参数
* *Required:* Yes

**`--filePatternType`**

要对提供的文件模式使用的文件模式匹配的类型。此参数的有效值为：
+ *0*：执行通配符匹配 - `*`字符将匹配任意数量的字符
+ *1*：执行正则表达式匹配

* *Type:* 多值参数. 用于`--filePatternType` 参数的参数数必须匹配 `FilePattern`，比较 `--comparisonType` 参数。
* *Required:* No

**`--filePattern`**

用于匹配输入中的资产文件路径的文件模式。根据相应的`--filePatternType`参数解释模式。第一个模式与第一次出现的`FilePattern`比较类型一起使用，第二个模式与第二次出现的比较类型一起使用，依此类推。

* *Type:* 多值参数. 用于 `--filePattern` 参数的参数数必须匹配 `FilePattern`，比较 `--comparisonType` 参数。
* *Required:* No

**`--firstAssetListFile`**

用作第一组输入以进行比较的文件。

* *Type:* 多值参数. `--firstAssetListFile`参数的参数数量必须与`--comparisonType`参数的参数数量匹配。
* *Required:* No

**`--secondAssetListFile`**

用作需要第二个输入文件的比较的第二组输入的文件。此参数不用于 `FilePattern` 或 `IntersectionCount` 比较类型。

* *Type:* 多值参数. `--secondAssetListFile`参数的参数数量必须与`--comparisonType`参数的非`FilePattern`参数的数量匹配。
* *Required:* No

**`--output`**

每个执行的比较的结果的输出文件。输出文件可以是文件，也可以是从另一个比较传递的变量。变量以 `$` 字符开头。有关变量的更多信息，请参阅 [打开 3D 引擎资产列表比较操作](/docs/user-guide/packaging/asset-bundler/list-operations/).

* *Type:* 多值参数. `--output`参数的参数数量必须与 `--comparisonType` 参数的参数数量匹配。
* *Required:* No

**`--print`**

此参数的行为会有所不同，具体取决于它是作为标志提供还是具有参数列表。
+ **Flag (no arguments)**: 将最终比较结果打印到控制台。
+  **With arguments**: 比较完成后，将每个参数的内容打印到控制台。参数可以是文件或变量。

* *Type:* 多值参数 or flag
* *Required:* No

**`--allowOverwrites`**

允许覆盖现有输出文件。默认情况下，不会覆盖现有文件。

* *Type:* Flag
* *Required:* No

### 示例 

**示例 ：比较以生成增量**

通过获取出现在`firstAssetList_pc.assetlist` 或 `secondAssetList_pc.assetlist`中的文件来生成新的资产列表 `deltaAssetList.assetlist`，但不能同时获取两者：

{{< tabs name="Command line example-Compare to generate a delta" >}}
{{% tab name="Windows" %}}

```cmd
build\windows\bin\profile\AssetBundlerBatch.exe compare --comparisonType delta ^
     --firstAssetFile firstAssetList_pc.assetlist ^
     --secondAssetFile secondAssetList_pc.assetlist ^
     --output deltaAssetList.assetlist
```

{{% /tab %}}
{{% tab name="Linux" %}}

```cmd
build/linux/bin/profile/AssetBundlerBatch compare --comparisonType delta \
     --firstAssetFile firstAssetList_pc.assetlist \
     --secondAssetFile secondAssetList_pc.assetlist \
     --output deltaAssetList.assetlist
```

{{% /tab %}}
{{< /tabs >}}

**示例 ：根据文件路径匹配进行比较**

生成一个新的资源列表`filePatternAssetList.assetlist`，其中仅包含 `assetList_pc.assetlist` 文件中的 XML 文件：

{{< tabs name="Command line example-Compare based on file path matching" >}}
{{% tab name="Windows" %}}

```cmd
build\windows\bin\profile\AssetBundlerBatch.exe compare --comparisonType filePattern ^
    --filePatternType 0 ^
    --filePattern "*.xml" ^
    --firstAssetFile assetList_pc.assetlist ^
    --output filePatternAssetList.assetlist
```

{{% /tab %}}
{{% tab name="Linux" %}}

```cmd
build/linux/bin/profile/AssetBundlerBatch compare --comparisonType filePattern \
    --filePatternType 0 \
    --filePattern "*.xml" \
    --firstAssetFile assetList_pc.assetlist \
    --output filePatternAssetList.assetlist
```

{{% /tab %}}
{{< /tabs >}}

**示例 ：计算多个资产列表中的交集**

使用`engine_pc.assetlist`, `game_pc.assetlist`, 和 `patch_pc.assetlist`上 `intersectionCount`打印出在以下任一资源列表之间出现 2 次或更多次的资源：

{{< tabs name="Command line example-Count intersection across multiple asset lists" >}}
{{% tab name="Windows" %}}

```cmd
build\windows\bin\profile\AssetBundlerBatch.exe compare --comparisonType intersectionCount ^
    --firstAssetFile engine_pc.assetlist,game_pc.assetlist,patch_pc.assetlist ^
    --print
```

{{% /tab %}}
{{% tab name="Linux" %}}

```cmd
build/linux/bin/profile/AssetBundlerBatch compare --comparisonType intersectionCount \
    --firstAssetFile engine_pc.assetlist,game_pc.assetlist,patch_pc.assetlist \
    --print
```

{{% /tab %}}
{{< /tabs >}}

## 捆绑包设置 - `bundleSettings` 

`bundleSettings`命令用于管理包设置文件，这些文件是配置文件，可让您存储常用的包配置，以便于重用和自动化。

### 选项 

**`--bundleSettingsFile`**

执行此命令时要修改的 bundle 设置文件。如果此文件已存在，则仅更改由命令调用指定的设置。

* *Type:* 单值参数
* *Required:* Yes

**`--assetListFile`**

设置要在包生成中使用的资源列表文件。

* *Type:* 单值参数
* *Required:* No

**`--outputBundlePath`**

设置生成的 asset bundle 写入到的位置。资源包使用`.pak`文件扩展名。

* *Type:* 单值参数
* *Required:* No

**`--bundleVersion`**

设置要在生成中使用的捆绑包格式版本。唯一允许的值为 `1`。

* *Type:* 单值参数
* *Required:* No

**`--maxSize`**

设置单个捆绑包允许的最大大小 （MB）。如果生成的任何捆绑包大于最大大小，则会将其拆分为更小的捆绑包并相应地命名。

* *Type:* 单值参数
* *Required:* No

**`--platform`**

要更新其捆绑包设置的平台。默认为项目的已启用平台，在 `AssetProcessorPlatformConfig.ini`中定义。有效的平台名称可以在平台配置文件中找到，也可以在  `dev/Cache/ProjectName`下的文件夹名称中找到。

* *Type:* 多值参数
* *Required:* No

**`--print`**

修改所有值后，将包设置文件的内容打印到控制台。

* *Type:* Flag
* *Required:* No

### 示例 

以下示例假定为项目启用了这些平台：`pc`

**示例 ：为 PC 设置默认最大捆绑包大小和资产列表**

为 PC 创建一个打包器设置文件`defaults_pc.bundlesettings`，最大捆绑包大小设置为 1024 MB，并将`allAssets_pc.assetlist`资产列表作为其输入：

{{< tabs name="Command line example-Set default max bundle size and asset list for PC" >}}
{{% tab name="Windows" %}}

```cmd
build\windows\bin\profile\AssetBundlerBatch.exe bundleSettings --bundleSettingsFile defaults.bundlesettings ^
    --maxSize 1024 ^
    --assetListFile allAssets.assetlist ^
    --platforms pc
```

{{% /tab %}}
{{% tab name="Linux" %}}

```cmd
build/linux/bin/profile/AssetBundlerBatch bundleSettings --bundleSettingsFile defaults.bundlesettings \
    --maxSize 1024 \
    --assetListFile allAssets.assetlist \
    --platforms linux
```

{{% /tab %}}
{{< /tabs >}}

## 资源包 - `bundles` 

`bundles` 命令用于生成包含资产列表中所有资产的最终捆绑包 (`.pak`)文件。捆绑包一旦创建就无法修改，只能重新生成。

### 选项 

**`--bundleSettingsFile`**

要加载的 bundle 设置文件。如果提供的参数将覆盖 settings 文件，则参数将覆盖 settings 文件。

* *Type:* 单值参数
* *Required:* No

**`--assetListFile`**

要在 bundle 生成中使用的资源列表文件。

* *Type:* 单值参数
* *Required:* No

**`--outputBundlePath`**

生成的资源包写入的位置。资源包使用`.pak`文件扩展名。
* *Type:* 单值参数
* *Required:* No

**`--bundleVersion`**

生成时使用的捆绑包格式版本。唯一允许的值为`1`。

* *Type:* 单值参数
* *Required:* No

**`--maxSize`**

单个捆绑包允许的最大大小，以 MB 为单位。如果生成的任何捆绑包大于最大大小，则会将其拆分为更小的捆绑包并相应地命名。

* *Type:* 单值参数
* *Required:* No

**`--platform`**

要为其生成捆绑包的平台。默认为项目的已启用平台，在`AssetProcessorPlatformConfig.ini`中定义。有效的平台名称可以在平台配置文件中找到，也可以在`dev/Cache/ProjectName` 下的文件夹名称中找到。

* *Type:* 多值参数
* *Required:* No

**`--allowOverwrites`**

允许覆盖现有捆绑包文件。默认情况下，不会覆盖捆绑包。

* *Type:* Flag
* *Required:* No

### 示例 

在以下示例中，假设当前项目已启用平台`pc`, `ios`, 和 `android`.

**示例 ：使用设置文件为 PC 创建捆绑包**

使用`defaults_pc.bundlesettings`文件为 PC 创建`assets_pc.pak`捆绑包：

{{< tabs name="Command line example-Create a bundle for PC" >}}
{{% tab name="Windows" %}}

```cmd
build\windows\bin\profile\AssetBundlerBatch.exe bundles --outputBundlePath assets.pak --bundleSettingsFile defaults.bundlesettings --platform pc
```

{{% /tab %}}
{{% tab name="Linux" %}}

```cmd
build/linux/bin/profile/AssetBundlerBatch bundles --outputBundlePath assets.pak --bundleSettingsFile defaults.bundlesettings --platform linux
```

{{% /tab %}}
{{< /tabs >}}

**示例 ：为所有平台创建捆绑包**

为项目的所有已启用平台创建捆绑包，使用`allAssets_pc.assetlist`, `allAssets_ios.assetlist`, 和 `allAssets_android.assetlist` 文件:

{{< tabs name="Command line example-Create bundles for all platforms" >}}
{{% tab name="Windows" %}}

```cmd
build\windows\bin\profile\AssetBundlerBatch.exe bundles --outputBundlePath assets.pak --maxSize 512 --assetListFile allAssets.assetlist
```

{{% /tab %}}
{{% tab name="Linux" %}}

```cmd
build/linux/bin/profile/AssetBundlerBatch bundles --outputBundlePath assets.pak --maxSize 512 --assetListFile allAssets.assetlist
```

{{% /tab %}}
{{< /tabs >}}

## 从种子捆绑 - `bundleSeed` 

`bundleSeed`命令用于直接从种子及其依赖项生成捆绑包，而无需使用中间资产列表。除了所需的种子之外，没有其他文件用作输入，并且仅生成捆绑包文件作为输出。

### 选项 

**`--addSeed`**

要在捆绑包文件生成中使用的种子。这些种子的所有资产依赖项也包含在捆绑包中。参数参数应作为预处理资产的缓存相对路径提供。

* *Type:* 多值参数
* *Required:* Yes

**`--bundleSettingsFile`**

要加载的 bundle 设置文件。如果提供的参数将覆盖 settings 文件，则参数将覆盖 settings 文件。

* *Type:* 单值参数
* *Required:* No

**`--outputBundlePath`**

生成的资源包写入的位置。资源包使用`.pak`文件扩展名。

* *Type:* 单值参数
* *Required:* No

**`--bundleVersion`**

生成时使用的捆绑包格式版本。唯一允许的值为 `1`。

* *Type:* 单值参数
* *Required:* No

**`--maxSize`**

单个捆绑包允许的最大大小，以 MB 为单位。如果生成的任何捆绑包大于最大大小，则会将其拆分为更小的捆绑包并相应地命名。

* *Type:* 单值参数
* *Required:* No

**`--platform`**

要更新其捆绑包设置的平台。默认为项目的已启用平台，在 `AssetProcessorPlatformConfig.ini`中定义。有效的平台名称可以在平台配置文件中找到，也可以在 `dev/Cache/ProjectName`下的文件夹名称中找到。

* *Type:* 多值参数
* *Required:* No

**`--allowOverwrites`**

允许覆盖现有捆绑包文件。默认情况下，不会覆盖捆绑包。

* *Type:* Flag
* *Required:* No

### Examples 

**示例 ：为种子重新生成捆绑包**

为`example.cgf`资产及其所有依赖项重新生成包`processed.pak`，最大大小为 512MB。

{{< tabs name="Command line example-Regenerate bundle for a seed" >}}
{{% tab name="Windows" %}}

```cmd
build\windows\bin\profile\AssetBundlerBatch.exe bundleSeed --addSeed example.cgf --outputBundlePath processed.pak --maxSize 512 --allowOverwrites
```

{{% /tab %}}
{{% tab name="Linux" %}}

```cmd
build/linux/bin/profile/AssetBundlerBatch bundleSeed --addSeed example.cgf --outputBundlePath processed.pak --maxSize 512 --allowOverwrites
```

{{% /tab %}}
{{< /tabs >}}
