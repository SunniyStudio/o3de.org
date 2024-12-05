---
linkTitle: 第2章 创建新项目
title: 第2章 创建新项目
description: 第2章 创建新项目
---
# 第2章 创建新项目
## 介绍
{{< note >}}
本章的源代码和资源可以在 GitHub 上找到：
https://github.com/AMZN-Olex/O3DEBookCode2111/tree/ch02_new_project
{{< /note >}}

本章将介绍：
* 如何创建新项目。
* 构建项目。
* 什么是 Asset Processor？
* 在 O3DE 编辑器中打开一个关卡。

{{< note >}}
您可以在以下位置找到官方入门指南：
https://www.o3de.org/docs/welcome-guide/create/creating-projects-using-project-manager/
{{< /note >}}

## O3DE 命令行界面
尽管可以使用 O3DE Project Manager 创建新项目，但我将向您展示如何使用命令行工具来完成它，因为我相信它将来会为您提供更多功能。此 CLI 可在`C:\O3DE\21.11.2\scripts\o3de.bat`中找到。
```shell
PS C:\git\book> C:\O3DE\21.11.2\scripts\o3de.bat -h
usage: o3de.py [-h]
```

这是创建新项目的命令行：
```shell
o3de.bat create-project -pp C:\git\book\MyProject
```
这将在`C:\git\book\MyProject`处创建一个新项目。

{{< note >}}
参数 `-pp` 代表项目路径。该脚本会将项目名称推断为 MyProject。
{{< /note >}}

有更多选项和命令可供 o3de.bat但这对于我们来说已经足够了。

确认已创建 `C:\git\book\MyProject` 并包含各种项目文件。

{{< note >}}
我建议你立即将你的项目置于 Git 源代码控制之下。提醒一下，我在 `c:\git\book` 文件夹下创建项目。如果您正在阅读本书，则可以使用以下命令克隆随附的源代码：
```shell
mkdir c:\git\
cd c:\git
git clone https://github.com/AMZN-Olex/O3DEBookCode2111 book
cd book
git checkout origin/ch02_new_project
```
随着您阅读本书的进度，切换到相应的分支。
{{< /note >}}

## 链接项目和 O3DE
如果您从全新的 O3DE 安装开始，并使用上述步骤创建了一个新项目，则该项目将已配置为使用安装在`C:\O3DE\21.11.2`中的 O3DE 引擎。但是，如果您安装了 O3DE 的多个副本，或者您正在不同的 O3DE 版本之间切换，那么了解项目如何声明它们使用的引擎是很有价值的。

涉及两个重要文件。第一个是 `project.json` 在你的项目中找到的 `C:\git\book\MyProject\project.json` 中。第二个是` C:\Users\<user>\.o3de
\o3de_manifest.json`。

`project.json` 指定要与 `engine` 属性一起使用的 O3DE 引擎安装的名称。
```json
{
  "project_name": "MyProject",
  ...
  "engine": "o3de-sdk"
}
```

`o3de_manifest.json` 指定所有引擎安装及其相应的名称。
```json
"engines_path": {
  "o3de-sdk": "C:/O3DE/21.11.2"
}
```

如果您曾经移动引擎安装或希望尝试新引擎，则需要相应地更新这些条目。

{{< note >}}
引擎和项目的位置完全取决于您，只要上述两个文件一致即可。
{{< /note >}}








