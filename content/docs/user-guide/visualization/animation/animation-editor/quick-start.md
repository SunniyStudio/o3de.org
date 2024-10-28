---
description: ' 使用动画编辑器快速启动，在 Open 3D Engine 中为角色制作动画。 '
title: 动画编辑器入门
---

请参阅以下程序，开始使用 ** 动画编辑器**。

在此过程中，您需要执行以下操作：
+ 导入Actor文件并创建动作集，为角色指定所需的动作。
+ 使用节点创建基本动画图。
+ 创建一个混合树来组合动作，并使用滑块控制角色从空闲到行走再到奔跑的运动。

## 第 1 步: 创建一个动作集

在以下步骤中，您将导入角色、为机器人插孔、选择所需的动作，然后将这些动作添加到动作集中。

**创建一个动作集**

1. 在 O3DE 编辑器，选择 **Tools**, **Animation Editor**。

1. 在动画编辑器中，选择 **Layouts**, **AnimGraph**。

1. 在 **Animation Editor** 中，选择 **File**, **Open Actor** 并导航到 `AnimationSamples\Simple_JackLocomotion` 目录。

1. 选择 `JackBind_ZUp.fbx` 文件，然后点击 **OK**。

   您的角色 Jack 将出现在**动画编辑器**中。

   ![Import the JackBind_ZUp.fbx file into the Animation Editor.](/images/user-guide/actor-animation/animationeditorquickstart/animation-editor-quick-start-jack-idle.png)

1. 在 **Motion Sets** 标签页上，在 **Motion Set Management**下，点击 **+** 图标添加动作集。

1. 选择 **MotionSet0** 节点。

1. 在 **Motion Set** 面板中，点击文件夹图标添加动作。

1. 导航到 `AnimationSamples/Simple_JackLocomotion` 目录。并选择以下文件：
   + `Jack_Idle_ZUp.fbx`
   + `Jack_Strafe_Run_Forwards_ZUp.fbx`
   + `Jack_Strafe_Walk_Forwards_ZUp.fbx`

1. 点击 **OK**。

1. 在**Motion Set Management**面板中，点击**Save**图标。

1. 导航到`/SamplesProject/AnimationSamples/Simple_JackLocomotion`目录。对于文件名，输入**quickstart**，然后点击**Save** 保存 `quickstart.motionset` 文件。

![Create a motion set and add motion files in the Animation Editor.](/images/user-guide/actor-animation/animationeditorquickstart/animation-editor-quick-start-motion-set.png)

## 第 2 步: 创建一个Animation Graph

在以下步骤中，创建动画图形和节点。

**创建一个Animation Graph**

1. 在**Anim Graph**标签页上，点击 **+** 图标创建一个Animation Graph。

1. 点击 **Save** 图标。

1. 导航到 `/SamplesProject/AnimationSamples/Simple_JackLocomotion` 目录。对于文件名，输入 **quickstart** ，然后点击 **Save** 保存 `quickstart.animgraph` 文件。

1. 在 **Anim Graph** 标签页中，右击网格，然后选择 **Create Node**, **Sources**, **Motion**。

![Create a motion node in the animation graph.](/images/user-guide/actor-animation/animationeditorquickstart/animation-editor-quick-start-anim-graph-node.png)

1. 选择**Motion0**节点，在**Attributes**窗格中单击**Select motions**。在对话框中选择 “Jack_Idle_ZUp.fbx”，然后选择 “OK”。

1. 右键单击网格，然后选择**Create Node**、**Sources**、**Blend Tree**。

1. 从 **Motion0** 节点，单击并拖动一条线到 **BlendTree0** 节点。一条带有箭头的过渡线将节点连接起来。

1. 从 **BlendTree0** 节点，单击并拖动一条线到 **Motion0** 节点。

![Connect the motion and blend tree nodes in the animation graph.](/images/user-guide/actor-animation/animationeditorquickstart/animation-editor-quick-start-motion-blend-tree-nodes.png)

1. 在**Parameters**面板中，点击 **+** 图标创建一个参数。

   1. 将 **Value type** 参数保留为默认值，**Float (slider)**。

   1. 对于 **Name**，重命名 `Parameter0` 为 **speed**。

   1. 点击 **Create**。

1. 在动画图表中，选择从**Motion0**节点连接到**BlendTree0**节点的过渡线。

   1. 在**Attributes**面板中，点击**Add condition**。

   1. 在**Select a Condition**对话框中，选择**Parameter Condition**，然后点击**Add Condition**。

   1. 在**Attributes**面板中，在 **Parameter Condition**下，点击 **Select parameter** 并选择 **speed**。

   1. 对于 **Test Function**，保留**param > testValue**的默认值。这意味着，如果速度大于零，空闲运动就会过渡到混合树，角色就会开始移动。

   ![Add parameter conditions to specify when the character starts moving.](/images/user-guide/actor-animation/animationeditorquickstart/animation-editor-quick-start-add-condition.png)

