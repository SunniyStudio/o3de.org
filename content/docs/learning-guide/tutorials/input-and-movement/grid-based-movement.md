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

1. 在 Entity Outliner 中选择 Shader Ball。 在 Entity Inspector 中，将 Script Canvas 组件添加到 Shader Ball。在新的 Script Canvas 组件上，**左键单击** {{< icon "open-in-internal-app.svg" >}} 按钮以打开 Script Canvas 编辑器。

1. 将 **On Graph Start** 节点添加到新图表中。 您可以在 **Note Palettte** 中搜索此节点，并将其 **拖动** 到编辑器的中心以创建新图形。 当您在 _Game Mode_ 中播放关卡时，**On Graph Start** 节点将在 Script Canvas 组件处于活动状态后立即执行一次。

1. 对于您希望图形处理的每个事件，必须有一个 **Input Handler** 节点。 将两个 **Input Handler** 节点添加到图表中。 在 **事件名称** 字段中，键入事件的名称 `MoveX` 和 `MoveY`。 将 **On Graph Start** 节点的 **Out** 引脚连接到两个 **Input Handler** 节点的 **Connect Event** 引脚。 现在，每当按下相应的键盘键时，将执行节点的 **Pressed**、**Held** 和 **Released** 插槽。按下某个键时，您可以通过将 **value** 输出槽连接到图形中的节点来捕获和使用事件的值。

    连接这些节点后，您的 Script canvas 图形应如下所示。

    ![Script Canvas graph with On Graph Start and Input Handler nodes connected](/images/learning-guide/tutorials/input-and-movement/grid-based-movement/sc-input-handlers.png)
    
1. 将两个 **From Values** 节点添加到 **Math > Vector3** 类别中的图形中。 您将使用这些节点创建一个 `Vector3`，表示在四个网格方向之一上的单个移动。将 `MoveY` 的 **Input Handler** 的 **Pressed** 引脚连接到其中一个 **From Values** 节点的 **In** 引脚。 然后，将 **value** 引脚连接到该节点的 **Y** 插槽。

1. 以相同的方式将 `MoveX` **Input Handler** 连接到第二个 **From Values** 节点，但这次将 **value** 引脚连接到 **X** 插槽。 当你按下`A` 或 `D` 键，此节点将输出 `(-1, 0, 0)` 或 `(1, 0, 0)`。

1. 要使 Shader Ball 移动，您需要将`Vector3`添加到球的当前世界平移中，然后将 Shader Ball 的平移设置为结果。 将两个 **获取世界翻译** 节点添加到图表中，并将 **From Values** 节点的 **Out** 引脚连接到 **Get World Translation **节点的 **In** 引脚。

    {{< note >}}
这些节点的 **EntityID** 字段设置为 '`Self`';这意味着，当节点执行时，它将返回 Script Canvas 图形所在的实体的转换。您可以通过将此图形附加到 Shader Ball 来简化所有与 Shader Ball 相关的引用。
{{< /note >}}

1. 接下来，您需要将移动向量和平移向量相加。 将 **Math** 类别中的两个 **Add (+)** 节点添加到图形中，并将 **Get World Translation** 节点的 **Out** 引脚连接到 **Add (+)** 节点的 **In** 引脚。 对于每个事件，将 **Get World Translation** **Translation** 插槽连接到 **Value 0**，并将 **From Values** **Vector3** 插槽连接到 **Value 1**。 现在，当 **Add (+)** 节点执行时，其 **Result** 插槽表示 Shader Ball 应移动到的世界平移。

1. 将两个 **Set World Translation** 节点添加到图表中。 将 **Add (+)** 节点的 **Out** 引脚连接到新节点的 **In** 引脚。 将 **Result** 引脚连接到 **World Translation** 插槽。 “设置世界转换”节点的“源”插槽设置为`Self`。

    连接这些节点后，您的 Script canvas 图形应如下所示。
