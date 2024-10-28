---
description: ' 使用Event节点触发并向 Open 3D Engine 中的 Script Canvas 发送值。 '
title: Event 节点
---

您可以在序列中添加**Event**节点，以触发并向 Script Canvas 发送值。您可以使用**Track View Events**窗口创建**Track Events**。然后将**Track Event**分配给一个动画键，该动画键已添加到**Event**节点的音轨中。当按键在序列中播放时，事件就会被触发。然后，Script Canvas 将使用***Track Event**来触发其他脚本逻辑。

**添加Event节点**

1. 在 Track View 中，选择或创建一个序列。

1. 在节点浏览器中，右键单击并选择**Add Event Node**。

1. 在**Track Event Name**窗口中，输入轨道名称，然后选择**OK**。

   这样就创建了一个**Track Event**节点，并自动为该节点添加了一个**Track Event**音轨。

   ![Creating track event nodes in a sequence.](/images/user-guide/cinematics/cinematics-track-view-editor-track-event-nodes.png)

**创建Track Event**

1.  在节点浏览器中，右键单击并选择**Edit Events**。这将打开**Track View Events**窗口。 

    ![Creating a track event for a sequence.](/images/user-guide/cinematics/cinematics-track-view-editor-track-event-nodes-2.png)

1. 在 **Track View Events** 窗口中，点击 **Add** 创建一个事件。

1. 输入事件名称并选择 **OK**。您的轨道事件就会出现在窗口中。

    ![Enter a name for a track event in your sequence.](/images/user-guide/cinematics/cinematics-track-view-editor-track-event-nodes-3.png)

1. 完成后，关闭窗口。现在，您可以在 Script Canvas 中指定此轨道事件。
