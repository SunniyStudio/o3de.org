---
title: 在 Script Canvas 中使用脚本事件
description: 了解如何在 Open 3D Engine （O3DE） 中将脚本事件与 Script Canvas 结合使用。
weight: 200
---

Script Canvas 会扫描并检测脚本事件资源，因此在定义脚本事件后，您可以在 Script Canvas 中使用它。

## 在节点面板中编写事件脚本

默认情况下，脚本事件资源显示在 **Script Canvas 编辑器** **Node Palette**的 **Script Events** 类别中。

![The Script Events category in the Script Canvas Node Palette.](/images/user-guide/scripting/script-events/using-in-script-canvas-1.png)

{{< note >}}
要更改类别的名称，请在 Asset Editor 中打开脚本事件的资产定义，然后编辑`Category`属性。
{{< /note >}}

## 发送事件

您可以通过向 Script Canvas 图形添加 **Send** *method\_name* 节点来发送事件。

**发送事件**

1. 将要发送的方法拖放到 Script Canvas 图形中。

1. 在上下文菜单中，选择 **Send** *method\_name*。

    ![Choose Send method_name in the context menu.](/images/user-guide/scripting/script-events/using-in-script-canvas-2.png)

    方法发送节点将添加到图形中。

    ![A send node added to a Script Canvas graph.](/images/user-guide/scripting/script-events/using-in-script-canvas-3.png)

1. 将此节点连接到适当的 logic 和 data inputs。当 Script Canvas 图形运行时，它会将事件发送到节点所连接的实体或系统。

## 处理事件

您可以通过向 Script Canvas 图形添加 **Receive** *method\_name* 节点来处理事件。

**处理事件**

1. 将脚本事件方法拖放到画布上。

1. 在上下文菜单中，选择**Receive** *method\_name*.

    ![Choose Receive method_name in the context menu.](/images/user-guide/scripting/script-events/using-in-script-canvas-4.png)

    事件处理程序方法节点将添加到图形中。

    ![A receive node added to a Script Canvas graph.](/images/user-guide/scripting/script-events/using-in-script-canvas-5.png)

1. 将事件处理逻辑连接到节点的 **Out** 引脚。

1. 根据需要连接数据引脚。
