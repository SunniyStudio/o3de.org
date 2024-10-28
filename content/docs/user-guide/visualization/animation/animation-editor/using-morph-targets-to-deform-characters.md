---
description: ' 在Open 3D Engine动画编辑器中使用变形目标对角色进行变形
  . '
title: 使用变形目标使角色变形
---

变形目标是以一系列顶点位置存储的变形网格。变形目标也称为混合形状或顶点级变形。您可以使用变形目标对角色的面部进行变形，以制作面部表情动画，也可以使用变形目标对角色的身体部位进行变形，以修正不想要的皮肤变形。您还可以模拟角色身上衣服的变形。

在**动画编辑器**中，您可以将变形目标与下列节点之一结合使用：
+ **Motion**节点--播放变形目标动画与骨骼动画类似。
+ **Motion Target** 节点 - 播放变形目标动画，与骨骼动画类似。

## 先决条件 

要在**动画编辑器**中使用变形目标，您必须执行以下操作：
+ 为导出 `.fbx` 准备资产。更多信息，请参阅 [使用 FBX 设置自定义 FBX 资产导出](/docs/user-guide/assets/scene-settings/)。
+ 在 DCC 工具（例如 Maya）中为角色创建变形目标并制作动画。
+ 将角色导出为 `.fbx` 文件。

