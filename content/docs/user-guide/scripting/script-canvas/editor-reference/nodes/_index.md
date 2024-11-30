---
linktitle: 节点
title: Script Canvas 节点
description: 了解如何在 Open 3D Engine （O3DE） Script Canvas 编辑器中放置、连接和组织节点。
weight: 200
---

Script Canvas 中的节点由标题栏、输入和输出组成。

![Anatomy of a node in Script Canvas.](/images/user-guide/scripting/script-canvas/nodes-anatomy.png)

**标题栏** - 节点的标题栏位于节点顶部的彩色带中。标题栏可以包含副标题，但并非所有节点都有字幕。

**输入** - 位于节点的左侧。有两种类型的输入节点：_执行_ 和 _数据_。执行输入将脚本的执行流驱动到给定节点中。数据输入为节点提供进行处理或决策所需的数据。

**输出** - 位于节点的右侧。执行和数据输出节点将执行流和数据驱动到任何连接的节点中。

## 输入、输出和连接类型

**Open 3D Engine （O3DE）** 有两种主要的引脚和连接类型。一些 inputs 和 output 决定了 logic 的流程和执行顺序。其他输入和输出将数据从一个节点传递到下一个节点。

**逻辑输入、输出和连接**

脚本的执行由每个节点上的三角形输入和输出驱动。这些连接决定了执行顺序。当附加到脚本的实体被激活时，Script Canvas 脚本将运行。节点从其左侧的输入连接。完成运行后，它们会激活连接到右侧输出的节点。

具有多个 connections 的 output logic pin 按顺序运行一个 logic branch。执行顺序由建立连接的顺序（从最早到最近）确定。如果需要特定的执行顺序，您可以使用单个 logic flow 或 **Sequencer** 节点指定序列顺序。

每次 logic flow 触发 node 时，具有多个连接的 incoming logic pin 都会运行。例如，如果一个节点由脚本中的三个不同节点触发，则该节点将运行三次。

**数据输入、输出和连接**

数据连接使脚本能够在节点之间读取和写入数据。从一个节点的右侧读取数据，然后在另一个节点的左侧设置数据。

## 建立连接

您只能在相同类型的引脚之间建立连接。例如， 您仅在 logic pins之间建立logic connections，并且仅在相同类型的data pins之间建立数据连接。您不能在不兼容的引脚之间创建连接，例如 logic 和 data。

**建立连接**

1. 在 **Script Canvas Editor** 画布中，从一个节点的输入引脚拖动到另一个节点的输出引脚。这将在两个引脚之间创建一条连接线。

1. 要将连接从一个引脚移动到另一个引脚，请将线的末端从一个引脚拖放到另一个引脚上。

    要删除连接，请右键单击并选择 **Delete**。您也可以按住 Alt 并选择连接以将其删除。

## 变量节点

变量节点使 Script Canvas 能够读取或写入特定变量。

![Variable node in Script Canvas.](/images/user-guide/scripting/script-canvas/nodes-variable-1.png)

![Variable node in Script Canvas.](/images/user-guide/scripting/script-canvas/nodes-variable-2.png)

另一种读取或写入变量的方法（无需单独的变量节点）是在节点数据引脚上使用 [变量引用](/docs/user-guide/scripting/script-canvas/editor-reference/variables/variable-references)。

有关在 Script Canvas 中使用变量的更多信息，请参阅 [Script Canvas 变量和变量管理器]](/docs/user-guide/scripting/script-canvas/editor-reference/variables/).

## 事件节点

在 O3DE 的 [事件总线 （EBus） 系统](/docs/user-guide/programming/messaging/ebus/) 中，可以发送或接收事件。Script Canvas 通过使用发送方节点和接收方节点来显示此系统。

### 发送方节点

事件发送方将事件直接发送到特定实体，或将事件广播到正在侦听并有兴趣处理事件的所有实体。大多数事件都是可寻址的，这意味着它们可以发送到特定实体。由于事件通常发送到实体，因此最常见的地址类型是`Entity Id`，但也可以使用其他地址类型。

以下示例使用 Light 事件创建发送方节点。

**创建发送方节点**

1. 在 **Node Palette** 搜索框中，键入 **Light**。结果显示与光相关的节点。
![Light-related nodes in the Script Canvas Node Palette.](/images/user-guide/scripting/script-canvas/nodes-event-sender-1.png)

    在 **Node Palette** 中，sender events 是深蓝色的条目。所有与 Light 相关的发送方事件都提供了一种与给定 Light 组件通信、配置或更改其行为的方法。您可以将任何与 Light 相关的发送方事件发送到具有 Light 组件的实体。如果拥有 Script Canvas 图形的实体也具有 Light 组件，则它可以将事件发送给自身。

1. 将 **Turn On** 或 **Turn Off** 拖到画布上以创建发送者节点。

    ![Light component Turn On sender node in Script Canvas.](/images/user-guide/scripting/script-canvas/nodes-event-sender-2.png)

    ![Light component Turn Off sender node in Script Canvas.](/images/user-guide/scripting/script-canvas/nodes-event-sender-3.png)

    发送方节点的 **Source** 引脚是指发送事件的实体。默认值为 **Self**，这意味着它会为 Script Canvas 组件所在的同一实体发送 Light 事件。但是，您可以将源更改为游戏世界中的任何实体。

    **State** 引脚是一个布尔值，用于控制光源的状态。

