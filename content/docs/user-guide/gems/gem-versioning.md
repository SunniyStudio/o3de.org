---
linktitle: Gem 版本控制和兼容性
title: Gem 版本控制和兼容性
description: 概述 Open 3D Engine Gem 如何声明和使用版本、兼容性和依赖项信息。
weight: 400
---

Gem 版本控制是传达对 Gem 所做更改类型的重要方式。

## O3DE 版本编号

O3DE 使用`MAJOR.MINOR.PATCH` [语义版本控制](https://semver.org/) 用于 `engine.json`, `gem.json` 和 `project.json` 文件中的所有`version`字段。

- `MAJOR` 用于 API 中断性更改
- `MINOR` 适用于添加新 API 或以非中断性方式更改 API 的非 API 重大更改
- `PATCH` 用于所有其他非 API 破坏性更改，通常是重要的修复

示例：如果对版本`2.0.1`的 Gem 进行了中断性 API 更改，则`MAJOR`版本将递增，`MINOR`和`PATCH`将重置为`0`，从而导致新版本为`3.0.0`。 有关更多示例，请参阅 [语义版本控制](https://semver.org/) 页面。

## O3DE 版本说明符


兼容引擎和依赖项的列表使用基于 [PEP 440](https://peps.python.org/pep-0440/#version-specifiers)的`<name><version specifier>`格式。

例如:

| 名称 | 版本说明符 | 合并 | 说明 |
|------|-------------------|----------|-------------|
| o3de | >=2.0.0           | o3de>=2.0.0 | o3de 版本 2.0.0 或更高 |
| o3de-sdk | == 1.2.0      | o3de-sdk==1.2.0 |  o3de-sdk 版本 1.2.0 | 
| Atom | ~=2.0.0           | Atom~=2.0.0 | Atom 版本 2.0.0 以上，但不包含 3.0.0 |

已知与名为`o3de-sdk` 版本`2.0.1`的引擎兼容的 Gem 可以在`gem.json`的`compatible_engines`字段中包含`o3de-sdk==2.0.1`


## `gem.json` 中的Gem 版本和兼容性信息

Gem 版本、依赖关系和兼容性信息存储在每个 Gem 根目录下的`gem.json`文件中。如果`gem.json`中没有兼容性或依赖项信息，或者字段为空，则假定 Gem 与所有引擎兼容，并且没有 Gem 依赖项。

<table class="fixed-table wrapped"><colgroup><col style="" /><col style="" /><col style="" /><col style="" /></colgroup>
<thead>
<tr>
<th>Field</th>
<th>Description</th>
<th>Default</th>
<th>Example</th>
</tr>
</thead>
<tbody>
<tr>
<td>version</td>
<td>`MAJOR.MINOR.PATCH` [语义版本](https://semver.org/) ，该版本会随着对 Gem 的更改而更新。开发人员可以使用`gem.json`中的`dependencies`字段来指示其他 Gem 依赖项及其兼容的版本范围。</td>
<td>1.0.0</td>
<td>1.2.3</td>
</tr>
<tr>
<td>compatible_engines</td>
<td>已知与此 Gem 兼容的引擎名称和版本说明符的列表。如果为空，则假定 Gem 与所有引擎兼容（如果它们满足`engine_api_dependencies`和`dependencies`字段中的所有要求）。</td>
<td>[ ]</td>
<td>

```json
{
    "compatible_engines":[
        "o3de-sdk==1.2.0",
        "o3de>=2.0.0"
    ]
}
```

此 Gem 与名为`o3de-sdk`版本`1.2.0`的引擎或任何名为`o3de`版本`2.0.0`或更高版本的引擎兼容。

</td>
</tr>
<tr>
<td>engine_api_dependencies</td>
<td>引擎 API 依赖项的列表。 如果为空，则假定 Gem 与任何引擎 API 的所有版本兼容。</td>
<td>[ ]</td>
<td>

```json
{
    "engine_api_dependencies":[
        "editor>=1.0.0"
    ]
}
```

此 Gem 依赖于`editor`API 版本`1.0.0`或更高版本。
</td>
</tr>
<tr>
<td>依赖项</td>
<td>Gem 依赖项的可选列表。</td>
<td>[ ]</td>
<td>

```json
{
    "dependencies":[
        "Atom>=1.0.0",
        "PhysX==2.0.0"
    ]
}
```
此 Gem 依赖于名为 `Atom` 版本`1.0.0`或更高版本的 Gem，以及`PhysX`版本`2.0.0`。
</td>
</tr>
<tr>
<td>平台</td>
<td>兼容平台的列表。</td>
<td>[ ]</td>
<td>

```json
{
    "platforms":[
        "Windows",
        "Linux",
        "Android",
        "MacOS"
    ]
}
```
 已知此 gem 与`Windows`, `Linux`, `Android` 和 `MacOS`兼容
</td>
</tr>
</tbody>
</table>

## 选择要使用的版本字段

`compatible_engines`字段是列出已知 Gem 兼容的引擎的一种简单方法，并且此字段中的信息将在 Project Manager 中向用户显示。O3DE 社区维护着一个名为`o3de-sdk`的引擎 SDK，而 GitHub 源代码中的引擎名称为`o3de`，因此建议至少考虑为这些常用引擎提供兼容性信息。

{{< note >}}
此时，您不能在`compatible_engines`版本说明符中使用引擎`display_version`，而只能使用`version`。
{{< /note >}}

`dependencies` 字段应该始终用于列出直接的 gem 依赖项，如果您不知道要依赖哪些版本，只需包含 gem 名称，而不带版本说明符部分。

示例：以下`gem.json` `dependencies`条目显示了对`Atom`的任何版本以及`PhysX`版本`1.0.0` 或更高版本的依赖关系。
```json
{
    "dependencies": [
        "Atom",
        "PhysX>=1.0.0"
    ]
}
```

如果您知道您的 Gem 与任何引擎兼容，只要它所依赖的 Gem 依赖项或 API 没有重大更改，则使用`dependencies`或`engine_api_dependencies`字段将需要更少的未来更新。


## 引擎 Gem 兼容性

引擎 Gem 的`dependencies`字段中不包含版本说明符，也不包含任何`compatible_engines`或`engine_api_dependencies`，因为已知它们与引擎 API 和引擎中的其他 Gem 兼容并经过测试，因此不需要维护兼容性信息。

## 注册 Gem 版本

当使用`o3de` CLI 或 Project Manager 注册 Gem 时，它将检查 Gem 是否与当前使用的引擎兼容。 如果发现任何问题，将向用户显示这些问题，并且不会注册 Gem。 如果用户仍想注册 gem，他们可以使用`--force`参数来绕过兼容性检查。

## Gem 版本选择

可以注册同一 Gem 的多个版本，不同的项目可以使用不同的版本。 配置项目后，将选择具有最高版本号的兼容 Gem。

例如:
`project.json`文件指定了对 `foo>=1.0.0`的 gem 的依赖关系，如果注册了名为`foo`的 gem 的 `1.0.0` 和 `2.0.0` 版本，则将使用具有最高版本`2.0.0`的 gem，只要该 gem 没有兼容性问题。 如果用户想使用`1.0.0`，他们可以将`project.json`中的 gem 依赖项更改为`foo==1.0.0`，以指示应该使用版本 `1.0.0`。

## Gem 依赖项解析

使用 CMake 配置项目时，将运行 Gem 依赖项解析，以确保使用正确的 Gem 进行构建。 如果 Gem 依赖项解析因无法满足要求而失败，则会显示一条错误消息，其中包含所考虑的 Gem 列表。 虽然不建议这样做，但如果 `O3DE_DISABLE_GEM_DEPENDENCY_RESOLUTION` CMake 变量由于错误而失败，或者对于您的用例来说是不必要的，例如，如果您提供自己的 SDK 并包含所有 Gem，并且未在项目中使用任何外部 Gem，则可以使用 '' CMake 变量来禁用 Gem 依赖项解析。
