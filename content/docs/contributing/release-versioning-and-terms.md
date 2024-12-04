---
linkTitle: 版本控制术语
title: Open 3D Engine版本控制和发布术语
description: 阅读本主题以了解 O3DE 的版本控制模型和发布术语
weight: 500
---

## Open 3D Engine (O3DE)版本编号

O3DE 使用`MAJOR.MINOR.PATCH` [语义版本控制](https://semver.org/)用于 `engine.json`, `gem.json` 和 `project.json` 文件中的所有 `version` 字段，以指示 API 和其他重要更改。

- `MAJOR` 用于 API 中断性更改
- `MINOR` 适用于添加新 API 或以非中断性方式更改 API 的非 API 重大更改
- `PATCH` 用于所有其他非 API 破坏性更改，通常是重要的修复

引擎显示版本使用`YY.MM.PATCH`，并且仅在 O3DE SDK 中使用。通过对 SDK 显示版本使用面向日期的方案，用户可以轻松判断引擎 SDK 的发布时间，但它不提供有关兼容性或所做更改类型的任何信息。

| 引擎显示版本     | 引擎版本    |
|------------|---------|
| 稳定的 SDK 版本 | 23.05.0 | 1.2.0              | 
| 开发 SDK 或源  | 00.00   | 2.0.0                  | 

## O3DE 发布术语

* **Stable**: 此功能已准备好供用户积极使用它进行开发。API 和功能可以被认为是稳定的，任何重要的新工作都将在进行更改之前捕获在 RFC 中。
* **Preview**: 此功能几乎完成并且很稳定。可能仍会有小的改变。
* **Experimental**: 此功能正在开发中。先不要依赖它;它可能会发生重大变化。

## `engine.json` 中的引擎版本信息

O3DE 引擎版本信息存储在引擎根目录的`engine.json` 文件中。 

| 字段 |描述 | CMake 全局属性 |
|---------------------|------------------------|------------------------|
| engine_name | 引擎的名称，在创建 SDK 时设置为`o3de-sdk`，在 O3DE GitHub 存储库源代码中设置为`o3de`。由工具用作引擎的标识符。 | | 
| display_version | `YY.MM.PATCH`的年份和月份版本显示在 SDK 安装程序和 Project Manager 中。此值仅在创建 SDK 时设置，在 O3DE GitHub 存储库源代码中为`00.00`。  | `O3DE_DISPLAY_VERSION_STRING` |
| version | `MAJOR.MINOR.PATCH` [语义版本控制](https://semver.org/)随着对引擎中的 API 进行更改，它会在 GitHub 存储库源代码中更新，并可用于一般引擎兼容性。开发人员可以使用其`gem.json` 或 `project.json`文件中的`compatible_engines`字段来指示其 Gem 或项目已知与哪个引擎版本兼容。 | `O3DE_VERSION_STRING` `O3DE_VERSION_MAJOR` `O3DE_VERSION_MINOR` `O3DE_VERSION_PATCH`|
| api_versions | `MAJOR.MINOR.PATCH` [语义版本控制](https://semver.org/)对于引擎中包含的框架和工具，它们是常见的依赖项。开发人员可以使用其`gem.json` 或 `project.json`文件中的`engine_api_dependencies`字段来指示其 Gem 或项目已知与哪些引擎 API 版本兼容。 | |
| build | 增量 SDK 内部版本号，仅在创建 SDK 时设置，在 O3DE GitHub 存储库源代码中为`0`。 | `O3DE_BUILD_VERSION` |
| O3DEVersion | 已弃用的版本字段已替换为`display_version`。 |
| O3DEBuildNumber | 已弃用的 build 字段，已替换为`build`。|

例：

开发人员可以使用其 `gem.json`中的`compatible_engines`字段来指示其 Gem 已知与开发中名为`o3de`版本 `2.0.0`或更高版本的任何引擎或名为`o3de-sdk`但版本为`1.2.0`的 SDK 引擎兼容。

```json
{
    "gem_name":"CustomGem",
    "compatible_engines": [
        "o3de>=2.0.0",
        "o3de-sdk==1.2.0"
    ]
}
```


### Engine API

在`engine.json`的`api_versions`字段中指定的 API 版本目前基于引擎的`Code`文件夹中的内容，并且作为简单但有用的方式提供给开发人员，以便在他们希望其 Gem 或工具与多个引擎版本兼容时指示兼容性，除非对引擎的`Code`文件夹中的 API 进行了重大更改。

| Engine API 字段|源路径 |
|---------------------|------------------------|
| `editor` | `Code/Editor` |
| `framework` | `Code/Framework` |
| `launcher` | `Code/LauncherUnified` |
| `tools` | `Code/Tools` |

例：

创建包含 Editor 工具的 Gem 的开发人员可以更新其`gem.json`文件，以表明已知该 Gem 与 Editor API 的任何非破坏性版本兼容。

```json
{
    "gem_name":"EditorToolGem",
    "engine_api_dependencies": [
        "editor~=1.0.0"
    ]
}
```
