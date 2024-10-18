---
linktitle: 项目和Gem模板
title: 项目和Gem模板
description: 了解 O3DE 中可用的项目和 gem 模板
weight: 100
toc: true
---

O3DE 包含一个模板系统，您可以根据这些模板创建新项目、新 gem 或引擎的新代码片段。 安装程序或 github 中的默认引擎包含一些基本模板，但更多模板可作为 gem 或软件源的一部分下载并添加到引擎中。

您可以在引擎根目录下的 `templates` 子文件夹中找到所有现有模板。 它们的操作方法是将模板文件复制到用户选择的文件夹中，并将这些文件中的某些占位符文本（如项目名称）替换为用户提供的实际名称。

## 引擎中包含的项目模板
这些模板用于整个项目，可使用项目管理器创建，然后在编辑器中打开进行操作。
有关如何通过模板创建项目的指南，请参阅 [使用项目管理器创建项目](/docs/welcome-guide/create/creating-projects-using-project-manager)。

### DefaultProject
对于使用 O3DE 的游戏项目来说，这是一个很好的全面起点。 它附带了一个脚本，用于将项目导出为可发布的独立软件包。 它默认激活了许多Gem，包括用户界面、基本原型设计、脚本画布和动画等Gem。 使用该模板需要安装 C++ 编译器，其中包含一个基本的 c++ 游戏模块，可用于编写核心游戏逻辑和组件。

### ScriptOnlyProject
它的 gem 选择与 DefaultProject 相同，但禁止所有 C++ 编译和链接。 该项目仍可使用引擎的 Lua 和 Script Canvas 功能，但不能嵌入自定义的 C++ 代码，也不能使用其他未预编译的 gem 或模块。 它可用于快速了解引擎和编辑器，而无需编译项目、下载第三方库或安装编译器或链接器。  你可以在以后将纯脚本项目转换为常规项目。

有关此功能的更多信息，请参阅 [纯脚本'快速启动'项目](/docs/user-guide/build/script-only-projects.md)文档。

### MinimalProject
该模板尽可能少激活Gem，是一个简易的起点。 不建议原封不动地使用该模板，而是从它开始，然后激活项目可能需要的Gem。  由于它激活的Gem越少越好，包含的其他Gem也越少越好，因此它的启动时间和资产编译时间都是最快的。

## 引擎中包含的著名Gem模板
这些模板用于制作您自己的自定义 gem，这些 gem 可用来扩展引擎，并在各个项目中重复使用。
要从模板创建 gem 和其他组件，必须使用命令行实用程序，类似于 [使用 CLI 创建项目](/docs/welcome-guide/create/creating-projects-using-cli) 中使用 CLI 创建项目的方式，但使用不同的命令 (`create-gem`)和不同的参数 (`--gem-path`)来提供输出文件夹。

linux 示例：
```shell
scripts/o3de.sh create-gem --help
scripts/o3de.sh create-gem --gem-path $HOME/O3DE/Projects/MyProject/Gem/MyGem -tn AssetGem
```

windows 示例：
```cmd
scripts\o3de create-gem --help
scripts\o3de create-gem --gem-path %USERPROFILE%\O3DE\Projects\MyProject\Gem\MyGem -tn AssetGem
```

### AssetGem
该模板可用于创建仅包含资产、无代码且无需 C++ 编译即可使用的 gem。 非常适合创建资产桶，以便在项目间重复使用，或在不附带任何代码的情况下打包和发布。 它仍可包含 Lua 脚本、Script Canvas 或 Python 工具。

### CppToolGem
该模板可用于为编辑器创建新的 C++ 工具。 它包含一个 C++ 代码脚手架，可使组件在编辑器中注册，并为您提供了开始添加自定义工具代码的位置。

### PythonToolGem
该模板可用于为用 Python 编写的编辑器创建新工具。 Python 可用于访问引擎和编辑器 API，还可使用 Qt For Python 创建新面板和 UI 元素。 它包含一个引导脚本，如果 gem 处于活动状态，该脚本将在启动时自动调用，还包含一个用 Python 编写的 Qt 对话框示例，该对话框可在编辑器的视图窗格系统中注册。

### DefaultGem
本模板用于开发更全面的 gem，其中包含以独立模式在引擎中运行的代码，以及在编辑器和工具端运行的代码。 它包含这两个模块，以及用于在适当位置注册和激活这两个模块的模板。

### GraphicsGem
该模板用于创建包含 Atom 渲染 API 的 gem。 最适合在引擎中添加新的渲染功能。 它包括一个向引擎注册的模块和一个挂钩 Atom 的功能处理器，让你可以立即开始编写你的渲染功能。

### UnifiedMultiplayerGem
该模板包含一个 Gem 的脚手架，该 Gem 将插入引擎的多人系统，并支持客户端和服务器之间的不同行为（包括具有两种行为的统一启动器）。它包括一个工具端模块以及一个引擎端模块和组件。 更多信息，请参阅 [多人游戏文档](/docs/user-guide/networking/multiplayer)。

### PrebuiltGem
本模板包含可作为预构建库（只有头文件，没有源代码）发布的 gem 的说明和起始布局。 它展示了如何与现有的静态库或共享库挂钩，而不是发送源代码，以及如果您希望 gem 无需在用户端编译即可运行，可以如何构建 gem 的可发送树。 预构建 gem 是发布可在纯脚本模式下运行的 gem 的唯一方法。

## 引擎中包含的其他模板
这些模板可用于节省模板代码的时间，并通过告诉 CLI 将其注入到现有项目或 gem 中来使用，因为它们包含代码片段或资产，并不是功能独立的。 应使用 CLI 命令`create-from-template` （从模板创建）来实例化模板，其中的`-dp` 目标路径是放置实例化模板的位置，`-tn` 参数是模板的名称。

linux 示例:
```shell
scripts/o3de.sh create-from-template --help
scripts/o3de.sh create-from-template -dp $HOME/O3DE/Projects/MyProject/Gem/MyGem/MyComponent -tn DefaultComponent
```

windows 示例:
```cmd
scripts\o3de create-from-template --help
scripts\o3de create-from-template --gem-path %USERPROFILE%\O3DE\Projects\MyProject\Gem\MyComponent -tn DefaultComponent
```

### DefaultComponent
该模板包含一个可注入现有 gem 或项目的简单组件。 在向现有代码库中添加新组件时，可节省模板键入的时间。它包含一个接口头，允许你立即开始添加 EBus 消息与组件对话。

### GemRepo
该模板包含一个Gem库的脚手架。 用户可以将Gem仓库的 URL 添加到自己的项目管理器中，然后就可以使用该仓库中的Gem了。 更多信息，请参阅[Gem版本库概览](/docs/user-guide/gems/repositories/overview)。

### RemoteRepo
该模板包含远程存储库的脚手架。 更多信息，请参阅 [远程版本库](/docs/learning-guide/tutorials/remote-repositories/create-remote-repository) 文档。

### ScriptCanvasNode
该模板包含 Script Canvas 节点的脚手架，可添加到现有代码库中，为引擎和编辑器提供新的 Script Canvas 节点。 实例化此模板后，将生成 XML 文件以及代码、头文件和 CMake 文件，以便连接到 Script Canvas 系统。 更多信息，请参阅 [Script Canvas自定义节点](/docs/user-guide/scripting/script-canvas/programmer-guide/custom-nodes/_index.md)文档。


