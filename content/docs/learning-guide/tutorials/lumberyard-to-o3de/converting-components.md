---
linkTitle: 转换组件
title: 转换组件
description: 了解如何将 Lumberyard 关卡的传统组件转换为其 O3DE 等效组件，以及如何使用 O3DE 打开 Lumberyard 关卡。
weight: 300
toc: true
---

本教程教您如何在 Lumberyard 关卡和切片上使用 **传统内容转换**。此过程会将旧版实体组件转换为其 O3DE 等效组件。它还能够定位位于另一个项目中的资产。本教程的其他部分将介绍如何使用 O3DE 打开 Lumberyard 关卡。

| O3DE 体验 |完成时间 | 功能聚焦                               |最后更新 |
| - | - |------------------------------------| - |
| Intermediate | 1 Hour | 使用 O3DE 转换旧组件并打开`.ly` 和 `.slice`文件 | February 27, 2024 |

{{< note >}}
您需要使用 O3DE 的 **自定义版本** 来学习本教程。查看 [从 GitHub 设置 O3DE](/docs/welcome-guide/setup/setup-from-github/) 以了解如何继续。
{{< /note >}}

## 检查你的 Assets 文件层次结构

如果您按照前面的步骤操作，则您的设置当前应为：

- 您拥有 Starter Game 项目，并且能够使用 Lumberyard 打开它。
- 您有一个 Asset Gem，其中包含从 Starter Game 项目转换的资源。
- 您有一个空的 O3DE 项目，该项目使用本地 Asset Gem 作为依赖项

为了将关卡正确转换为 O3DE，您需要确保转换后的资源的相对路径与 Lumberyard 项目相同：

- 对于 Gem 资源，相对路径从 **Assets** 文件夹开始
- 对于项目资源，相对路径从项目的文件夹开始（而不是从它可能包含的 Assets 文件夹开始）

| 路径 |相对路径 |
| - | - |
| C:\my-o3de-gem\\**Assets**\test.fbx | test.fbx |
| C:\my-o3de-project\\**SamplesProject**\Assets\test.fbx | Assets\test.fbx |

您不必想得太多，只需将 Lumberyard Gem 中的文件夹放入 O3DE Gem 中，并将 StarterGame 项目中的文件夹放入 O3DE 项目中即可。纹理在转换过程中已重命名，以便使用正确的设置导入，因此您无需重命名它们。

{{< known-issue >}}
如果要转换 StarterGame 项目，则应重命名或删除`trees_atlas_am_oak_leaf_02.azmaterial`。此材质目前应用于树木，而它不应该应用于树木，如果我们保留它，树木将不会渲染其材质。
{{< /known-issue >}}

## 生成 AssetCatalog 文件

`assetcatalog.xml`文件是为给定项目处理的每个资源的列表，以及它们的相对路径和资源 ID。当您打开 O3DE 项目（在`YOUR_PROJECT_FOLDER\Cache\pc`下）时，会生成它，我们依靠它来**查找 O3DE 归属于我们转换的资产的资产 ID**。

我们需要 O3DE 的自定义版本，因为此文件默认为二进制，我们需要将其更改为 XML，以便能够在 pyton 脚本中解析它。打开代码编辑器并转到 `YOUR_O3DE_REPO/Tools/AssetProcessor/native/AssetManager/AssetCatalog.cpp`。 在 `AssetCatalog::SaveRegistry_Impl()`中，将 `AZ::ObjectStream::ST_BINARY` 改为 `AZ::ObjectStream::ST_XML`。

```cpp
// these 3 lines are what writes the entire registry to the memory stream
AZ::ObjectStream* objStream = AZ::ObjectStream::Create(&catalogFileStream, *serializeContext, AZ::ObjectStream::ST_XML);
{
    QMutexLocker locker(&m_registriesMutex);
    objStream->WriteClass(&m_registries[platform]);
}
```

然后删除项目文件夹中的 `Cache` 文件夹并重新启动引擎。您必须等待资产处理器任务结束 （如果您之前使用过 Lumberyard，请确保在启动 O3DE 之前 **关闭任务栏中的 Lumberyard Asset Processor**）。

成功运行旧版内容转换脚本后，您将能够恢复此更改。