### 接收器节点

事件接收器在接收到特定事件时实现特定行为。

以下示例为 Light 事件创建接收器节点。

**创建接收方节点**

1. 在 **Node Palette** 搜索框中，键入 **Turn**。

    在结果列表中，**Turned Off** 和 **Turned On** 等事件接收器具有浅蓝色图标。

    ![Some event receiver nodes in the Script Canvas Node Palette.](/images/user-guide/scripting/script-canvas/nodes-event-receiver-1.png)

1. 将 **Turned On** 拖到画布上以创建接收器节点。

    ![A Light component Turned On event receiver node.](/images/user-guide/scripting/script-canvas/nodes-event-receiver-2.png)

    接收方节点的 **Source** 引脚是指从中接收事件的实体。默认值为 **Self**，这意味着节点接收 Script Canvas 组件所在的同一实体的光照事件。您可以将 target 更改为游戏世界中的任何实体。

    您还可以使用 [变量引用](/docs/user-guide/scripting/script-canvas/editor-reference/variables/variable-references) 指定目标。每当变量发生更改时，事件总线处理程序都会更新 Source 以匹配变量引用。

1. 点击 **Add/Remove Events**.

    由于接收方节点通常是多个事件的容器，因此您可以选择 **Add/Remove Events** 来查看和添加给定组件的任何可用事件接收器。在本例中，Light 组件公开两个事件： **Turned Off** 和 **Turned On**.

    ![Adding an event to a receiver node in Script Canvas.](/images/user-guide/scripting/script-canvas/nodes-event-receiver-3.png)

1. 选中 **Turned Off** 复选框，将 **Turned Off** 事件添加到接收方节点。

    此时将显示节点中的第二个蓝色条带。该节点现在正在侦听 **Turned On** 和 **Turned Off** 事件。

    ![A receiver node with two events in Script Canvas.](/images/user-guide/scripting/script-canvas/nodes-event-receiver-4.png)

1. 再次单击 **Add/Remove Events**，然后清除 **Closed** 复选框。**Turned Off** 事件将从接收方节点中删除。

#### 显示和使用连接控件

所有接收器节点都有与连接相关的引脚或控件，默认情况下这些引脚或控件是隐藏的。您可以使用这些控件来管理活动何时连接或断开连接。Connected 表示事件已准备好接收事件，Connected 表示事件未接收事件。连接控件还可以在节点成功连接、断开连接或遇到错误时通知您。

以下示例使用 Light 组件 **Turned On** 事件节点。

**启用和使用显示连接控件**

1. 确保 **Node Inspector**可见。在 Script Canvas 编辑器中，选择 **View**、**Node Inspector**，或按 **Ctrl+Shift+I**。

1. 选择 Light **Turned On** 节点以将其选中。

    ![Click to select a node in Script Canvas Editor.](/images/user-guide/scripting/script-canvas/nodes-event-connection-controls-1.png)

1. 在 **Node Inspector**中，选择 **Display Connection Controls**.

    ![Select Display Connection Controls in the Node Inspector.](/images/user-guide/scripting/script-canvas/nodes-event-connection-controls-2.png)

    Light 组件 **Turned On** 接收器节点展开以提供与连接相关的引脚。

    ![Expanded receiver node with connection controls.](/images/user-guide/scripting/script-canvas/nodes-event-connection-controls-3.png)

    * **Connect** 和 **Disconnect** - 使用 **Disconnect** 引脚来防止接收器节点连接。当事件应已连接并可用于接收事件时，请使用 **Connect** 引脚。

        **Connect** 和 **Disconnect** 引脚 在使用 **On Tick** 事件时特别有用。例如，如果您有一个复杂的操作，您不希望为游戏的每个 tick 处理该操作，则可以断开 **On Tick 事件** 的连接，直到需要为止。

        {{< note >}}
启用接收方节点的 **Display Connection Controls** 属性时，该节点不再自动连接。在这种情况下，Script Canvas 假定您要指定连接发生的时间。
        {{< /note >}}

    * **OnConnected** - 当事件连接成功时触发。如果要在连接发生时沿连接路径继续执行，则此 pin 非常有用。
    * **OnDisconnected** - 当事件成功断开连接时触发。如果要在连接发生时沿断开连接路径继续执行，则此引脚非常有用。
    * **OnFailure** - 如果事件需要源但未提供源，或者提供的源无效，则事件无法连接。**OnFailure** 引脚显示诊断信息，您可以使用这些信息来验证地址数据是否已正确指定到接收器节点的 **Source** 引脚。

**示例**

在以下示例中，为 **On Tick** 事件接收器节点启用了 **Display Connection Controls**。**Tick** 事件在图表生命周期开始时断开连接。当灯打开时，该示例会为每个刻度随机更改灯的颜色。

![Controlling the On Tick event by using connection controls.](/images/user-guide/scripting/script-canvas/nodes-event-connection-controls-4.png)
