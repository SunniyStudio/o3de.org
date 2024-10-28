---
description: ' 在Open 3D Engine中的<guilabel>Track View</guilabel>编辑器中添加Director (Scene)节点. '
title: Director (Scene) 节点
---

**Director (Scene)**节点包含一个摄像机轨迹，用于指定轨迹视图序列的活动摄像机。您可以在**Director**节点下添加特定于序列的节点（例如**Depth of Field**或**Comment**），以覆盖在序列级别设置的相同节点。

**在轨迹视图中添加Director节点**

1. 在 O3DE 编辑器中，选择 **Tools**, **Track View**.

1. 在Track View中，点击 **Add Sequence** ![Add Sequence Icon](/images/user-guide/cinematics/cinematics_add_sequence_icon.png) 图标。

1. 在 **Add New Sequence** 对话框中，为序列输入名称，点击 **OK**。

1. 右击序列并选择**Add Director (Scene) Node**。

1. 右击 **Director** 节点并点击 **Add Track**。

   ![Add the Director node in the Track View to manage your track view sequence.](/images/user-guide/cinematics/cinematics-trackview-nodes-director.png)

1. 选择轨道并双击，将按键定位在时间轴上突出显示的行上。

1. 双击绿色标记，在**Key Properties**下，输入**Value**。

   您可以添加以下轨道，然后将键属性设置到 **Director** 节点。
**Director Node轨道和关键属性**


{{< note >}}
您可以在一个场景中添加多个**Director**节点，但只能有一个**Director**节点处于活动状态。
要更改活动的**Director**节点，请右键单击该节点并选择**Set as Active Director**。停用**Director**节点时，所有子节点动画都会停用。当您想在同一轨迹视图序列中启用或禁用特定对象的动画时，这将非常有用。
{{< /note >}}
