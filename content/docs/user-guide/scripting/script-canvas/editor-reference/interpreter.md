---
linktitle: 解释器
title: Script Canvas 解释器
description: 了解如何在 Open 3D Engine （O3DE） 中使用嵌入在 Script Canvas Editor 中的 Script Canvas 解释器。
weight: 600
---

Script Canvas 提供了一个解释器接口，几乎可以放置在引擎或编辑器中的任何情况下。您可以从 Script Canvas Editor 中的 **Tools** 菜单打开 Script Canvas 解释器。

![Script Canvas Interpreter Widget](/images/user-guide/scripting/script-canvas/script-canvas-interpreter-widget.png)

Interpreter 获取 Scripts Canvas 源文件，并允许用户立即配置和执行它。如果源文件生成多个执行线程，这些线程在“On Graph Start”之后立即开始和停止，则脚本将继续无限期地执行。用户可以按停止按钮或关闭窗口以停止 Widget 执行。

![Script Canvas Interpreter Widget](/images/user-guide/scripting/script-canvas/script-canvas-interpreter-widget-opened.png)

{{< note >}}
请注意，许多节点以及模拟期间可用于 Entities 的许多功能在执行 Interpreter 时将不适用。当检测到对 'Self' 的引用时，解释器将不会运行图形。
{{< /note >}}