## 从 Lumberyard 关卡中删除图层

理论上，当Prefab系统被禁用时，O3DE 仍然支持关卡层。但是，在实践中，对于像 StarterGame 这样大的关卡，当您打开关卡时，它会崩溃。此外，在下一节中用于将关卡转换为Prefab的 SerializeContextTools 尚无法解析层。

这意味着您 **需要手动删除图层并将其实体移动到新的父实体**。对于嵌套层，您需要执行相同的操作。在 StarterGame 关卡执行此操作大约需要 25 分钟。

关闭 Lumberyard 后，请不要忘记从任务栏关闭资产处理器，否则 O3DE 资产处理器将不会启动。

{{< video src="/images/learning-guide/tutorials/lumberyard-to-o3de/remove-layer.mp4" info="Remove layers." autoplay="true" loop="true" >}}

## 运行 Legacy Content Conversion 脚本

该脚本位于`YOUR_O3DE_REPOSITORY\Gems\AtomLyIntegration\CommonFeatures\Assets\Editor\Scripts\LegacyContentConversion\LegacyComponentConverter.py`中。该脚本本身非常简单，没有第三方依赖项，但是 **它希望从 Lumberyard dev 文件夹启动**，因此您需要在那里打开命令行。然后运行 （确保更新 args） ：

```cmd
YOUR_O3DE_REPOSITORY\python\python SCRIPT_PATH\LegacyComponentConverter.py -project=StarterGame -include_gems -assetCatalogOverridePath=YOUR_O3DE_PROJECT_PATH\Cache\pc\assetcatalog.xml
```

您将看到多个关于 *“Could not match （...） to a corresponding source atom material”* 的警告。如果您仔细观察，这些警告总是关于 *“_physics”* 或 *“_proxy”* 材料。这是故意这样做的，因为 Atom 并不真正支持这些材质，因此我们只需跳过它们（通过 Legacy Asset Converter 中的“Skip White Texture Materials”跳过它们）。

{{< caution >}}
这个 **工具是破坏性的**，它会内联更改文件并且不会创建副本。如果要在转换后返回，建议您使用 Git 跟踪 Gems、Levels 和 Slice Lumberyard 文件夹 （或创建副本）。
{{< /caution >}}

如果你想检查新旧切片文件之间的差异，请确保隐藏空格更改，否则大多数行将被标记。

## （可选） 在 O3DE 项目中打开修改后的 Lumberyard 关卡

目前 O3DE 仍然能够打开`.slice` 和 `.ly`文件，但 [引擎将取消支持](https://github.com/o3de/sig-content/issues/148)在某个时候。要切换此设置，您只需通过隐藏的用户设置**禁用Prefab系统**。创建一个新文件或将 `YOUR_O3DE_PROJECT\user\Registry\editorpreferences.setreg`中的内容替换为如下所示的内容。

```json
{
    "Amazon": {
        "Preferences": {
            "EnablePrefabSystem": false
        }
    }
}
```

然后，您需要 **手动复制切片和关卡文件**。对于 StarterGame 项目，这些是您需要复制到 O3DE 项目中的最重要的文件夹：

|Lumberyard 位置 |O3DE 位置 |
| - | - |
| dev\StarterGame\Levels | YOUR_PROJECT\Levels |
| dev\StarterGame\Slices | YOUR_PROJECT\Slices |
| dev\Gems\StarterGame\Environment\Assets\Slices | YOUR_GEM\Assets\Slices |

启动编辑器后，您还需要在打开关卡之前将 **ed_enableDPEInspector** CVar 设置为 false。为此，请导航到 Console 选项卡，单击底部的 “X” 图标并按名称搜索 CVar。

![Disable DPE](/images/learning-guide/tutorials/lumberyard-to-o3de/disable-dpe-cvar.png)

关卡将是漆黑的，您可以创建一个实体并 **为其分配一个定向光组件**，旋转度为 （-75， 0， -15） 和 2 作为 Intensity。您还需要在 Viewport 部分下的 Editor Settings 中将 Perspective Far Plane 值从 100 更新到 10 000。它应该是这样的：

![Slice in O3DE](/images/learning-guide/tutorials/lumberyard-to-o3de/slice-in-o3de.png)
