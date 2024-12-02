---
title: 设置注册表
linktitle: 设置注册表
description: 了解如何使用设置注册表为 Open 3D Engine （O3DE） 中的工具和应用程序提供设置和配置选项。
weight: 350
---

使用设置注册表，您可以为 Open 3D Engine （O3DE）** 应用程序和工具提供设置和配置。

设置存储在扩展名为`.setreg` 和 `.setregpatch`的 JSON 文件中。您可以使用 [JSON 指针语法](https://tools.ietf.org/html/rfc6901) 定义、查询和更改 O3DE 应用程序设置。在以下示例中，JSON 指针路径为`"/O3DE/CustomSetting"`:

```JSON
{
    "O3DE": {
        "CustomSetting": false
    }
}
```

可以通过 [JSON Patch](https://tools.ietf.org/html/rfc6902) 或 [JSON Merge Patch](https://tools.ietf.org/html/rfc7386) 格式文件来合并和修改设置。

您可以通过以下方法与 O3DE 中的设置注册表进行交互：

* [命令行参数](./command-line-arguments)
* [AZ::Console 系统](./az-console)
* [脚本 (Lua, Script Canvas, Python)](./script-api)

## 主题

| 主题 | 说明 |
| - | - |
| [命令行参数和设置注册表](./command-line-arguments) | 了解如何从命令行修改 Settings Registry 并访问提供给应用程序的命令行。 |
| [Settings Registry Script API](./script-api) | 通过 Python 和 Lua 的示例了解 Settings Registry Script API。 |
| [使用控制台命令访问设置注册表](./az-console) | 使用 Console 命令读取和修改 Settings Registry。 |
| [从设置注册表发出控制台命令](./issue-az-console-commands) | 了解如何使用设置注册表运行 Console 命令。 |
| [使用 C++ 将 Settings Registry 输出到 Stream](./output-settings-registry) | 使用 C++ 将 Settings Registry 输出到 IO 流。 |
| [设置注册表开发人员指南](./developer-documentation) | 提供有关 Settings Registry 的详细开发人员信息。为多种情况提供了设置注册表示例。详细介绍了 CMake Build Generation 系统、Settings Registry 和 Gem Module System 之间的交互。 |
| [Gem 加载和 Settings Registry](./gem-loading) | 了解如何使用 Settings Registry 加载 Gem 模块和配置 Gem 设置。 |
| [设置注册表链接](./import-settings-registry) | 了解设置注册表如何触发其他设置注册表文件的导入。 |

## 相关主题

| 主题 | 说明 |
| --- |------------------------------|
| [Gem模块系统](/docs/user-guide/programming/gems/overview/) | 说明 O3DE 如何在 C++ 中加载和初始化 Gem。 |
