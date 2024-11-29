---
description: ' Use bundle mode to ensure that your asset bundles have the required
  assets for your O3DE game. '
title: Using Bundle Mode to Test Bundles
weight: 300
---

Bundle 模式是一个过程，允许您启用资源加载以优先处理 Bundle 而不是松散缓存资源。构建用于打包游戏的种子列表后，您可以使用 bundle 模式和 `sys_report_files_not_found_in_paks`控制台变量来测试您的打包规则。捆绑包模式使您可以轻松地从您指定的位置加载和报告所有捆绑包  \(游戏 `.pak` 文件\)  中的问题，而无需创建发布版本。

使用捆绑模式涉及两项关键任务：
+ 为不在捆绑包中的资产启用缺少资产报告。这将启用 “捆绑模式”。
+ 为您的游戏安装和加载捆绑包。

启用报告后，`sys_report_files_not_found_in_paks`控制台变量会报告何时加载不在任何捆绑包中的资源。通过有选择地加载 bundle 并使用 `sys_report_files_not_found_in_paks` 命令，您可以找到需要包含在 bundle 中的资产。

## 启用 Bundle Mode

要启用捆绑模式，请使用 `sys_report_files_not_found_in_paks` 控制台变量并指定值 '`1`'、'`2`' 或 '`3`'。值 '`1`' 将缺失的文件作为日志条目写入，而不发出警告消息。

以下列表显示了`sys_report_files_not_found_in_paks` 控制台变量的有效参数。

```
0 = Disabled
1 = Log
2 = Warning
3 = Error
<Every other value> = Error
```

### 日志文件条目

缺少的文件将记录为类似于以下内容的条目：

```
Missing from bundle: @assets@\levels\milestone2\auto_resourcelist.txt
```

如果您在启动器中使用`sys_report_files_not_found_in_paks`控制台变量，则错误消息将写入日志文件。

### 设置 Console 变量

在运行编辑器或 Launcher 之前启用 console 变量可确保报告所有缺失的资产。要确保在运行编辑器或启动器时控制台变量始终处于活动状态，请修改项目目录中的 `editor.cfg` 和 `autoexec.cfg`。

您还可以在运行时使用控制台(**\~**) 或远程控制台启用 console 变量。

## 捆绑模式命令

捆绑模式有两个命令：
+ **loadbundles** *<bundle\_directory>* *<extension>* - 将指定目录中的所有捆绑包加载到游戏中。如果未提供任何参数，则目录默认为`Bundles`，扩展名为`.pak`。
+ **unloadbundles** - 卸载通过`loadbundles`命令加载的任何捆绑包。

## 使用捆绑模式示例

以下过程显示了捆绑模式的工作原理。在示例中，当缺少捆绑包时，将进入游戏模式。

**测试捆绑包模式**

1. 在控制台窗口中，输入以下命令：

   ```
   sys_report_files_not_found_in_paks 1
   ```

   `1` 参数 指定将缺失文件报告为日志条目，而不是警告或错误。

1. 进入游戏模式。此时将显示 Missing from bundle errors （捆绑包中缺失） 错误列表。

![Missing from bundle errors in the console window.](/images/user-guide/assetbundler/asset-bundler-bundle-mode-1.png)

1. 输入命令`loadbundles` 以加载该级别的捆绑包。

![Using the loadbundles command.](/images/user-guide/assetbundler/asset-bundler-bundle-mode-2.png)

   错误较少，但仍缺少一些资产。[Asset Validation Gem](/docs/user-guide/gems/reference/assets/asset-validation)与种子相关的命令可以帮助查找缺失的资源。

1. 使用 Asset Validation gem `addseedpath` 命令添加可能缺少的捆绑包。

   ```
   addseedpath levels\milestone2\level.pak
   ```

1. 输入 `listknownassets` 命令。

![Listing known assets in the console.](/images/user-guide/assetbundler/asset-bundler-bundle-mode-3.png)

1. 检查输出。在以下示例中，输出显示缺少的按钮资产。

![Identifying missing assets in the output.](/images/user-guide/assetbundler/asset-bundler-bundle-mode-4.png)

   对于按钮资产，捆绑包是不久前打包的，必须重新打包。但是，其他资产也仍然缺失。

1. 将缺少的资源添加到关卡的种子列表中。

1. 运行关卡的 [捆绑命令](/docs/user-guide/packaging/asset-bundler/command-line-reference/)。

1. 将捆绑包放入`Bundles`目录中。

1. 输入`assetbundlerbatch assetlists`命令，如以下示例所示。 使用`--print`参数检查输出。在示例中，为了提高可读性，已对单行命令进行了格式设置。

   ```
   assetbundlerbatch assetlists
         --addseed levels\milestone2\level.pak
         --addseed levels\milestone2\milestonecutscene.scriptevents
         --addseed levels\milestone2\hardcodedassetreference.luac
         --print
   ```

1. 验证输出是否按预期显示。

1. 再次输入e `assetbundlerbatch assetlists` 命令以捆绑资源，但这次没有`--print`参数。示例命令是单行的，但为了便于阅读，已进行格式设置。

   ```
   assetbundlerbatch assetlists
         --addseed levels\milestone2\level.pak
         --addseed levels\milestone2\milestonecutscene.scriptevents
         --addseed levels\milestone2\hardcodedassetreference.luac
         --platform pc
         --assetlistfile DemoLevelList.assetlist
   ```

   输出:

   ```
   Saving Asset List file to ( G:\P4\dev\DemoLevelList_pc.assetlist )...
   Save successful! ( G:\P4\dev\DemoLevelList_pc.assetlist )
   ```

1. 输入`assetbundlerbatch bundles`命令，如以下示例所示。

   ```
   assetbundlerbatch bundles
         --assetlistfile DemoLevelList_pc.assetlist
         --platform pc
         --outputbundlepath G:\P4\dev\DemoProject\Bundles\milestone2.pak
   ```

   输出:

   ```
   Creating Bundle ( G:\P4\dev\DemoProject\Bundles\milestone2_pc.pak )...
   Bundle ( G:\P4\dev\DemoProject\Bundles\milestone2_pc.pak ) created successfully!
   ```

1. 输入`loadbundles`命令重新加载捆绑包，然后进入游戏模式。

![All loaded assets are now in bundles.](/images/user-guide/assetbundler/asset-bundler-bundle-mode-5.png)

   进入游戏模式时加载的所有资源现在都位于捆绑包中。
