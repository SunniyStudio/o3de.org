---
linkTitle: 代码Gem
title: 代码Gem规范
description: Open 3D Engine中的代码Gem概述。
weight: 300
---

代码Gem包含可扩展 **Open 3D Engine (O3DE)** 编辑器或为您的 O3DE 项目集成功能和逻辑的源代码。它们还包含源代码所需的资产，如界面元素以及用作测试和示例的资产。

## 创建Gem

按照 [创建O3DE Gem](/docs/user-guide/programming/gems/creating)创建一个基于**DefaultGem模板**的Gem。代码Gem和资产Gem具有相同的Gem目录结构，您可以使用相同的命令来创建它们。

## Gem代码

Gem 的代码组件位于 `Code` 目录中。DefaultGem 模板会在 Gem 的 `Code/Source` 目录中提供以下模板代码。

![An image of the file directory of Gem code](/images/user-guide/programming/gems/defaultgem-template-directory-code-source.png)

您可以从自己创建的 Gem 或引擎源代码的 `Templates/DefaultGem` 目录中的 DefaultGem 模板浏览 Gem 的源代码。

以下文件是 Gem 的代码组件。

### `<Gem>ModuleInterface.h`, `<Gem>Module.cpp`

`<Gem>ModuleInterface` 是主要的 Gem `AZ::Module`类，包含将 Gem 功能连接到 O3DE 的入口函数。DefaultGem 模板已经通过调用 `AZ_DECLARE_MODULE_CLASS` 宏设置了连接到 O3DE 的 Gem 模块。

有关模块及其在 O3DE 中职责的更多信息，请参阅 [Gem 模块系统](/docs/user-guide/programming/gems/overview/) 概述。

### `<Gem>SystemComponent.cpp`, `<Gem>SystemComponent.h`

`<Gem>SystemComponent` 类是一个全局单例类，负责管理 Gem 的代码。Gem模块注册了Gem系统组件，该组件负责管理Gem模块的生命周期：初始化、激活和停用。Gem系统组件在Gem加载时激活，在Gem卸载时停用。

Gem 系统组件允许 Gem 模块内的组件通过连接到其他 Gem 的 EBus，与其他 Gem 的组件进行通信。它还处理初始化和关闭、事件、内存分配和调试器连接。

### `<Gem>EditorModule.cpp`

`<Gem>EditorModule`类包含在编辑器中使用Gem的逻辑。它与处理Gem逻辑的主Gem模块是分开的。例如，如果Gem实现了一个组件，主Gem模块会实现组件的逻辑，而编辑器模块会在编辑器中实现组件的用户界面。

### `<Gem>EditorSystemComponent.cpp`, `<Gem>EditorSystemComponent.h`

`<Gem>EditorSystemComponent` 类是管理编辑器模块的系统组件类。你可以使用该类来编程编辑器功能。例如，如果你的 Gem 实现了在 O3DE 编辑器中出现的组件或窗口。

## 构建 Gem

### 先决条件

您必须先注册您的Gem，然后才能构建它。

如果你使用 `o3de` 脚本中的[`create-gem`命令](creating) 创建了一个 Gem，它会自动注册。

有关将Gem注册到项目的更多信息，请参阅 [将Gem注册到项目](/docs/user-guide/project-config/register-gems/)。

### 使用命令行构建 Gem

将一个 Gem 注册到引擎、项目或全局 `o3de_manifest.json` 之后，就可以使用 CMake 构建该 gem，与 [构建引擎和项目](/docs/user-guide/build/) 的方式相同。[添加到项目](/docs/user-guide/project-config/add-remove-gems/) 的 Gem 会在构建项目时自动构建。不过，在开发一个 Gem 时，你可能想单独构建它，以避免重建整个项目。

要单独构建一个 Gem，请在适合你的 [引擎构建类型](/docs/welcome-guide/setup/setup-from-github/building-windows/#build-the-engine) 的目录中，使用 `cmake --build` 命令并指定构建方案和配置。使用 `--target` 标志指定要构建的 Gem 模块： 主 Gem 模块使用 `<Gem Name>`，编辑器模块使用 `<Gem Name>.Editor`。 如果你的 Gem 包含静态库，你也可以将它们包含在要构建的目标列表中。

下面的示例显示了为名为 `MyGem` 的Gem构建编辑器和主Gem模块的命令：

```cmd
cmake --build build/windows --target MyGem.Editor MyGem --config profile -- -m
```

当你构建一个 gem 时，构建系统也会构建该 gem 的依赖项，这些依赖项列在该 gem 的 `code/CMakeLists.txt` 文件中。

### 在Visual Studio中构建Gem

1. 打开与 [引擎构建类型](/docs/welcome-guide/setup/setup-from-github/building-windows/#build-the-engine) 相应的 Visual Studio 构建解决方案。对于源代码引擎，这是引擎的根目录；对于预构建的 SDK 引擎，这是项目的根目录。
1. 在**解决方案资源管理器**中找到Gem的 `Code` 目录。在大多数情况下，该目录位于 `Gems\<MyGem>\Code`。 该目录包含 Gem 的所有构建目标，包括主模块 `<MyGem>` 和编辑器模块 `<MyGem>.Editor`。
1. 确保在顶部工具栏中选择了正确的构建配置。有关构建配置的信息，请参阅 [生成的构建配置](/docs/user-guide/build/configure-and-build/#generated-build-configurations). 
1. **右击**模块，从菜单中选择**构建**，即可构建模块。

    ![Building a Gem in Visual Studio with the context menu of the Solution Explorer](/images/user-guide/programming/gems/VS-build-gem.png)
