---
linktitle: 编辑器界面
title: Script Canvas编辑器界面
description: 了解 Script Canvas 编辑器的布局。
weight: 100
---

您可以从 O3DE 编辑器打开 **Script Canvas 编辑器**。

**打开 Script Canvas 编辑器**

1. 在 O3DE 编辑器中，依次选择 **Tools**、**Script Canvas**。

1. 选择 **File**、**New Script** 或从 **Node Palette** 中拖动一个节点并将其拖放到画布上。

![Use the Script Canvas Editor in O3DE to create connections for nodes.](/images/user-guide/scripting/script-canvas/user-interface.png)

在 Script Canvas Editor 中，您可以执行以下操作：

1. 使用菜单栏执行以下操作：
   + 创建、保存和打开您的脚本。
   + 剪切、复制或撤消操作。
   + 更改 Script Canvas Editor （Script Canvas 编辑器） 视图。

1. 使用选项卡在脚本之间切换。

1. 在 **Node Palette** 中，您可以搜索节点。

1. 您可以将节点从 **Node Palette** 拖到画布上，或者 **右键单击** 画布以获取上下文菜单。

1. 在节点上，您可以指定参数的值。

1. 拖动以将一个节点的输出引脚连接到另一个节点的输入引脚。此行在节点之间创建连接。

1. **变量管理器** 显示脚本中使用的变量。您可以添加或删除变量并设置其默认值。要创建 **Get**、**Set** 或 **OnValueChanged** 节点，您可以将变量从 **变量管理器** 拖到脚本上。请参阅 [Script Canvas 变量和变量管理器](/docs/user-guide/scripting/script-canvas/editor-reference/variables/)了解更多信息。

1. 在 **Node Inspector** 中，您可以查看和修改所选节点的属性。

## 其他工具

Script Canvas Editor 具有以下附加菜单和工具。

### 书签

查看和修改已保存的书签。您可以使用书签在脚本上保存位置，然后使用键盘快捷键移动到该位置。有关详细信息，请参阅 [在 Script Canvas 中添加书签](/docs/user-guide/scripting/script-canvas/editor-reference/nodes/organizing/adding-bookmarks).

### 注释

您可以在 Script Canvas Editor 画布上创建浮动文本块。有关详细信息，请参阅 [在 Script Canvas 中添加注释](/docs/user-guide/scripting/script-canvas/editor-reference/nodes/organizing/commenting-nodes).
