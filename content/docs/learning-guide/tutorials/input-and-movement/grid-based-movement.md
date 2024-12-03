---
linkTitle: 基于网格的移动
title: 基于网格的移动
description: 了解如何在 Open 3D Engine （O3DE） 中从输入设备事件实现基于网格的移动。
weight: 100
---

## 该技术

本教程将介绍如何使用设备输入在网格上移动实体。 本教程的其他部分介绍了 Script Canvas 变量、使用条件检查阻止事件以及使用线性插值创建平滑移动。

| O3DE 体验 |完成时间 |功能聚焦 |最后更新 |
| - | - | - | - |
| 新手 | 20 分钟 | 输入绑定资产, **Input** 组件, **Script Canvas** 组件 | December 9, 2022 |

## 您将学到什么

在本教程中，您将学习如何：
- 在 **Asset Editor** 中创建一个 Input Bindings 资源，将输入设备信号链接到输入事件。
- 通过添加 Input 组件并引用 Input Bindings 资源，在关卡中启用这些输入事件。
- 使用 Script Canvas 创建一个脚本，用于侦听输入事件并在事件发生时沿网格移动实体。
- 使用线性插值添加随时间变化的平滑实体运动。
- 添加在实体移动时阻止输入事件。

## 先决条件

- [Script Canvas 编辑器](/docs/user-guide/scripting/script-canvas/)的基本应用知识。
- 从标准项目模板构建的项目，或包含标准模板中的 Gem 的项目。

## 步骤

### 准备场景

在本教程中，您将修改 Atom 默认环境的多个子实体，即 Shader Ball、Grid 和 Camera。 网格将表示 Shader Ball 在其上移动的网格或基于平铺的地形。 您需要将摄像机附加到 Shader Ball，以便摄像机跟随它在网格上的移动。

1. 在新关卡中，在 **Entity Outliner** 中选择 Grid 实体。 在 **Entity Inspector** 中，将 [Grid](/docs/user-guide/components/reference/atom/grid/) 组件的 **Primary Grid Spacing** 设置为`5 米`。 将 **Secondary Grid Spacing** 设置为 `1 meter`，将 **Secondary Color** 设置为白色，即`255, 255, 255`，以便网格间距更加明显。

1. 在 Entity Outliner 中选择 Shader Ball。 目前，它位于四个网格空间的交汇处，太大，无法容纳在单个空间内。 在 Entity Inspector 中，将 [Transform](/docs/user-guide/components/reference/transform/) 组件的 **Uniform Scale** 设置为 `0.5`。 将 **Translate** 值设置为 `X: 0.5, Y: 0.5, Z: 0.0`; Shader Ball 现在应该适合单个网格空间。 通过将 **Rotate** 设置为 `X: 0.0, Y: 0.0, Z: 0.0` 来删除它的所有旋转。
 
1. 在 Entity Outliner 中选择摄像机，**左键单击**并将其 **拖动** 到 Shader Ball 实体上，然后释放它以将摄像机作为 Shader Ball 的子实体附加。 在 Entity Inspector 中，将 Transform （变换） 组件的 **Translate** 值设置为 `X: 0.0, Y: -4.0, Z: 5.0` ，将 **Rotate** 值设置为 `X: -45.0, Y: 0.0, Z: 0.0`。 作为 Shader Ball 的子实体，摄像机的平移和旋转是相对于其父实体的;摄像机将位于 Shader Ball 的后面并俯视它。当前附加到此实体的 [Fly Camera Input](/docs/user-guide/components/reference/gameplay/fly-camera-input/) 组件将干扰您稍后创建的输入事件。 在 Entity Inspector 中右键单击 Fly Camera Input （飞行摄像机输入） 组件，然后选择 **Delete component （删除组件）**。

下图显示了完成这些步骤后的场景布局。

![The grid, Shader Ball, and camera in the 3D Viewport after setting up the scene](/images/learning-guide/tutorials/input-and-movement/grid-based-movement/prepare-scene.png)