2. 
    ![Script Canvas graph with movement and translation vectors added and the result set to the Shader Ball's world translation](/images/learning-guide/tutorials/input-and-movement/grid-based-movement/sc-set-world-translation.png)

1. 将 Script Canvas 图形保存在项目的目录中。 在 Shader Ball 的 Script Canvas 组件中，**左键单击** {{< icon "browse-edit-select-files.svg" >}} 按钮，然后选择您创建的 Script Canvas 图形ted.

1. 保存关卡。 然后，按 **Ctrl + G** 进入游戏模式，测试您是否已正确设置所有内容。 您应该能够按 F**W**、**A**、**S** 和 **D** 在网格中移动 Shader Ball。

### 使用变量简化图形

Script Canvas 变量通过将节点之间的连接替换为引用来简化图形的视觉复杂性。 图形可能还需要某些变量才能正确执行;Output Pin（输出引脚）的值可能仅在节点执行后的有限时间内有效。 最佳做法是将值保存为变量，以便以后可靠地访问它们。 在本节中，您将使用变量改进之前在本教程中创建的 Script Canvas 图形。

1. 如果您的 Script Canvas 图形尚未在 Script Canvas 编辑器中打开，请 **左键单击** Shader Ball 的 Script Canvas 组件上的 {{< icon "open-in-internal-app.svg" >}} 按钮。

1. 在 Script Canvas 编辑器的 **Variable manager** 工具中，选择 **Create Variable** 并从下拉列表中选择 `Vector3`以创建新的 `Vector3` 变量。将新变量命名为`Start Location`;它将表示 Shader Ball 在移动之前的平移。

1. 创建另外两个 `Vector3` 变量，分别命名为 `End Location` 和 `Move Vector`。

    添加这些变量后，您的 Variable Manager 应如下所示。

   ![The Variable Manager tool with three Vector3 variables in it](/images/learning-guide/tutorials/input-and-movement/grid-based-movement/sc-variable-manager.png)

1. 通过选择每个连接并按 **Delete** 来删除图形中所有绿色的 `Vector3` 连接。

1. 如果插槽未连接，您可以通过 **双击** 引脚或 **右键单击** 并选择 **Convert to Reference** 来使其成为变量引用。将所有 `Vector3` 插槽转换为参考。

1. 通过将 Variable Manager 中的相应变量拖放到插槽中来设置引用。 将 `Move Vector` 变量拖到 **From Values** 节点的 **Vector3** 插槽中。

1. 转换图形中剩余的绿色 `Vector3` 引脚以使用引用并向其添加适当的变量。

    添加这些变量引用后，您的图表应如下所示。

   ![Graph that is using only variable references in Vector3 input and output slots](/images/learning-guide/tutorials/input-and-movement/grid-based-movement/sc-using-variable-references.png)
   
1. 在 Variable Manager 中为输入事件值创建两个`Number`类型的新变量。为他们命名`MoveY Input Value` 和 `MoveX Input Value`。

1. 删除 **Input Handler** 和 **From Values** 节点之间的黄色`Number`连接。转换相应的插槽以使用变量引用，并将新变量拖到其中。

    您的图表应如下所示。

    ![Graph that is using two Number-type variables](/images/learning-guide/tutorials/input-and-movement/grid-based-movement/sc-using-variable-references-2.png)
    
1. 保存图表并测试您的关卡。

### 使用 **Lerp Between** 节点添加随时间变化的平滑运动

您可能已经注意到，很难看到 Shader Ball 的即时移动。 现在，您将使用 _linear interpolation_ 或简称 _Lerp_ 将 instant movement 替换为随时间推移发生的平滑移动。

1. 打开 Script Canvas 图形后，断开两个 **Set World Translation** （设置世界转换） 节点的连接。 要断开节点，您可以删除其连接，或在节点上按住 **单击鼠标左键** 并来回摇动它。

1. 从 **Math** 类别中添加一个 **Lerp Between** 节点。 将两个 **Add (+)** 节点的 **Out** 引脚连接到 **Lerp Between** 节点的 **In** 引脚。

1. 将 **Start** 和 **Stop** 插槽转换为使用引用，并将`Start Location` 和 `End Location`变量 **拖放** 到适当的插槽。

1. **Maximum Duration** 插槽中的值将决定在开始位置和结束位置之间移动所需的时间。 不要将值硬编码到 **Maximum Duration** 插槽中，而是创建一个可以从 Script Canvas 组件修改的变量。 在 Variable Manager 中，创建一个名为`Move Duration`的`Number`类型的变量。在 **Node Inspector** 工具中，将 **初始值源** 参数设置为`From Component`。

    `Move Duration` 变量的设置方式应如下所示。

    ![Move Duration node in the Variable Manager and Node Inspector](/images/learning-guide/tutorials/input-and-movement/grid-based-movement/sc-move-duration.png)

1. 将 **Maximum Duration** 槽转换为使用引用，并将其设置为使用新的`Move Duration`变量。

1. 将 **Lerp Between** 节点的 **Tick** 引脚连接到 **Set World Translation** 节点的 **In** 引脚。 然后，将 **Step** 插槽连接到 **World Translation**;这应该会自动删除对 `End Location` 的引用。 现在，在执行 **Lerp Between** 节点时，将计算 **Step** 的新值，并且 **Set World Translation** 节点将使用此新值执行。

1. 将 **Lerp Between** 节点的 **Lerp Complete** 引脚连接到第二个 **Set World Translation** 节点的 **In** 引脚。 **World Translation** 插槽仍应使用 `End Location` 变量。 在 Lerp 期间发生的游戏更新时间不太可能与 **Maximum Duration** 时间完全叠加。 如果在完成每个插值后没有将 Shader Ball 的世界平移设置为所需的 `End Location`，则 Shader Ball 位置中的小错误会累积，并且 Shader Ball 不会完全对齐网格。

    您的图表应如下所示。

    ![Graph that is using two Number-type variables](/images/learning-guide/tutorials/input-and-movement/grid-based-movement/sc-lerp-between.png)

1. 保存图形并在 Shader Ball 的 Entity Inspector 中找到 Script Canvas 组件。

1. 组件的 **Variables** 属性应将`Move Duration`列为变量。 将其更改为正值（如 `1.0` ）以设置移动 Lerp 的持续时间。 从组件设置的变量很容易修改;使用它们可快速测试和调整新值，而无需打开 Script Canvas。

1. 保存关卡并测试 Shader Ball 的移动。

### 块输入事件

Shader Ball 的移动有问题！ 如果在最后一次移动完成之前按某个键移动 Shader Ball，则 Shader Ball 将不再与网格对齐。 您需要在 Shader Ball 移动时阻止新的输入事件。

1. 在 Script Canvas 图表中，创建一个新的`Boolean`类型变量，并将其命名为`Is Moving`。 您将使用此变量来跟踪 Shader Ball 何时移动。 此变量的默认值为`False`，这很合适;当您进入 Game Mode（游戏模式）时，Shader Ball _is not_移动。

1. 断开 **From Values** 节点与 **Input Handler** 节点的连接。 您需要在收到输入事件后插入条件检查。

1. 从 **Logic** 类别添加两个 **If** 节点。 将每个 **Input Handler** 节点连接到一个 **If** 节点，**Pressed** 引脚应连接到 **If** 节点的 **In** 引脚。

1. 将 **Condition** 插槽转换为使用引用，并在其上 **拖放** `Is Moving` 变量。 **If**节点有两个输出引脚，则只会执行与'Is Moving'具有相同值的引脚。 当 `Is Moving`为 `True` 时，您希望阻止其他移动，因此 **True** 引脚不应连接到任何对象。 当 `Is Moving`为 `False` 时，您希望执行移动逻辑，因此将 **False** 引脚连接到 **From Values** 节点的 **In** 插槽。

1. 当移动逻辑开始执行时，您还需要将 `Is Moving` 变量的值更改为 `True`。 向图表中添加两个 **Set Is Moving** 节点。 您可以在 Node Palette （节点调色板） 工具中找到该节点，或者，按住 **Alt** 并 **左键单击并拖动** Variable Manager 中的 `Is Moving`变量。 通过左键单击并拖动节点并将鼠标悬停在连接上，将这些节点插入到 **If** 和 **From Values** 节点之间的连接上。

1. 在两个 **Set Is Moving** 节点的 **Boolean** 插槽上单击鼠标左键，以选中该框。 这会在节点执行时将 'Is Moving' 的值设置为 `True`。

    您的图表应如下所示。

    ![If and Set is Moving nodes are inserted after the Input Handlers to check for movement](/images/learning-guide/tutorials/input-and-movement/grid-based-movement/sc-movement-check.png)

1. 当一个动作完成时，`Is Moving` 变量应该设置为 `False`，这样条件检查就不会阻止下一个动作。 将单个 **Set Is Moving** 节点添加到图表中，并将其连接到在 Lerp 完成时执行的最终 **Set World Translation** 节点。 应取消选中 **Set Is Moving**的 **Boolean** 插槽，这会将变量设置为 `False`。

    您的图表应如下所示。

    ![Set Is Moving node is reset to False after Lerp completes](/images/learning-guide/tutorials/input-and-movement/grid-based-movement/sc-reset-is-moving.png)
    
1. 保存图表并测试您的关卡。 现在，Shader Ball 在最后一次移动完成之前无法移动，并且它与网格保持对齐。

## 相关资源

|资源 |描述 |
|-|-|
| [在 O3DE 中使用玩家输入](/docs/user-guide/interactivity/input/using-player-input/) | 与输入相关的用户指南主题。|
| [使用 Script Canvas 创建游戏和其他行为](/docs/user-guide/scripting/script-canvas/) | 与 Script Canvas 相关的用户指南主题。 |
| [Input 组件](/docs/user-guide/components/reference/gameplay/input/) | Input 组件的参考。 |
| [Script Canvas 组件](/docs/user-guide/components/reference/scripting/script-canvas/) | Script Canvas 组件的参考。 |