**主题**
+ [先决条件](#animation-editor-morph-targets-prerequisites)
+ [导入 Morph Targets](#animation-editor-importing-morph-targets)
+ [打开 Actor 文件](#animation-editor-opening-actor-files)
+ [在Actor上预览Morph Target](#animation-editor-previewing-morph-targets-on-actors)
+ [预览带Morph Target的Motion节点](#animation-editor-creating-motion-nodes-with-morph-targets)
+ [创建 Morph Target 节点](#animation-editor-creating-morph-target-nodes)

## 导入 Morph Targets 

当您将`.fbx`文件导入 O3DE 时，该文件中的所有变形目标和变形目标动作都将作为Actor的一部分导入。这样您就可以在**动画编辑器**中打开Actor文件，而无需额外的步骤。

您还可以改变形态目标的导入方式。

**更改变形目标的导入方式**

1. 在**Asset Browser**中，右键单击您的`.fbx`文件并选择**Edit Settings**。

![Choose the Edit Settings option for your .fbx file in the Asset Browser.](/images/user-guide/actor-animation/asset-browser-fbx-file-edit-settings.png)

1. 在**FBX 设置**窗口中，单击 “**Actors**”选项卡。

   此时会出现一个修饰符，表示将导入变形目标。

1. 单击**Select morph targets**字段旁边的按钮。

![Click the Select morph targets button on the Actors tab in the FBX Settings window.](/images/user-guide/actor-animation/fbx-settings-select-morph-targets-button.png)

1. 在**Select nodes**窗口中，选择要导入的变形目标，然后点击**Select**。 

![Select the morph targets to import in the Select nodes window.](/images/user-guide/actor-animation/morph-targets-select-nodes-window.png)

1. 在**FBX Settings**窗口中，单击**Update**保存更改。

**更改目标动作的导入方式**

1. 在**资产浏览器**中，右键单击您的`.fbx`文件并选择**Edit Settings**。

1. 在**FBX Settings**窗口中，单击**Motions**选项卡。

    {{< note >}}
   如果您的 `.fbx`文件中有变形目标动画，则会出现一个修饰符，表示将导入变形目标动作。如果您不希望在`.motion`文件中包含变形目标动画，可以移除该修饰符。
{{< /note >}}

![Motions tab in the FBX Settings window.](/images/user-guide/actor-animation/fbx-settings-motions-tab.png)

1. 点击 **Update** 保存更改。

## 打开 Actor 文件 

在**动画编辑器**中打开Actor文件时，默认情况下会导入所有变形目标和变形目标动作。要改变形态目标的导入方式，请参阅[导入变形目标](#animation-editor-importing-morph-targets)。

**打开Actor文件**

1. 在 O3DE 编辑器中，选择**Tools**，**Animation Editor**。

1. 在**动画编辑器**中，选择**File**，**Open Actor**。

1. 在**Pick EMotion FX Actor**窗口中，选择一个要导入的Actor，然后单击**OK**。

![Choose an actor to import in the Pick EMotion FX Actor window.](/images/user-guide/actor-animation/animation-editor-pick-emotionfx-actor-dialog-box.png)

## 预览Actor的变形目标

您可以预览Actor身上的变形目标。

**预览变形目标**

1. 在 O3DE 编辑器中，选择**Tools**，**Animation Editor**。

1. 在**动画编辑器**中，选择**View**，**Morph Targets**。

1. 在**Morph Targets**窗口中，通过以下操作预览Actor身上的变形目标形状：

  1. 选择**Select All**复选框。

     如果启用，变形目标滑块将覆盖Actor身上的变形目标动作。

  1. 移动变形目标名称旁边的滑块，查看Actor的网格变形。

   ![Preview morph target shapes on your actor in the Morph Targets window.](/images/user-guide/actor-animation/animation-editor-morph-targets-window-example-2.gif)

   1. 点击变形目标旁边的**Edit**，根据需要调整滑块的范围。默认范围为`0` 到 `1`。

   ![Adjust the minimum and maximum value range for the morph target.](/images/user-guide/actor-animation/edit-morph-target-window.png)

1. 预览完变形目标后，清除**Select All**复选框并关闭**Morph Targets**窗口。

## 使用变形目标创建动作节点

使用变形目标创建动作节点与其他生成动作节点的方法类似。

**使用变形目标创建动作节点**

1. 在**动画编辑器**中，在**Motion Sets**选项卡的**Motion Set Management**下，执行以下操作之一：
  + 单击 **+** 图标创建动作集。
  + 单击文件夹图标，打开**Pick EMotion FX Motion Set** 窗口，选择要导入的动作集。单击**OK**。

   ![Choose a motion set to import in the Pick EMotion FX Motion Set window.](/images/user-guide/actor-animation/animation-editor-pick-emotionfx-motion-set-dialog-box.png)

1. 在**Anim Graphs**窗格中，单击**+**图标创建动画图表。

![Create an animation graph in the Anim Graphs pane.](/images/user-guide/actor-animation/anim-graphs-resource-management-new-animation-graph.png)

1. 将**Anim Graph Palette**的**Sources**选项卡中的**Motion**节点拖动到动画图形中。

![Drag the Motion node to the animation graph.](/images/user-guide/actor-animation/anim-graph-motion-node-example.gif)

1. 在动画图形中，选择已添加的 **Motion** 节点。

![Anim Graph tab in the middle pane of the Animation Editor.](/images/user-guide/actor-animation/anim-graph-motion-node-selected.png)

1. 在**Attributes**窗格中，单击**Select motions**。

![Select motions button in the Attributes pane of the Animation Editor.](/images/user-guide/actor-animation/attributes-pane-select-motions-button.png)

1. 在**Motion Selection**窗口中，选择要导入的带有变形目标的动作，然后点击**OK**。

## 创建变形目标节点

除了创建带有变形目标的动作节点外，您还可以使用**Morph Target**节点来直接对动画图形中的变形目标进行动画操作。

**创建变形目标节点***

1. 在**动画编辑器**中，将**Blend Tree**节点从**Anim Graph Palette**的**Sources**选项卡拖到动画图中。

1. 在动画图中，双击**Blend Tree**节点。您应该会在**Anim Graph Palette**中看到一个**Final Node**节点和其他节点。

![Blend Tree node and additional nodes in the Anim Graph Palette.](/images/user-guide/actor-animation/anim-graph-blendtree-node-finalnode.png)

1. 从**Anim Graph Palette**的**Blending**选项卡中拖动**Morph Target**节点到动画图形中。

1. 执行以下操作添加一个绑定姿势作为输入姿势：

  1. 从**Anim Graph Palette**的**Sources**选项卡中拖动**Bind Pose**节点到动画图。

  1. 将**Bind Pose**节点的**Output Pose**连接到**Morph Target**节点的**Input Pose**。

1. 执行以下操作以添加参数来控制变形目标的权重：

  1. 将**Anim Graph Palette**中**Sources**选项卡上的**Parameters**节点拖动到动画图上。

  1. 在**Parameters**窗格中，单击 **+** 图标创建参数。

   ![Parameters pane in the Animation Editor.](/images/user-guide/actor-animation/anim-graph-parameters-pane.png)

   1. 在 **Create Parameter** 窗口中，完成以下事项：

      1. 对于 **Value Type**，选择 **FloatSlider**。

      1. 对于 **Minimum** 和 **Maximum**，使用默认值。

      1. 点击 **Create**。

      ![Create Parameter window in the Animation Editor.](/images/user-guide/actor-animation/anim-graph-create-parameter-dialog-box.png)
   
   1. 在动画图形中，执行以下操作：

      1. 将**Parameter**节点的**Parameter**与**Morph Target**节点的**Morph Weight**相连。

      1. 将**Morph Target**节点的**Output Pose**连接到**Final Node**节点的**Input Pose**。

      ![Example animation graph that shows the connection between the Parameter node, Morph Target node, and Final Node node.](/images/user-guide/actor-animation/anim-graph-parameters-morph-target-connection.png)

1. 在动画图表中，选择 **Morph Target** 节点，如果还没有被选择。

1. 在**Attributes**面板中，点击**select morph targets**。

![Click the select morph targets button in the Attributes pane.](/images/user-guide/actor-animation/attributes-pane-select-morph-targets-button.png)

1. 在**Morph target selection** 窗口，选择要导入的变形目标，然后点击 **OK**。

![Morph target selection window to import a morph target.](/images/user-guide/actor-animation/morph-target-selection-window.png)

  变形目标会在动画图中的**Morph Target**节点和**Attributes**窗格中更新。

   ![Morph target updated in the Morph Target node and Attributes pane.](/images/user-guide/actor-animation/animation-graph-attributes-pane-morph-target-set.png)

1. 在 **Anim Graphs** 窗格，双击名称激活动画图形。

1. 在 **Parameters** 窗格，移动滑块播放动画。

![Move the parameter slider to play the morph target animation.](/images/user-guide/actor-animation/morph-target-animation-parameter-slider.gif)
