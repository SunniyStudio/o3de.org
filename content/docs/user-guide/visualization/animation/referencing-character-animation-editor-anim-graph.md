---
description: ' 引用外部Anim Graph，简化 Open 3D Engine 中基于节点的动画制作。 '
title: 引用外部Anim Graph
---

一个基于节点的动画系统如果有数千个节点，就很难管理。O3DE 的**EMotion FX 动画编辑器**使用分层节点，有助于缓解这一问题，但对游戏逻辑进行通用级别的更改仍然具有挑战性。

**动画编辑器***引用节点通过提供外部动画图（anim graph）文件的引用来解决这个问题。这有助于降低动画图形的规模和复杂性，并最大限度地减少人为错误。引用节点作为其引用的动画图形的根状态机，始终输出一个姿势。

## 优点

引用动画图形有以下好处：
+ **共享动画图或片段**- 您可以创建可在多个其他动画图形中共享的动画图形片段或片断。例如，你可以创建一个动作动画图形片段，在所有角色中共享，同时对其余部分进行个性化处理。使用引用功能后，每次使用时都无需复制和粘贴相同的动画图形。
+ **易于维护** - 您可以在一个地方维护共享的动画图形。如果复制并粘贴动画图形，则必须单独维护每个副本。

    {{< note >}}
  由于引用动画图的更改可能会破坏另一个动画图的行为，因此跟踪引用层次结构非常重要。如需了解更多信息，请参阅 [使用引用动画图的最佳实践](#character-animation-editor-anim-graph-reference-best-practices)。
{{< /note >}}

+ **更易于协作** - 通过清晰地分离动画图形，多人可以同时为不同的角色开发动画。

## 使用外部动画图形

本节将向您展示如何在**动画编辑器**中创建引用节点、为其分配外部动画图形以及查看和管理引用图形。

**创建外部动画图形的引用**

1. 在**动画编辑器**中，执行以下操作之一：

    + **右击** **Anim Graph** 网格，选择 **Create Node**, **Sources**, **Reference**。

        ![Create a reference node from the Anim Graph grid.](/images/user-guide/actor-animation/char-animation-editor-anim-graph-ref-1.png)

    + 点击**Anim Graph Palette**标签页。从**Sources**，拖拽**Reference**节点到网格。

        ![Drag a reference node from the Anim Graph Palette to the Anim Graph grid.](/images/user-guide/actor-animation/char-animation-editor-anim-graph-ref-2.png)

        新的Reference节点会以紫色显示在**Anim Graph**网格中。

        ![A new reference node in the Anim Graph grid.](/images/user-guide/actor-animation/char-animation-editor-anim-graph-ref-3.png)

1. 选择 **Reference** 节点。节点被选中后，颜色会变为橙色。

    ![Click a reference node to see its attributes.](/images/user-guide/actor-animation/char-animation-editor-anim-graph-ref-4.png)

    **Attributes**标签页的**Reference**部分显示Reference节点的属性，

1. 在 **Attributes** 标签页上，对于**Anim graph**，点击 (**...**) 图标。

    ![Click to browse to an external anim graph for the reference node.](/images/user-guide/actor-animation/char-animation-editor-anim-graph-ref-5.png)

1. 在**Pick EMotion FX Anim Graph**对话框中，选择要分配给Reference节点的`.animgraph`文件，然后点击**OK**。

    ![Choose the anim graph to assign to the reference node.](/images/user-guide/actor-animation/char-animation-editor-anim-graph-ref-6.png)

1. 在**Anim Graph**网格中，**双击** Reference节点来查看引用的动画图形所包含的节点。

    ![Double-click the Reference node.](/images/user-guide/actor-animation/char-animation-editor-anim-graph-ref-7.png)

1. 继续**双击**节点，深入研究下面的节点。在本例中，引用的动画图包含一个**StateMachine**节点，而**StateMachine**节点包含一个**EntryNode**和一个**ExitNode**。 

    ![Double-click the next node.](/images/user-guide/actor-animation/char-animation-editor-anim-graph-ref-8.png)

   以这种方式查看引用动画图时，引用动画图是只读的。

    ![Referenced anim graphs in the Animation Editor are read-only.](/images/user-guide/actor-animation/char-animation-editor-anim-graph-ref-9.png)

1. 网格上方的显示屏会显示您在节点层次结构中的当前位置。要返回到前一个节点，请单击节点名称。

    ![Click a node name to see the node in the Anim Graph grid.](/images/user-guide/actor-animation/char-animation-editor-anim-graph-ref-10.png)

1. 在**Anim Graph**网格的右上方，单击导航页面图标。

    ![Click the Toggle Navigation Pane icon.](/images/user-guide/actor-animation/char-animation-editor-anim-graph-ref-11.png)

   导航窗格在网格右侧打开，显示节点的层次结构。导航窗格显示所有加载的动画图形。当前节点的名称以粗体显示。每个节点的颜色表示其类型。例如，入口节点为绿色，出口节点为红色。

    ![The navigation pane showing the node hierarchy](/images/user-guide/actor-animation/char-animation-editor-anim-graph-ref-12.png)

1. 要编辑外部动画图形，请**右键单击**您为其分配的引用节点，然后选择**Open '***filename***.animgraph' file**。您对外部动画图形所做的更改将反映在所有引用它的动画图形中。

    ![Right-clicking a reference node to edit the anim graph that is assigned to it.](/images/user-guide/actor-animation/char-animation-editor-anim-graph-ref-13.png)

## 使用引用动画图表的最佳实践

请参阅以下使用引用动画图表的最佳实践：
+ **跟踪您的引用的层次结构** - 跟踪引用层次结构的一种方法是维护一个动画图形及其引用的图表。这种图表可以帮助开发人员和测试人员了解哪些动画图形会受到所做更改的影响。图表还能帮助你了解正在处理的动画图是否被另一个动画图引用。
+ **目录层次结构** - 使你的动画图目录和文件层次结构与你的引用层次结构相同。例如，层级较高目录中的动画图使用或引用层级较低目录中的动画图资产，反之亦然。
+ **最小化参数数量** - 尽量减少引用动画图形中的参数数量。使用大量参数会增加复杂性。
+ **有效管理动作集** - 使用引用动画图形时，要管理动作集，可以考虑以下选项：
  + 管理独立的动作集 每个动作集包含一个动画图形的动作。
  + 为领导动画图创建一个大型动作集。该动作集将包含领导动画图和任何引用动画图中使用的所有动作。
  
    {{< note >}}
    这两个选项都允许单独测试引用的动画图形。
{{< /note >}}

### 使用引用动画图表的技巧

在使用引用动画图形时，请避免以下做法：
+ **更改被另一个动画图形引用的动画图形** - 更改被另一个动画图形引用的动画图形可能会破坏其行为。例如，如果从被引用的动画图中移除另一个动画图使用的参数，该参数就会恢复为默认值。这可能会导致意想不到的行为。
+ **重新命名、移动或删除动画图形** - 重命名、移动或删除动画图时，其资产 ID 会发生变化。因此，引用重命名、移动或删除的动画图的所有动画图也必须更新。拥有一个能跟踪引用层次结构的系统（如 [最佳实践](#character-animation-editor-anim-graph-reference-best-practices) 中所述），就能轻松知道哪些动画图受到影响，哪些需要更新。
