---
linkTitle: 转换材质
title: 转换材质
description: 了解如何运行转换器工具将 Lumberyard .mtl 文件处理为 O3DE .azmaterial 和关联的纹理
weight: 200
toc: true
---

本教程将教您如何使用 **Legacy Asset Converter** 将传统材质文件 （*.mtl*） 转换为 Atom 材质 （*.azmaterial*）。此工具还会将 Metallic、Normal 和 Roughness 纹理转换为可接受的 Atom 格式。

| O3DE 体验 |完成时间 |功能聚焦 |最后更新 |
| - | - | - | - |
| 初级 |15 分钟 |转换 `.mtl` 文件并在 O3DE 中测试它们 | February 27, 2024 |

## Launch the Legacy Asset Converter application

这个实用程序应用程序位于 AtomLyIntegration gem 中，为了让应用程序找到引擎，需要做一个 **第一次设置**。打开文件夹`YOUR_O3DE_INSTALL_FOLDER\Gems\AtomLyIntegration\TechnicalArt\DccScriptingInterface`，复制文件 `settings.local.json.example`  并将其重命名为`settings.local.json`。您需要使用文本编辑器打开它，以**替换路径以匹配您的安装** O3DE。

```json
{
	"O3DE_DEV": "C:\\depot\\o3de-dev",
	"PATH_O3DE_3RDPARTY": "C:\\depot\\3rdParty",
	"PATH_O3DE_BIN": "C:\\depot\\o3de-dev\\build\\\windows_vs2019\\bin\\profile",
    "DCCSI_LOG_PATH": "{your_path}\\user\\log\\DCCsi"
}
```

{{< note >}}
“O3DE_DEV” 是您本地 O3DE github 存储库的位置。“PATH_O3DE_3RDPARTY”是第三方安装文件夹（默认为 `C:\Users\CURRENT_USER\.o3de\3rdParty`）。“PATH_O3DE_BIN” 是 O3DE `editor.exe` 可用的文件夹。“DCCSI_LOG_PATH” 是此工具日志的输出位置，您可以使用任何东西。
{{< /note >}}

该实用程序依赖于 **本地 O3DE python 安装**，Windows 可通过`.bat`文件访问，Linux 可通过`.sh`文件访问。现在设置已设置，您可以在`DccScriptingInterface`文件夹上打开命令行并运行：

```cmd
.\python foundation.py
.\python config.py
```

最后，您将能够通过以下命令从此文件夹 **打开 Legacy Asset Converter** ：

```cmd
.\python SDK\Maya\Scripts\Python\legacy_asset_converter\main.py
```

## 转换材质和纹理

该工具将以友好的 UI 迎接您，允许您选择转换选项。您可以将 “Actions” 值保留为默认值。

![Legacy Asset Converter](/images/learning-guide/tutorials/lumberyard-to-o3de/legacy-asset-converter.png)

1. 单击按钮以设置包含`.mtl` 文件的输入文件夹。
2. 单击按钮设置输出文件夹。
3. 单击以启动转换
4. 您可以通过此栏跟踪进度

**StarterGame** 项目的大多数资源都位于文件夹`LUMBERYARD_FOLDER\dev\Gems\StarterGame`中。建议使用其 `Assets`文件夹作为输入，一次转换一个 Gem，因为它将保留文件夹层次结构。转换尚不支持某些材质，您可以查看 `legacy_asset_converter\constants.py`文件以查看支持的类型。

{{< known-issue >}}
目前，该实用程序 **仅将 *.fbx* 文件作为入口处理**，并且只会转换它们关联的 `.mtl` 文件。没有 .fbx 关联的材质（如地形或效果材质）将不会被转换（将在不久的将来修复）。
{{< /known-issue >}}

## 在 O3DE 中导入材质

如果您计划转换 Lumberyard 切片和关卡，请务必 **保持与 StarterGame 中相同的文件层次结构**。我们将创建一个 **本地 Gem 来存储资产**，为此，请在 O3DE 文件夹中打开一个命令行并运行：

```cmd
YOUR_O3DE_REPO\scripts\o3de create-gem --gem-path CUSTOM_PATH --template-name AssetGem
```

现在是导出材质的文件夹，并将 `Assets` 的内容复制到 O3DE gem 的 `Assets` 文件夹中。不要丢弃`.assetinfo`文件，因为它们会向 O3DE 提供有关如何打开模型及其碰撞体/LOD 的信息。

您可以创建新的 O3DE 项目或修改现有项目。重要的是，您将新 Gem 作为依赖项添加到项目中。您可以查看 [此文档](/docs/user-guide/project-config/add-remove-gems/) 以获取启用 Gem 的分步方法。有了这个功能，下次打开项目时，你应该能够浏览到导出的材质，并在 **材质编辑器** 中打开它们。

![Material Editor](/images/learning-guide/tutorials/lumberyard-to-o3de/material-editor.png)
