---
linkTitle: 添加 Lua 脚本
description: 使用 Lua Script （Lua 脚本） 组件将脚本功能添加到 Open 3D Engine 中的游戏实体。
title: 将 Lua 脚本添加到组件实体
toc: true
weight: 150
---

O3DE 使您可以使用 **Lua Script** 组件轻松地将脚本功能添加到您的游戏实体中。以下步骤向您展示如何在 O3DE Editor 中执行此操作。

**在 O3DE 编辑器中将 Lua 脚本添加到组件实体**

1. 在 **Entity Outliner** 视图窗格可见的情况下，**左键单击**要向其添加 Lua 脚本的实体。

1. 单击 **Add Component**，在下拉菜单的 `Scripting` 类别中，选择 **Lua Script**。

    ![Lua Script component](/images/user-guide/scripting/lua/add-lua-component.png)

1. **Lua Script** 组件显示在 **Entity Inspector**中。 点击{{< icon browse-edit-select-files.svg >}} 文件选择按钮从要使用的文件层次结构中选择 Lua 脚本。

    {{< note >}}
您可以选择 `.lua` 文件（原始文本副本）或 `.luac` 文件（脚本的预编译版本）。功能应该是相同的。预编译版本更可取，因为它加载速度更快，并且通常更小。但是，如果您遇到任何问题，可以使用`.lua`文件。
{{< /note >}}

1. 脚本加载完成后，单击 {{< icon "open-in-internal-app.svg" >}} 启动 **Lua Editor** 并更改您的脚本。
