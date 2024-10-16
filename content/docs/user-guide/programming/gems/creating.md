---
title: 创建O3DE Gem
description: 了解创建与 Open 3D Engine Gem 系统一起使用的组件的要求。
weight: 200
---

您可以开发自己的独立模块，作为 **Open 3D Engine (O3DE)** Gem 发布和构建。一个 gem 只是一捆代码和/或资产、一个清单文件、一个 CMake 构建文件和一个用于项目配置工具的可选显示图标。这就是发布一个 Gem 所需要的一切。

Gem 的最低要求是一个包含 `CMakeLists.txt` 构建文件和 `gem.json` 清单文件的目录。

虽然您可以通过自行创建所有文件来手动创建 Gem，但建议使用位于`<engine>/scripts`目录下的`o3de`工具：

{{< tabs name="创建 Gem" >}}
{{% tab name="Windows" %}}

```cmd
<engine>\scripts\o3de.bat create-gem -gp <path to create gem at>
```

{{% /tab %}}
{{% tab name="Linux" %}}

```cmd
<engine>/scripts/o3de.sh create-gem -gp <path to create gem at>
```

{{% /tab %}}
{{< /tabs >}}

这将创建具有标准文件结构的 Gem，以及根据 [模板](https://github.com/o3de/o3de/tree/development/Templates) 创建的 CMake 文件。

有关如何使用 `o3de` 工具的更多信息，请参阅 [项目配置 CLI 参考](/docs/user-guide/project-config/cli-reference/)。

## Assets

每个 Gem 都有一个 `Assets` （资产）目录，其中可包含模型、纹理、脚本、动画等内容。资产文件的访问方式与游戏项目中的相同。O3DE 使用这个根目录来查找资产文件路径。例如，当 O3DE 查找 `textures/rain/rainfall_ddn.tif` 文件时，它会在 `<GemName>/Assets/textures/rain/` 目录中查找。

你的 Gem 在启动器运行时使用的资产应在 [Gem 依赖 `.xml` 文件](#gem-dependencies) 中列出其源资产，或在 [`<GemName>/Assets/seedList.seed`](#default-gemnameassetsseedlistseed) 中列出其编译资产路径。

### 默认 `<GemName>/Assets/seedList.seed`

该文件包含一个编译资产路径和平台的列表，将自动显示在资产捆绑程序图形用户界面中，方便用户查找并包含在项目资产捆绑程序中。
当使用 [*AssetBundlerBatch* `--addDefaultSeedListFiles`](/docs/user-guide/packaging/asset-bundler/command-line-reference/#options-2)CLI 选项时，它们也会被包含在捆绑包中。
`seedList.seed`方法在您需要提供默认编译资产及其使用平台的即用列表时非常有用。

有关资产捆绑程序、`.seed` 文件和捆绑资产的更多信息，请参阅 [Asset Bundler文档](/docs/user-guide/packaging/asset-bundler/)。

{{<important>}}
文件名 `seedList.seed` 区分大小写，必须位于 Gem `Assets` 文件夹中，否则种子列表不会自动出现在 Asset Bundler GUI 中。
{{</important>}}

### Gem 依赖

Gem 依赖项`.xml`文件包含源资产列表，资产处理器将使用该列表创建包含所有编译资产路径和平台的种子列表。
使用 Gem 依赖项文件可以方便地指定具有全局模式的源资产，例如，当你想包含所有着色器变体时，但需要运行资产处理器。  

有关创建 Gem 依赖项的说明，请参阅 [O3DE 项目的默认依赖项](/docs/user-guide/packaging/asset-bundler/default-dependencies/)。


## Code (可选)

Gem 代码可以包含在 Gem 的 `CMakeLists.txt` 文件所选取的任何目录中，不过按照惯例，只有一个源代码模块的 Gem 会使用 `Code` 作为目录名。

包含代码的Gem称为**代码Gem**。有关开发代码Gem的更多信息，请参阅 [代码Gem规格](/docs/user-guide/programming/gems/code-gems)。

## Manifest 文件

每个 gem 都需要一个描述该 gem 的 `gem.json` 清单文件。请参阅 [Gem manifest 文档](./manifest) 以获取配置 清单的信息。

## 图标文件(可选)

每个 Gem 还可以包含一张可选图片，作为 GUI 项目配置工具的图标。该图片应为尺寸为 140x80 的 `.png`、`.gif` 或 `.jpg`，并命名为 `preview.<ext>`。Gem图标的相对路径在 `gem.json`中指定。

## CMakeLists.txt 文件

Gem还需要一个 `CMakeLists.txt` CMake 构建文件，以便 O3DE 构建系统能识别它们。这应该是一个用于构建 Gem 源代码的标准 CMake 文件，它应该位于 Gem 的根目录下。

对于使用二进制库或未作为源代码发布的可执行文件的 Gem，O3DE 有一个 [第三方软件包系统](/docs/user-guide/build/packages/)。

您的 `CMakeLists.txt` 文件与其他 CMake 文件一样。创建它时，请牢记以下要点：

* 为 Gem 生成的目标将与 `gem.json` 清单中定义的 Gem 名称相同。在调用 Gem 的 `CMakeLists.txt` 时，这将是活动目标。
* 您可以使用 O3DE 核心构建系统中可用的功能。请参阅源代码中 `cmake` 目录的内容。
* 避免使用`file(DOWNLOAD ...)`。O3DE 的软件包系统是一个强大的替代品，应予以使用。

## 创建只包含资产的Gem

当你创建一个没有指定模板的 Gem 时，它会根据 “DefaultGem ”模板创建。这样创建的 Gem 可同时打包代码和资产。你也可以通过`create-gem` 命令指定 “AssetGem ”模板，创建一个只提供资产的 Gem：

{{< tabs name="创建资产Gem" >}}
{{% tab name="Windows" %}}

```cmd
<engine>\scripts\o3de.bat create-gem --gem-path <path to create gem at> --template-name AssetGem
# or
<engine>\scripts\o3de.bat create-gem --gem-path <path to create gem at> --template-path <engine-root>\Templates\AssetGem
```

{{% /tab %}}
{{% tab name="Linux" %}}

```cmd
<engine>/scripts/o3de.sh create-gem --gem-path <path to create gem at> --template-name AssetGem
# or
<engine>/scripts/o3de.sh create-gem --gem-path <path to create gem at> --template-path <engine-root>/Templates/AssetGem
```

{{% /tab %}}
{{< /tabs >}}

## Gem 模板

O3DE 提供了一个 Gem 模板列表，可在 O3DE 版本库中的 [Templates](https://github.com/o3de/o3de/tree/development/Templates)目录或您的 O3DE 引擎安装中找到。
