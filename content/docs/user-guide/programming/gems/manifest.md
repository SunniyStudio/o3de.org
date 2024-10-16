---
linkTitle: Gem Manifest 文件
description: 了解定义 Open 3D Engine Gem 的Manifest文件的格式和结构。
title: Gem Manifest 文件 (gem.json)
weight: 400
---

`gem.json` 清单提供有关Gem的数据。每个 Gem 的根文件夹中都必须有一个 `gem.json` 文件。
## `gem.json` Manifest 内容

| 字段 | 是否必须 | 说明  |
|-|-|-|
| gem_name | **必须** | 该 gem 的名称。 该名称必须少于 64 个字符，只包含字母数字、'_' 或 '-' 字符，并以字母开头。 |
| display_name | **必须** | 该 gem 的用户友好显示名称。 该名称将显示在用户界面中，可以包含  `gem_name` 字段中不允许使用的字符。 |
| canonical_tags | **必须** | 用于区分清单类型的预定义标签列表，可以是 `Project`, `Template` 或 `Gem`。 对于Gem，该字段应始终为 `Gem`。  |
| compatible_engines | 可选 | 已知与该 gem 兼容的引擎名称和可选版本说明的列表：即 `o3de>=2.0.0`、`o3de-sdk==1.2.0`、`o3de-custom` 等。如果为空，则假定 gem 与所有满足 `engine_api_dependencies` 和 `dependencies` 字段中所有要求的引擎兼容。详见[Gem 版本](../../../user-guide/gems/gem-versioning.md)。|
| dependencies | 可选 | 您的 Gem 直接依赖的任何其他 Gem 的名称，带有可选的版本说明符：例如，`Atom>=1.0.0`、`PhysX==2.0.0`、`ScriptCanvas` 等。详情请参见 [Gem 版本](../../../user-guide/gems/gem-versioning.md) 。 |
| documentation_url | 可选 | 您的 gem 文档的 URL：即 https://www.mydomain.com/docs. |
| download_source_uri | 可选 | 包含 Gem 压缩包的`.zip`文件的 URI。 这是直接下载 Gem 的`.zip`压缩包：即 https://github.com/o3de/o3de-extras/releases/download/1.0/Example-2.0.zip  |
| engine_api_dependencies | 可选 | 引擎 API 依赖关系列表。 如果为空，则假定该 gem 与任何引擎 API 的所有版本兼容。详见 [Gem 版本](../../../user-guide/gems/gem-versioning.md) 。 |
| icon_path | 可选 | 图标图像文件（通常为 `preview.png`）文件名的相对路径。 |
| last_updated | 可选 | 该文件或 Gem 最后更新的日期，格式为 `YYYY-MM-DD`、`YYYY-MM-DD HH:MM:SS` 或 `YYYY-MM-DDTHH:MM:SS`。 |
| license | **必须** | 本 Gem 使用的许可证：即 Apache-2.0 或 MIT。 |
| license_url | **必须** | 许可证网站的 URL：即 https://opensource.org/licenses/Apache-2.0 或 https://opensource.org/licenses/MIT。 |
| origin | **必须** |该 Gem 的发起人名称：即 XYZ 公司。  |
| origin_uri | 可选 | 已被弃用，由 `download_source_uri` 代替。 |
| origin_url | 可选 | 该 gem 的发起者 URL：即 https://www.mydomain.com。 |
| platforms | 可选 | 该 Gem 已知的兼容平台列表：即 "Windows","Linux","Android","iOS","MacOS" 等。 |
| repo_uri | 可选 | 包含该 Gem 的 Gem 资源库的 URI。 |
| requirements | 可选 | 请注意您的Gem所需的任何条件：例如，该Gem要求您从 https://www.mydomain.com安装X。 |
| sha256 | 可选 | `origin_uri` 字段指向的 `.zip` 压缩包的 SHA-256 摘要。 测试时可以省略此字段，但我们强烈建议包含它。 |
| source_control_ref | 可选 | 此 Gem 版本的源代码控制参考。 可以是标签、提交哈希值或分支：如 `release-1.0.0`、`0462139`、`development` 等。  |
| source_control_uri | 可选 | 该Gem源代码库的 URI：即 https://github.com/o3de/o3de-extras.  |
| summary | **必须** | 对Gem的简短描述。 |
| type | **必须** | gem 的类型，可以是 `Code`, `Asset` 或 `Tool`。该字段在项目管理器中用作过滤器。 |
| user_tags | 可选 | 用于对Gem进行分类的用户自定义标记列表。该列表应始终包括您的 gem 名称，以及任何其他可帮助用户发现您的 gem 的常用标记：如 `Network`, `Rendering`, `Utility`, `Scripting`等。 |
| version | 可选 | `MAJOR.MINOR.PATCH` [语义版本](https://semver.org/)。随着 gem 的更改而更新。开发人员可使用 `gem.json` 中的 `dependencies` 字段来指明其他 gem 依赖项及其兼容的版本范围。详情请参阅[Gem 版本](../../../user-guide/gems/gem-versioning.md) 。|
| versions_data | 可选 | JSON 字典列表，其中包含每个 Gem 版本的更改字段。通常这些数据只存储在 `repo.json` 文件中，以跟踪每个 Gem 版本的更改和唯一下载 URI。 |


## `gem.json` Manifest 示例

### ScriptCanvas `Tool` Gem `gem.json` Manifest
ScriptCanvas 是引擎自带的 `Tool` 类型 gem，可提供编辑器和游戏功能。
```json
{
    "gem_name": "ScriptCanvas",
    "display_name": "Script Canvas",
    "version": "1.0.0",
    "license": "Apache-2.0 Or MIT",
    "license_url": "https://github.com/o3de/o3de/blob/development/LICENSE.txt",
    "origin": "Open 3D Engine - o3de.org",
    "origin_url": "https://github.com/o3de/o3de",
    "type": "Tool",
    "summary": "The Script Canvas Gem provides Open 3D Engine's visual scripting environment, Script Canvas.",
    "canonical_tags": [
        "Gem"
    ],
    "user_tags": [
        "Scripting",
        "Tools",
        "Utility"
    ],
    "icon_path": "preview.png",
    "requirements": "",
    "documentation_url": "https://o3de.org/docs/user-guide/gems/reference/script/script-canvas/",
    "dependencies": [
        "ScriptEvents",
        "ExpressionEvaluation",
        "GraphCanvas"
    ],
    "compatible_engines":[],
    "engine_api_dependencies":[]
}
```

### AudioEngineWwise `Code` Gem `gem.json` Manifest
AudioEngineWwise 是引擎自带的`Code`类型 gem，需要安装外部应用程序才能使用。
```json
{
    "gem_name": "AudioEngineWwise",
    "display_name": "Wwise Audio Engine",
    "version": "1.0.0",
    "license": "Apache-2.0 Or MIT",
    "license_url": "https://github.com/o3de/o3de/blob/development/LICENSE.txt",
    "origin": "Open 3D Engine - o3de.org",
    "origin_url": "https://github.com/o3de/o3de",
    "type": "Code",
    "summary": "The Wwise Audio Engine Gem provides support for Audiokinetic Wave Works Interactive Sound Engine (Wwise).",
    "canonical_tags": [
        "Gem"
    ],
    "user_tags": [
        "Audio",
        "Utility",
        "Tools"
    ],
    "icon_path": "preview.png",
    "requirements": "Users will need to download Wwise from the <a href='https://www.audiokinetic.com/download/'>Audiokinetic Web Site</a>.",
    "documentation_url": "https://o3de.org/docs/user-guide/gems/reference/audio/wwise/audio-engine-wwise/",
    "dependencies": [
        "AudioSystem"
    ],
    "compatible_engines":[],
    "engine_api_dependencies":[]
}
```

### DevTextures `Asset` Gem `gem.json` Manifest
DevTextures 是引擎自带的 `Asset` 类型 gem，可提供对原型和试生产有用的通用纹理资产。
```json
{
    "gem_name": "DevTextures",
    "display_name": "Dev Textures",
    "version": "1.0.0",
    "license": "Apache-2.0 Or MIT",
    "license_url": "https://github.com/o3de/o3de/blob/development/LICENSE.txt",
    "origin": "Open 3D Engine - o3de.org",
    "origin_url": "https://github.com/o3de/o3de",
    "type": "Asset",
    "summary": "The Dev Textures Gem provides a collection of general purpose texture assets useful for prototypes and preproduction.",
    "canonical_tags": [
        "Gem"
    ],
    "user_tags": [
        "Assets",
        "Debug",
        "Utility"
    ],
    "icon_path": "preview.png",
    "requirements": "",
    "documentation_url": "https://o3de.org/docs/user-guide/gems/reference/assets/dev-textures/",
    "dependencies": [],
    "compatible_engines":[],
    "engine_api_dependencies":[]
}
```
