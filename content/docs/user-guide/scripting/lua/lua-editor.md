---
linkTitle: Lua 编辑器
title: Open 3D Engine 的 Lua 编辑器简介
description: 了解 Open 3D Engine 中的 Lua 编辑器。
toc: true
weight: 300
---

O3DE **Lua 编辑器** （Lua IDE） 提供了一个直观的集成开发环境 （IDE），让您在创建或扩展游戏时可以轻松编写、调试和编辑 Lua 脚本。Lua Editor 是一个独立的应用程序，但可以使用 Edit Mode 工具栏直接从 **O3DE Editor** 打开。

## 编辑

Lua Editor 可以同时打开多个脚本。每个脚本在编辑器中都有自己的选项卡。该编辑器提供了一组用于文本编辑的标准功能，但也包括用于编辑源代码的有用功能。

下表总结了编辑和调试时可用的选项。

|**行动** |**键盘快捷键** |
| --- | --- |
|注释所选块 |**Ctrl+K** |
|复制 |**Ctrl+C** |
|切割 |**Ctrl+X** |
|查找 |**Ctrl+F** |
|在打开的文件中查找 |**Ctrl+Shift+F** |
|查找下一个 |**F3** |
|折叠源函数 |**Alt+0** |
|前往线路 |**Ctrl+G** |
|浆料 |**Ctrl+V** |
|快速查找本地 |**Ctrl+F3** |
|快速查找本地反向 |**Ctrl+Shift+F3** |
|重做 |**Ctrl+Y** |
|替换 |**Ctrl+R** |
|在打开的文件中替换 |**Ctrl+Shift+R** |
|全选 |**Ctrl+A** |
|选择大括号¹ |**Ctrl+Shift+]** |
|向下转置行 |**Ctrl+Shift+向下键** |
|移调行 |**Ctrl+Shift+向上键** |
|取消注释所选块 |**Ctrl+Shift+K** |
|撤消 |**Ctrl+Z** |
|展开源函数 |Alt+Shift+0 |

¹ Select to brace 选择以大括号为边界的块。在使用此选项之前，光标必须紧挨着块的开始或结束大括号。

## 维护单独的搜索结果

除了通常的搜索功能外，**Find** 功能还可以分别显示四种不同搜索的结果。

1. 点击**Find** 图标 ![Find Results Icon](/images/user-guide/scripting/lua/lua-editor-debugger-find-results-icon.png) 或 按下**Ctrl+F**在当前打开的文件或所有打开的文件中执行搜索。

    ![Lua Editor Find dialog](/images/user-guide/scripting/lua/lua-editor-debugger-find-dialogue.png)

1. 在开始搜索之前，请选择 **Find 1**, **Find 2**, **Find 3**, 或 **Find 4** 选择要在其中查看结果的窗口。您可以在选项卡式窗口中分别维护四个搜索的结果。其他窗口中的搜索结果保持不变。

    ![Find Results](/images/user-guide/scripting/lua/lua-editor-debugger-find-results-window.png)

1. 要直接转到代码中找到搜索结果的行，请双击搜索结果中的该行。

    {{< tip >}}
为方便起见，您还可以停靠或浮动 **Find Results** 窗口。
    {{< /tip >}}

## Perforce 集成

Lua Editor 包括 Perforce 集成功能。当您从 Perforce 环境打开文件时，Lua Editor 会在文本编辑窗口的右上角显示文件的状态。

![Not Checked Out](/images/user-guide/scripting/lua/lua-editor-debugger-p4-not-checked-out.png)

![Checked Out By You](/images/user-guide/scripting/lua/lua-editor-debugger-p4-checked-out-by-you.png)

**Source Control** 菜单提供了 **Check Out/Check In** 功能。

![Source Control Menu](/images/user-guide/scripting/lua/lua-editor-debugger-check-out-icon.png)