1. 在动画图形中，选择从 **BlendTree0** 节点开始并连接到 **Motion0** 节点的过渡线。在动画图形中，选择从 **BlendTree0** 节点开始并连接到 **Motion0** 节点的过渡线。

   1. 在**Attributes**窗格中，单击**Add condition**。

   1. 在**Select a Condition**对话框中，选择**Parameter Condition**，然后单击**Add Condition**。

   1. 在**Attributes**窗格中，在**Parameter Condition**下，单击**Select parameter**。选择**speed**，然后单击**OK**。

   1. 在**Test Function**中，选择**param == testValue**。这意味着，如果速度等于零，运动将转回空闲状态，角色停止运动。

      ![Add parameter conditions to specify when the character stops moving.](/images/user-guide/actor-animation/animationeditorquickstart/animation-editor-quick-start-add-condition-02.png)

## 第 3 步: 混合动画

在以下步骤中，您将使用混合树节点构建混合树，将行走和运行动画混合在一起。

**混合动画**

1. 在动画图形中，双击 **BlendTree0** 节点。

1. 右键单击网格并选择 **Create Node**、**Sources**、**Motion**。

1. 选择 **Motion1** 节点。

   1. 在 **Attributes** 面板，选择 **Select motions**。

   1. 在**Motion Selection Window**中，选择`jack_strafe_walk_forwards_zup`，然后点击 **OK**。

      **Motion1** 节点的属性应如下所示：

      ![Add the walk motion file to the Motion1 node.](/images/user-guide/actor-animation/animationeditorquickstart/animation-editor-quick-start-motion-node-walk.png)

1. 在动画图形中，右键单击网格，选择**Create Node**、**Sources**、**Motion**。

1. 选择 **Motion2** 节点。

   1. 在**Attributes**面板中，点击**Select motions**。

   1. 在对话框中，选择 `jack_strafe_run_forwards_zup` ，然后点击 **OK**。

      **Motion2** 节点的属性应如下所示：

      ![Add the run motion file to the Motion2 node.](/images/user-guide/actor-animation/animationeditorquickstart/animation-editor-quick-start-motion-node-run.png)

1. 右键单击网格，选择 **Create Node**、**Blending**、**Blend Two**。

1. 选择 **BlendTwo0** 节点。

  1. 在**Attributes** 窗格中的**Sync Mode**，选择**Full Clip Based**。

1. 对于 **Motion1** 节点，选择 **Output Pose** 框，然后将连接器拖到 **BlendTwo0** 节点的 **Pose 1** 输入端。

1. 对于**Motion2**节点，选择**Output Pose**框并将连接器拖到**BlendTwo0**节点的**Pose 2**输入端。

1. 对于 **BlendTwo0** 节点，选择 **Output Pose** 框，然后将连接器拖到 **FinalNode0** 节点的 **Input Pose** 上。

   您的混合树应该如下所示：

   ![Create your blend tree in the animation graph.](/images/user-guide/actor-animation/animationeditorquickstart/animation-editor-quick-start-blend-tree-node.png)

1. 右键单击网格，选择 **Create Node**、**Sources**、**Parameters**。

1. 右键单击网格并选择 **Create Node**，**Math**，**Smoothing**。

1. 对于 **Parameters0** 节点，选择 **speed** 输出框，并将连接器拖到 **Smoothing0** 节点的 **Dest** 输入框。

1. 对于 **Smoothing0** 节点，选择 **Result** 输出框，并将连接器拖至 **BlendTwo0** 节点的 **Weight** 输入框。

   您的混合树应该如下所示：

   ![Create a blend tree for your animation graph.](/images/user-guide/actor-animation/animationeditorquickstart/animation-graph-quick-start-blend-tree-final.gif)

1. 在**动画编辑器**中，选择**File**，**Save All**。然后在对话框中点击**OK**。

1. 导航至`/SamplesProject/AnimationSamples/Simple_JackLocomotion`目录。在文件名中输入 **quickstart**，然后单击**Save**保存工作区。

1. 在**Anim Graph**选项卡中，单击**Play**按钮。现在角色应在空闲模式下显示动画。

1. 在**Parameters**窗格中，将**speed**滑动控制向右移动，使杰克行走。再向右移动滑块，使杰克跑起来。

{{< video src="/images/user-guide/actor-animation/animationeditorquickstart/animation-editor-quick-start-jack-running.mp4" info="Animate Jack the robot in the Animation Editor." autoplay="true" loop="true" width="300" >}}