### 创建 Input Bindings 资产

接下来，您将创建一个 Input Bindings 资产，用于将输入设备事件生成器链接到输入事件。 在本教程中，您将使用键盘的`W`, `A`, `S`, 和 `D` 键生成两个输入事件，这两个事件将在网格的 X 轴和 Y 轴上移动 Shader Ball。  

1. 从 O3DE 编辑器的 **Tools** 菜单中打开 Asset Editor。在 Asset Editor 的 **File** 菜单中，选择 **New**，然后选择 **Input Bindings** 以创建新的 Input Bindings 资源。

1. **左键单击**输入事件组旁边的 {{{< icon "add.svg" >}}  两次，以添加两个新的输入事件。

1. 在新事件的 **事件名称** 属性中，将第一个事件命名为 `MoveY`，将第二个事件命名为 `MoveX`。 当您开始编写 Shader Ball 的移动脚本时，您需要记住这些事件名称。

1. **左键单击** **Event Generators**旁边的 {{< icon "add.svg" >}} 向事件添加一个生成器。为每个事件添加两个事件生成器。 在出现的 **Class to create** 对话框中，选择第二个选项 **InputEventMap**。

1. 将四个生成器的 **Input Device Type** 分别设置为 **keyboard**。

1. 对于`MoveY`事件，将事件生成器的 **Input Name** 设置为 **keyboard_key_alphanumeric_W** 和 **keyboard_key_alphanumeric_S**。 对于 `MoveX` 事件，将事件生成器的 **Input Name** 设置为 **keyboard_key_alphanumeric_D** 和 **keyboard_key_alphanumeric_A**。

1. 将 `S` 和 `A` 键的 **Event value multiplier** 更改为 `-1.0`，因为这将对应于 X 轴和 Y 轴负方向的移动。 您可以将 `W` 和 `D`键的 **Event value multiplier** 保留为默认值`1.0`。 这对应于 X 轴和 Y 轴上沿正方向的移动。 使用 **Event value multiplier** 来减少您使用的事件数量可能是有利的，并且可以简化处理事件所需的脚本。

1. 将项目目录中的 Input Bindings 资源另存为`grid-based-movement.inputbindings`。

下图显示了 Asset Editor 中完成的 Input Bindings 资产。

![The completed Input Bindings asset in Asset Editor](/images/learning-guide/tutorials/input-and-movement/grid-based-movement/input-bindings.png)

### 添加 Input 组件

接下来，您将添加 Input 组件并引用您创建的 Input Bindings 资产。

1. 在 Entity Outliner 中选择 Shader Ball。 在 Entity Inspector 中，将 Input 组件添加到 Shader Ball。

1. 在 Input （输入） 组件中，**左键单击**该组件 {{< icon "browse-edit-select-files.svg" >}} 按钮，然后选择您创建的 Input Bindings 资产。

![The Input component with the Input Bindings asset selected](/images/learning-guide/tutorials/input-and-movement/grid-based-movement/input-component.png)

{{< note >}}
您可以将 Input 组件附加到关卡中的任何活动实体;如果引用了 Input Bindings 资产，则关卡中的所有实体都可以接收和处理其事件。
{{< /note >}}

### 使用 Script Canvas 创建脚本

接下来，您将创建一个脚本来处理 `MoveX` 和 `MoveY` 输入事件，并根据收到的事件和事件的值移动 Shader Ball。

1. Select the Shader Ball in Entity Outliner.  In Entity Inspector, add a Script Canvas component to the Shader Ball. On the new Script Canvas component, **left-click** the {{< icon "open-in-internal-app.svg" >}} button to open Script Canvas Editor.

1. Add the **On Graph Start** node to a new graph.  You can search for this node in the **Note Palettte** and **drag** it to the center of the editor to create a new graph.  When you play a level in _Game Mode_, the **On Graph Start** node will execute a single time as soon as the Script Canvas component is active.

1. There must be one **Input Handler** node for each event that you want a graph to handle.  Add two **Input Handler** nodes to the graph.  In the **Event Name** fields, type the names of your events, `MoveX` and `MoveY`.  Connect the **Out** pin of the **On Graph Start** node to the **Connect Event** pin of the two **Input Handler** nodes.  Now the nodes' **Pressed**, **Held**, and **Released** slots will execute whenever the corresponding keyboard keys are pressed. When a key is pressed, you can capture and use the event's value by connecting the **value** output slot to nodes in your graph.

    连接这些节点后，您的 Script canvas 图形应如下所示。

    ![Script Canvas graph with On Graph Start and Input Handler nodes connected](/images/learning-guide/tutorials/input-and-movement/grid-based-movement/sc-input-handlers.png)
    
1. Add two **From Values** nodes to the graph from the **Math > Vector3** category.  You will use these nodes to create a `Vector3` representing a single movement in one of the four grid directions.  Connect the **Pressed** pin of `MoveY`'s **Input Handler** to the **In** pin of one of the **From Values** nodes.  Then, connect the **value** pin to the **Y** slot of that node.

1. Connect the `MoveX` **Input Handler** to the second **From Values** node in the same way, but this time connect the **value** pin to the **X** slot. When you press the `A` or `D` keyboard keys , this node will output either `(-1, 0, 0)` or `(1, 0, 0)`.

1. To get the Shader Ball to move, you need to add a `Vector3` to the ball's current world translation and then set the Shader Ball's translation to the result.  Add two **Get World Translation** nodes to the graph and connect the **Out** pins of the **From Values** node to the **In** pins of the **Get World Translation** nodes.

    {{< note >}}
The **EntityID** fields of these nodes are set to `Self`; this means that when a node executes, it will return the translation of the entity that the Script Canvas graph is on. You simplify all Shader Ball-related references by attaching this graph to the Shader Ball.
{{< /note >}}

1. Next, you need to add the movement and translation vectors together.  Add two **Add (+)** nodes from the **Math** category to the graph, and connect the **Out** pins of the **Get World Translation** nodes to the **In** pins of the **Add (+)** nodes.  For each event, connect a **Get World Translation** **Translation** slot to **Value 0** and the **From Values** **Vector3** slot to **Value 1**.  Now, when the **Add (+)** node executes, its **Result** slot represents the world translation that the Shader Ball should move to.

1. Add two **Set World Translation** nodes to the graph.  Connect the **Out** pin of the **Add (+)** nodes to the **In** pins of the new nodes.  Connect the **Result** pins to the **World Translation** slots.  The **Source** slots of the **Set World Translation** nodes are set to `Self`.

    After connecting these nodes, your Script canvas graph should look like the following.

    ![Script Canvas graph with movement and translation vectors added and the result set to the Shader Ball's world translation](/images/learning-guide/tutorials/input-and-movement/grid-based-movement/sc-set-world-translation.png)

1. Save the Script Canvas graph in your project's directory.  In the Shader Ball's Script Canvas component, **left-click** the {{< icon "browse-edit-select-files.svg" >}} button and choose the Script Canvas graph you created.

1. Save the level.  Then, test that you have set up everything correctly by pressing **Ctrl + G** to enter Game Mode.  You should be able to press **W**, **A**, **S**, and **D** to move the Shader Ball around the grid.

### Simplify the graph with variables

Script Canvas variables simplify the visual complexity of a graph by replacing connections between nodes with references.  Graphs may also require certain variables to execute correctly; the value of an output pin may only be valid for a limited time after a node's execution.  It is best practice to save values as a variable in order to reliably access them later.  In this section, you will improve the Script Canvas graph you created previously in this tutorial by using variables.

1. If your Script Canvas graph is not already open in the Script Canvas editor, **left-click** the {{< icon "open-in-internal-app.svg" >}} button on the Shader Ball's Script Canvas component.

1. In the **Variable Manager** tool of the Script Canvas editor, choose **Create Variable** and select `Vector3` from the dropdown list to create a new `Vector3` variable.  Name the new variable `Start Location`; it will represent the Shader Ball's translation before it moves.

1. Create two more `Vector3` variables named `End Location` and `Move Vector`.

    After adding these variables, your Variable Manager should look like the following.

   ![The Variable Manager tool with three Vector3 variables in it](/images/learning-guide/tutorials/input-and-movement/grid-based-movement/sc-variable-manager.png)

1. Remove all of the green `Vector3` connections in your graph by selecting each connection and pressing **Delete**.

1. If a slot is unconnected, you can make it a variable reference by **double-clicking** on the pin or **right-clicking** on it and selecting **Convert to Reference**. Convert all the `Vector3` slots to references. 

1. Set the reference by dragging the appropriate variable from Variable Manager and dropping it the slot.  **Drag** the `Move Vector` variable to the **Vector3** slot of both **From Values** nodes.

1. Convert the remaining green `Vector3` pins in the graph to use references and add the appropriate variable to them.

    After adding these variable references, your graph should look like the following.

   ![Graph that is using only variable references in Vector3 input and output slots](/images/learning-guide/tutorials/input-and-movement/grid-based-movement/sc-using-variable-references.png)
   
1. Create two new variables in Variable Manager of the `Number` type for the input event values. Name them `MoveY Input Value` and `MoveX Input Value`.

1. Delete the yellow `Number` connections between the **Input Handler** and **From Values** nodes. Convert the appropriate slots to use variable references and drag your new variables into them. 

    Your graph should look like the following.

    ![Graph that is using two Number-type variables](/images/learning-guide/tutorials/input-and-movement/grid-based-movement/sc-using-variable-references-2.png)
    
1. Save the graph and test your level.

### Add smooth motion over time with the **Lerp Between** node

You may have noticed that it is difficult to see the instant movements of the Shader Ball.  Now you will replace instant movement with smooth movement that happens over time using _linear interpolation_, or _Lerp_ for short. 

1. With your Script Canvas graph open, disconnect both of the **Set World Translation** nodes.  To disconnect a node, you can delete its connections or **Hold left-click** on the node and shake it back and forth.

1. Add a single **Lerp Between** node from the **Math** category.  Connect the **Out** pins from both **Add (+)** nodes to the **In** pin of the **Lerp Between** node.

1. Convert the **Start** and **Stop** slots to use a reference and **drag and drop** the `Start Location` and `End Location` variables to the appropriate slot.

1. The value in the **Maximum Duration** slot will determine how long the movement between the start and end locations will take.  Rather than hard code a value into the **Maximum Duration** slot, create a variable you can modify from the Script Canvas component.  In Variable Manager, create a variable with the `Number` type named `Move Duration`. In the **Node Inspector** tool, set the **Initial Value Source** parameter to `From Component`.

    Your `Move Duration` variable should be set up like the following.

    ![Move Duration node in the Variable Manager and Node Inspector](/images/learning-guide/tutorials/input-and-movement/grid-based-movement/sc-move-duration.png)

1. Convert the **Maximum Duration** slot to use a reference and set it to use the new `Move Duration` variable.

1. Connect the **Tick** pin of the **Lerp Between** node to the **In** pin of the **Set World Translation** node.  Then, connect the **Step** slot to **World Translation**; this should automatically remove the reference to `End Location`.  Now, while the **Lerp Between** node is executing, a new value for **Step** will be calculated, and the **Set World Translation** node will execute with this new value.

1. Connect the **Lerp Complete** pin of the **Lerp Between** node to the **In** pin of the second **Set World Translation** node.  The **World Translation** slot should still use the `End Location` variable.  It's unlikely that the game ticks that occur during a Lerp add up perfectly to the **Maximum Duration** time.  If you did not set the world translation of the Shader ball to the desired `End Location` after every Lerp completed, small errors in the Shader Ball's position would accumulate, and the Shader Ball would not perfectly align to the grid.

    Your graph should look like the following.

    ![Graph that is using two Number-type variables](/images/learning-guide/tutorials/input-and-movement/grid-based-movement/sc-lerp-between.png)

1. Save the graph and locate the Script Canvas component in the Shader Ball's Entity Inspector.

1. The component's **Variables** property should list `Move Duration` as a variable.  Change it to a positive value like `1.0` to set the duration of the movement Lerp.  Variables set from the component are easy to modify; use them to quickly test and tune new values without the need to open Script Canvas.

1. Save the level and test the Shader Ball's movement.

### Block input events

There is a problem with the Shader Ball's movement!  If you press a key to move the Shader ball before the last movement is complete, the Shader Ball will no longer align with the grid.  You need to block new input events when the Shader Ball is moving.

1. In your Script Canvas graph, create a new `Boolean` type variable and name it `Is Moving`.  You'll use this variable to track when the Shader Ball is moving.  The default value for this variable is `False`, which is appropriate; when you enter Game Mode, the Shader Ball _is not_ moving.

1. Disconnect the **From Values** nodes from the **Input Handler** nodes.  You need to insert a conditional check after input events are received.

1. Add two **If** nodes from the **Logic** category.  Connect each of the **Input Handler** nodes to an **If** node, the **Pressed** pin should connect to the **In** pin of the **If** node.

1. Convert the **Condition** slots to use a reference and **drag and drop** the `Is Moving` variable on them.  The **If** nodes have two output pins, only the pin with the same value as `Is Moving` will execute.  When `Is Moving` is `True`, you want to block additional movement, so the **True** pin should not connect to anything.  When `Is Moving` is `False`, you want the movement logic to execute, so connect the **False** pin to the **In** slot of the **From Values** node.

1. You also need to change the value of the `Is Moving` variable to `True` when the movement logic begins to execute.  Add two **Set Is Moving** nodes to the graph.  You can find the node in the Node Palette tool, alternatively, hold **Alt** and **left-click and drag** the `Is Moving` variable from Variable Manager.  Insert these nodes on the connection between the **If** and **From Values** nodes by **left-clicking and dragging** the node and hovering the mouse over the connection.

1. **Left-click** on the **Boolean** slot of the two **Set Is Moving** nodes so that the box is checked.  This sets the value of `Is Moving` to `True` when the node executes.

    Your graph should look like the following.

    ![If and Set is Moving nodes are inserted after the Input Handlers to check for movement](/images/learning-guide/tutorials/input-and-movement/grid-based-movement/sc-movement-check.png)

1. When a movement is complete, the `Is Moving` variable should be set to `False` so that the conditional check won't block the next movement.  Add a single **Set Is Moving** node to the graph and connect it to the final **Set World Translation** node that executes when a Lerp completes.  The **Boolean** slot of **Set Is Moving** should be unchecked, which sets the variable to `False`.

    Your graph should look like the following.

    ![Set Is Moving node is reset to False after Lerp completes](/images/learning-guide/tutorials/input-and-movement/grid-based-movement/sc-reset-is-moving.png)
    
1. Save the graph and test your level.  Now the Shader Ball cannot move until the last movement is complete, and it stays aligned with the grid.

## Related resources

| Resource | Description |
|-|-|
| [Using player input in O3DE](/docs/user-guide/interactivity/input/using-player-input/) | User Guide topics related to input. |
| [Creating gameplay and other behaviors with Script Canvas](/docs/user-guide/scripting/script-canvas/) | User Guide topics related to Script Canvas. |
| [Input component](/docs/user-guide/components/reference/gameplay/input/) | Reference for the Input component. |
| [Script Canvas component](/docs/user-guide/components/reference/scripting/script-canvas/) | Reference for the Script Canvas component. |
