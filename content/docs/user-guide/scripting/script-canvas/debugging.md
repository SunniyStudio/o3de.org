---
linktitle: 调试
title: Script Canvas 调试
description: 在 Open 3D Engine （O3DE） 中调试 Script Canvas 图形。
weight: 600
---

Script Canvas 支持对游戏中运行的 Script Canvas 图形进行实时调试。您可以使用 **O3DE 编辑器** 或非编辑器工具 （如游戏启动器） 作为调试目标。

打开 Script Canvas 调试器并选择目标

1. 在 **Script Canvas Editor** 中，选择 **Tools**, **Debugging**. 调试器面板将在 Script Canvas Editor 的底部打开。

   ![Choose Tools, Debugging.](/images/user-guide/scripting/script-canvas/script-canvas-debugging-1.png)

1. 在调试面板的 **Live** 选项卡上，使用下拉菜单选择调试目标。默认目标是 O3DE Editor，但您可以使用 Script Canvas 调试在独立模式下运行的游戏。有关详细信息，请参阅 [游戏内调试](#in-game-debugging).

   ![Choosing the debug target.](/images/user-guide/scripting/script-canvas/script-canvas-debugging-2.png)

## 选择要调试的实体和图形

选择调试目标后，您可以选择要调试的实体和图形。

选择要调试的实体和图形

1. 要查看具有可用于调试的 Script Canvas 图形的实体，请展开 **Entities** 选项卡上的项目。**Entities**选项卡显示调试程序在编辑时已知的具有 Script Canvas 图形的实体。

   ![Entities with Script Canvas graphs.](/images/user-guide/scripting/script-canvas/script-canvas-debugging-3.png)

   {{< note >}}
不支持在单个实体上多次运行同一图表。
   {{< /note >}}

1. 要获取项目中所有可用 Script Canvas 图形的完整列表，请选择 **Graphs** 选项卡。表中的每个图表都显示正在使用该图表的所有实体。**Graphs** 选项卡可用于调试动态生成的脚本。有关详细信息，请参阅本主题后面的 [调试动态生成的图形](#debugging-a-dynamically-spawned-graph)。

   ![Graphs that are attached to entities.](/images/user-guide/scripting/script-canvas/script-canvas-debugging-4.png)

1. 在 **Entities** 选项卡或 **Graphs** 选项卡上，选中要调试的实体或图形的复选框。

   ![Selecting entities to debug.](/images/user-guide/scripting/script-canvas/script-canvas-debugging-5.png)

1. 要捕获所选图形的所有实例，请选择 **All Graph Instances**.

   ![Selecting all instances of a graph.](/images/user-guide/scripting/script-canvas/script-canvas-debugging-6.png)

## 配置调试器选项

使用以下选项配置调试器行为。

**Auto Capture**

![The Auto Capture option enabled.](/images/user-guide/scripting/script-canvas/script-canvas-debugging-7.png)

如果您希望在调试器连接到指定目标后立即捕获该目标的输出，请启用此选项。对于外部工具，启用此选项后，捕获将立即开始。对于 O3DE Editor，当编辑器进入游戏模式时开始捕获。

**Live Updates**

![The Live Updates option enabled.](/images/user-guide/scripting/script-canvas/script-canvas-debugging-8.png)

启用此选项可在捕获数据时显示数据。禁用后，将以静默方式捕获数据，并且仅在捕获完成后显示。

{{< note >}}
启用实时更新并捕获大量数据时，编辑器性能会明显下降。为了获得更好的性能，您应该禁用实时更新，尤其是对于较长的捕获。
{{< /note >}}

## 运行调试器

选择要调试的实体或图形后，您就可以运行 Script Canvas 调试器了。

**运行 Script Canvas 调试器**

1. 点击 **Capture**. **Capture** 按钮自动将 O3DE 置于游戏模式。

   ![The Script Canvas debugger Capture button.](/images/user-guide/scripting/script-canvas/script-canvas-debugging-9.png)

   {{< note >}}
如果选择 **Editor** 作为捕获目标，则游戏必须正在运行，调试器才能返回结果。
   {{< /note >}}

   Script Canvas 调试器在图形运行时开始捕获数据。如果启用了实时更新，则当正在调试的图形在游戏期间变为活动状态时，数据将显示在 debugger 面板中。否则，数据将在捕获完成后显示。

   ![Data being captured in the Script Canvas debugger.](/images/user-guide/scripting/script-canvas/script-canvas-debugging-10.png)

1. 获得足够的数据后，再次选择 **Capture** 以停止数据捕获。

## 检查捕获的数据

捕获的数据显示在日志中，该日志按处理顺序排序。每行表示单个节点的处理。

![Captured data in the Script Canvas debugger.](/images/user-guide/scripting/script-canvas/script-canvas-debugging-11.png)

{{< note >}}
目前，只能存储捕获数据的单个实例。捕获一组新数据时，以前的数据将丢失。
{{< /note >}}

**检查捕获的数据**

1. 要查看与日志中的某一行对应的 Script Canvas 节点，请选择该行。

   每行通常显示节点的 **In** 信号和 **Out** 信号。如果 **In** 或 **Out** 信号不存在，则该节点是给定逻辑线的第一个或最后一个节点。如下图所示，**Set Location Rotation** 节点是最后一个节点，因此不存在 **Out** 信号。

   ![Choosing a debugger line to show the corresponding node on a Script Canvas graph.](/images/user-guide/scripting/script-canvas/script-canvas-debugging-12.png)

1. 使用向上或向下箭头键在 Debugger 面板中的日志消息之间移动。执行此操作时，将突出显示 Script Canvas 图形中的相应节点。

1. 要检查节点正在使用的数据，请展开日志消息。

   ![Expanding a log message to reveal its data.](/images/user-guide/scripting/script-canvas/script-canvas-debugging-13.png)

   {{< note >}}
某些节点发送以 annotation 形式显示的附加信息。例如，**Print** 节点发送它显示的完整字符串。
   {{< /note >}}

1. 要展开所有行，请选择 ![Expand log messages.](/images/user-guide/scripting/script-canvas/script-canvas-debugging-14.png) 展开图标。

1. 要折叠所有行，请选择折叠 ![Collapse log messages.](/images/user-guide/scripting/script-canvas/script-canvas-debugging-15.png) 图标。

1. 要搜索一个或多个特定节点名称，请使用 **Search** 框。

   ![Using search to find specific nodes in the Script Canvas debugger.](/images/user-guide/scripting/script-canvas/script-canvas-debugging-16.png)

## 调试动态生成的图形

动态生成的图形通常是可生成对象的一部分。由于在编辑时无法知道动态生成的图形，因此您必须按名称选择它们，而不是按使用它们的一个或多个实体来选择它们。

调试动态生成的图形

1. 在 **Graphs** 选项卡上，选择图表的名称。

1. 按照用于调试任何其他 Script Canvas 图形的相同步骤进行操作。调试器会在图形在游戏过程中变为活动状态时记录图形的操作。

## 游戏内调试

对于游戏内调试，您可以使用 Script Canvas 调试器连接到正在运行的游戏启动器。

**调试正在运行的游戏**

1. 运行游戏的启动器。

1. 在 Script Canvas 调试器的 **Live** 选项卡上，从调试目标列表中选择启动器。当您选择启动器作为调试目标时，将记录您指定的图表的 Script Canvas 执行情况。

   ![Choosing a launcher debug target.](/images/user-guide/scripting/script-canvas/script-canvas-debugging-2.png)

## 调试注意事项

使用 Script Canvas 调试器时，请记住以下几点。

### 性能

编辑器在捕获数据时性能严重下降。要缓解这种情况，请禁用实时更新并避免捕获冗长的会话。长时间的会话很容易导致日志记录事件急剧增加。如果您的游戏以 60fps 的速度运行，并且您有 40 个 Script Canvas 节点在每个时钟周期上运行，则每秒必须显示 2400 条日志消息。一分钟后，此数字将增加到 144000 条消息。要最大程度地减少捕获的数据量，请限制日志记录的范围和强度。

### 保存 issue

当您修改并保存图形时，某些 ID 会在资产中重新映射，但在可视化的 Script Canvas 场景中不会重新映射。因此，日志记录消息中使用的统一 ID 不再与视觉表示匹配。这种不匹配会导致视觉抓取失败。要解决此问题，请关闭并重新打开 Script Canvas 场景。
