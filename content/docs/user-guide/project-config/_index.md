---
linktitle: 项目配置
title: 项目配置
description: 了解 Open 3D Engine （O3DE） 中项目配置的基础知识，并获取有关 Project Manager 和 O3DE CLI 工具的详细信息。
weight: 100
---

**Open 3D Engine (O3DE)** 项目定义和配置构成游戏或其他应用程序的代码和资产集。[Gems](/docs/user-guide/gems)提供一些代码和资源。项目目录（有时称为“项目根目录”）包含您的项目。

要创建和管理项目，您可以使用基于 GUI 的 **Project Manager** 工具或 O3DE 命令行界面 （CLI）。本节中的主题提供了有关使用以下任一选项创建 O3DE 项目的详细信息。

|主题 |描述 |
| - | - |
| [了解 `project.json`](#understanding-project-json) | 详细了解项目清单（一个重要的项目配置文件）中的字段。 |
| [项目管理器](project-manager/) | 了解如何使用基于 GUI 的工具创建和管理项目。 |
| [添加和移除Gem](add-remove-gems/) | 了解如何在项目中添加和删除 Gem。 |
| [注册 Gem](register-gems/) | 了解如何从 O3DE 外部的源注册外部 Gem。 |
| [O3DE CLI 参考](cli-reference/) | 了解如何使用`o3de` Python 脚本创建和配置 O3DE 环境及其对象，包括引擎、项目和 Gem。|
| [故障排除](troubleshooting/) | Troubleshoot common issues that you might encounter during project configuration.排查在项目配置过程中可能遇到的常见问题。 |
| [本地 `project.json` 覆盖](#user-project-json) | 了解如何使用项目 `user` 文件夹中的 `project.json` 覆盖文件覆盖引擎名称和路径。 |

要构建项目，请使用 Project Manager 或 **CMake**。有关更多信息，请参阅以下任何资源：

* [入门教程](/docs/welcome-guide/create/) 在入门指南中。
* [项目管理器](project-manager/) 及其 **Build** 命令。
* [配置和构建](/docs/user-guide/build/configure-and-build)主题。

## 了解 `project.json` {#understanding-project-json}

每个项目的根目录都包含一个名为`project.json`的项目清单文件。此文件存储重要的工程配置属性。O3DE 在项目创建期间为您创建此文件。

要更改此文件中的任何属性，您可以使用文本编辑器手动编辑文件，或使用 `edit-project-properties` [O3DE CLI](./cli-reference) 命令编辑单个属性。此外，您还可以在 Project Manager 中编辑 `display_name` 属性。

下表描述了`project.json`中的每个属性。许多属性的默认值包含创建项目时给定的项目名称。我们将此项目名称指示为 "`<PROJECT_NAME>`"。

下面的"`<USER>`"占位符是指基于所使用的操作系统的用户主目录。例如，如果您的用户名是 “Foo”，并且您使用的是 Windows 计算机，则`<USER>`目录将为`C:\Users\Foo`。

|操作系统 |主目录路径 |
| --- | --- |
| Windows | `C:\Users\<YourUserName>` |
| Linux | `/home/<YourUserName>` |
| MacOs | `/Users/<YourUserName>` |



|属性 |必需 |描述 |默认 |
| --- | --- | --- | --- |
| **project_name** | **Required** | 项目的名称。`--project-name` O3DE CLI 参数使用此名称来标识项目。 | "`<PROJECT_NAME>`" |
| **display_name** | **Required** | 项目在 Project Manager 中的显示名称。 | "`<PROJECT_NAME>`" |
| **engine** | Optional | 此项目使用的引擎名称和可选版本说明符。在 O3DE 清单中注册引擎，位于`<USER>/.o3de/o3de_manifest.json`. | "o3de" |
| **engine_api_dependencies** | Optional | 引擎 API 依赖项的列表。 如果为空，则假定项目与任何插件 API 的所有版本兼容。 | "" |
| **external_subdirectories** | Optional |要包含在项目构建中的一个或多个目录的路径。您可以使用任何带有`CMakeLists.txt`文件的目录。当您使用 `register --external-subdirectory-project-path` O3DE CLI 命令将 Gem 注册到项目时，O3DE 会在此处添加其路径。 | [ ] |
| **canonical_tags** | **Required** | O3DE 清单中的一个标准字段，用于标识 O3DE 对象的类型。示例：“Gem”、“Project”。项目应使用 “Project” 标签。 | [ "Project" ] |
| **compatible_engines** | Optional | 已知与此项目兼容的引擎名称和可选版本说明符的列表：即 `o3de>=2.0.0`, `o3de-sdk==1.2.0`, `o3de-custom`等。如果为空，则假定项目与所有引擎兼容（如果它们满足 `engine_api_dependencies` 和 `gem_names` 字段中的所有要求）。 | [ ] |
| **gem_names** | Optional | 此项目使用的 Gem 名称和可选版本说明符的列表，包括项目中包含的 Gem：即`Atom`, `PopcornFX==1.2.0` 等。 | [ "`<PROJECT_NAME>`" ] |
| **icon_path** | Optional | 项目图标的路径和文件名。Project Manager 使用此图标。该文件必须位于项目根目录中。当前建议的大小为 210 像素宽 280 像素高。 | "preview.png" |
| **license** | **Required** | 项目使用的许可证，以及要包含的任何版权信息。请考虑提供许可证的 URL。此字段供 Project Manager 使用。 | "许可证 `<PROJECT_NAME>`使用的内容在这里：即 https://opensource.org/licenses/MIT" |
| **origin** | Optional | 项目的 URL，例如存储库 URL 或项目网站。此字段供 Project Manager 使用。 | "`<PROJECT_NAME>` 的主存储库位于此处：即 http://www.mydomain.com" |
| **project_id** | Optional | 创建项目时生成的 UUID。 | "< UUID >" |
| **restricted_name** | Optional | 要与项目关联的受限文件夹的名称。 | "`<PROJECT_NAME>`" |
| **summary** | Optional | 项目的简短描述。此字段供 Project Manager 使用。 | "`<PROJECT_NAME>`的简短概述" |
| **user_tags** | Optional | 要与项目关联的任何关键字标签。例子： "Physics", "Assets", "AWS". 有关 Gem 使用的标准标签集的示例，请参阅项目管理器  **Gem Catalog**。Project Manager 使用这些标记进行文档、搜索和筛选。 | [ "`<PROJECT_NAME>`" ] |
| **version** | Optional | `MAJOR.MINOR.PATCH` [semantic version](https://semver.org/) 该更新会随着对项目的更改而更新。 | "1.0.0" |

## 本地 `project.json` 覆盖 {#user-project-json}

在本地安装了多个插件时，让您的项目使用具有特定名称或路径的插件，而无需更改与团队共享的`project.json`文件，这可能很有用。 O3DE 将使用项目文件夹中的`user/project.json`覆盖文件中的设置来覆盖项目根目录中的`project.json`文件设置。

要更改 `user/project.json`覆盖文件中的任何属性，您可以使用文本编辑器手动编辑文件，或使用带有`--user` 选项的 `edit-project-properties` [O3DE CLI](./cli-reference)命令编辑单个属性。 当您添加项目时，项目经理将在此文件中设置`engine_path` 。
2
| 字段 | 说明 | 示例 |
| --- | ---| --- |
| **engine** | 使用可选说明符覆盖引擎名称 |  "o3de==1.2.3" |
| **engine_path** | 要使用的引擎的绝对本地路径 | "C:/o3de-custom" |


示例:

开发人员正在使用 [git worktrees](https://git-scm.com/docs/git-worktree) 同时处理 o3de GitHub 存储库的多个分支。 每个引擎实例的 `engine_name` 都是相同的，因此开发人员使用 `user/project.json` 文件将 `engine_path` 设置为他们想要使用的特定引擎。

示例:

引擎开发人员所在的团队将引擎 SDK 和项目存储在源代码控制中，但将引擎源代码保存在单独的存储库中。 开发人员在其本地计算机上有两个存储库：
```
/home/user/repo1 
            /o3de-sdk <-- engine SDK
            /project  <-- project
/home/user/repo2      
            /o3de     <-- engine source code
```
由于开发人员是从源代码构建引擎，并且不想使用 SDK，因此他们在项目的`/home/user/repo1/project/user/project.json`覆盖文件中将 `engine_path` 设置设置为`/home/user/repo2/o3de`。
