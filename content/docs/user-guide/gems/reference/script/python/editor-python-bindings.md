---
linkTitle: Editor Python Bindings
title: Editor Python Bindings Gem
description: The Editor Python Bindings Gem provides Python commands for Open 3D Engine (O3DE) Editor functions.
toc: true
---

**Open 3D Engine （O3DE）** 编辑器中的一些任务很繁琐，或者很容易自动化。为了自动执行 Editor 任务，O3DE Editor 支持通过 Python 绑定编写脚本。这些绑定通过 Python 编辑器绑定 Gem 启用，并通过 O3DE 编辑器中嵌入的 Python 3 库使用。您可以通过 O3DE 的 Python 控制台访问 Python 读取、评估、打印循环 （REPL），或者使用在启动时加载和运行 Python 脚本的参数启动 Editor。

## 启用 Editor 自动化

通过为项目选择 Python 编辑器绑定 Gem，然后重新构建编辑器，可以启用编辑器自动化。启用 Python 绑定不需要特定的配置 （debug、profile、release）。由于绑定是通过您为项目选择的 Gem 启用的，因此您需要确保为要自动化的所有项目启用此 Gem。

## 使用 Editor 自动化

开始使用编辑器自动化的最简单方法是使用 O3DE 编辑器中提供的 REPL 并尝试一些命令。通过选择 **Tools > Other > Python Console** 打开此 REPL。Python 控制台将在新选项卡中打开，通过该选项卡，您可以访问一个控制台，该控制台显示 Python 的输出、REPL 输入以及可用命令的完整参考。要访问引用，请选择 Python 控制台右下角的 **？** 图标。

您还可以通过选择 **Tools > Other > Python Scripts**（其他 Python 脚本）来访问一组可用脚本，包括编辑器中常见任务的一些示例。这些脚本存储在目录中，具体取决于其范围。仅用于您的项目的脚本存储在 `<project name>\Editor\Scripts` 目录中，而与 Gem 一起使用的脚本存储在`\Gems\<gem_name>\Editor\Scripts`中。

编辑器自动化主要通过 Event Bus （EBus） 系统驱动。在使用 Editor 绑定之前，您应该熟悉 EBus 的基础知识。要了解编辑器自动化系统使用的一些特定总线，请查看 Python 编辑器绑定 Gem 示例。
