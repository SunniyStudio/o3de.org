---
description: ' 请参阅 Open 3D Engine 中的以下控制器节点。 '
title: 控制器节点
---

请参阅**动画编辑器**中的以下控制器节点。

**主题**
+ [Get Transform](#animation-editor-get-transform-node)
+ [Set Transform](#animation-editor-set-transform-node)

## Get Transform 

**Get Transform** 节点从你指定的关节中获取变换数据。变换包括平移（位置）、旋转和缩放。您可以使用此节点返回动画的变换。

**要创建Get Transform节点**

1. 在 O3DE 编辑器中，选择 **Tools**, **Animation Editor**。

1. 在**Animation Editor**中，在**Anim Graph**标签页中，打开现有的动画图表或点击 **+** 图标创建一个。

1. 右击图表，选择 **Create Node**, **Sources**, **Blend Tree**。

1. 双击 **Blend Tree** 节点，右击图表，然后选择 **Create Node**, **Controllers**, **Get Transform**。

![Add the Get Transform node to your anim graph.](/images/user-guide/actor-animation/animation-editor-get-set-transform-1.png)

   **Get Transform** 节点出现在你的图表中。

   ![Get Transform node](/images/user-guide/actor-animation/animation-editor-get-transform.png)

1. 要从特定动画中获取变换，请右键单击图形并选择 **Create Node**, **Sources**, **Motion**。

1. 连接**Motion**节点的**Output Pose** 到**Get Transform**的**Input Pose**。

![Connect the Motion node to the Get Transform node in your anim graph.](/images/user-guide/actor-animation/animation-editor-get-set-transform-2.png)

1. 从 **Get Transform** 节点，在 **Attributes** 标签页中，点击 **Select node**。 这将选择要从中获取变换的关节。

![Set the joint for the Get Transform node.](/images/user-guide/actor-animation/animation-editor-get-set-transform-6.png)

1. 在 **Node Selection Window**中，选择要使用的关节，并点击 **OK**。

![Select a joint for the Get Transform node.](/images/user-guide/actor-animation/animation-editor-get-set-transform-4.png)

   **Get Transform** 节点将输出**Output Translation** (position), **Output Rotation**, 和 **Output Scale** 向量 (x, y, z)

1. 在右侧面板中，在**Attributes**标签页上，指定 **Transform Space**。 您可以指定以下内容。
****


## Set Transform 

**Set Transform** 节点设置所选关节的变换，包括平移（位置）、旋转和缩放。

**要创建Set Transform节点**

1. 在O3DE编辑器中，选择**Tools**, **Animation Editor**。

1. 在**Animation Editor**中，在**Anim Graph**标签页中，打开现有的动画图表或点击 **+** 图标创建一个。

1. 右击图表，选择 **Create Node**, **Sources**, **Blend Tree**。

1. 双击 **Blend Tree** 节点，右击图表，然后选择 **Create Node**, **Controllers**, **Set Transform**。

![Add the Set Transform node to your anim graph.](/images/user-guide/actor-animation/animation-editor-get-set-transform-5.png)

   **Set Transform** 节点使用 **Input Pose**, **Translation** (position), **Rotation**, 和 **Scale** 作为输入，然后为选中的节点（关节）输出 **Output Pose**。

1. 对于**Set Transform**节点，在**Attributes**标签页上，点击**Select joint**。 指定要设置变换的关节。

![Set the Select joint attribute for the Set Transform node.](/images/user-guide/actor-animation/animation-editor-get-set-transform-3.png)

1. 选择您喜欢的关节，然后点击**OK**。**Set Transform**节点将为**utput Pose**输出向量(x, y, z)。

1. 在右窗格的**Attributes**选项卡上，指定**Transform space**。您可以指定以下内容。
****

