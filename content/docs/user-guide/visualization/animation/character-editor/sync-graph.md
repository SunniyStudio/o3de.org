---
description: ' 在 Open 3D Engine中，使用同步动画图来同步演员之间的动画。 '
title: '同步动画图形： 示例'
---

您可以使用同步动画图来同步演员之间的动画。例如，一个角色的动画可能会触发另一个角色的动画。一个动画图可以是主要动画图，也可以有多个辅助动画图。同样，一个动画图既可以是一个图的辅助图，也可以是另一个图的主要图。

本主题介绍同步两个动画图形的以下主要步骤：

1. 为要同步的实体添加所需的组件，包括 Anim Graph 组件。

1. 使用动画编辑器创建动作集和一个或多个动画图形。

1. 为辅助图形添加一个参数，以便从主图形接收更改事件。

1. 为主图表添加一个参数。

1. 在主图表中添加仆从参数操作，以便向辅助图表发送更改事件。

1. 使用 O3DE 的 [Event Bus (EBus)](/docs/user-guide/programming/messaging/ebus/) 系统和 Lua 脚本同步图形。

本专题以两个角色（一个机器人角色（“Jack”）和一个枪角色）为例说明图形同步。当玩家激活同步模式并使用键盘开火时，机器人会做出开火动作，枪也会开火。当玩家关闭同步模式时，机器人会做出开火动作，但枪支不会开火。

![The example game scene.](/images/user-guide/actor-animation/char-animation-editor-sync-graph-1.png)

机器人使用`syncFeature_Jack`动画图形，喷枪使用`syncFeature_Gun` 图形。机器人图形是主要图形，喷枪图形是次要图形。

同步模式开启时，枪会与机器人同步开火。当机器人开火时，辅助图的`shoot`参数将接收主图的 `gunTrigger`参数状态。附属于喷枪的辅助图形会接收参数变化事件并发射喷枪。

## 1. 添加所需组件

第一步是为要同步的实体添加所需的组件。示例中实体使用的机器人和喷枪组件将在以下章节中介绍。

### Robot Entity 组件

机器人实体使用以下组件：
+ **Transform**
+ **Actor**
+ **Anim Graph**
+ **Input**
+ **Lua Script**

下图显示了实体检查器中机器人实体组件的配置。

![The components for the robot entity.](/images/user-guide/actor-animation/char-animation-editor-sync-graph-robot-components.png)

### Gun Entity 组件

喷枪实体使用以下组件：
+ **Transform**
+ **Actor**
+ **Lua Script**
+ **Anim Graph**
+ **Input**

下图显示了实体检查器中枪支实体组件的配置。

![The components for the gun entity in Entity Inspector.](/images/user-guide/actor-animation/char-animation-editor-sync-graph-gun-components.png)

## 2. 创建运动集和动画图形

设置好实体和组件后，创建一个运动集和两个动画图形。运动集包含图形所使用的运动，而辅助和主要动画图形则对实体进行动画和同步。

**创建运动集和两个动画图形**

1. 创建动作集。有关创建动作集的信息，请参阅[动画编辑器入门](/docs/user-guide/visualization/animation/animation-editor/quick-start/)。本例的**MotionSet0**包含`gunshootanimation`、`jack_shoot`和`jack_idle`动作。

![Example motion set.](/images/user-guide/actor-animation/char-animation-editor-sync-graph-2.png)

1. 创建辅助动画图。有关创建辅助动画图的信息，请参阅 [动画编辑器入门](/docs/user-guide/visualization/animation/animation-editor/quick-start/)。辅助图形控制一个或多个实体，其动作由主图形决定。

   本例中的`syncFeature_Gun`二级图具有一个 **BindPose0** 节点和一个 **Motion0** 节点。**Motion0** 节点包含 `gunshootanimation` 动作。

   ![Motion node with associated animation.](/images/user-guide/actor-animation/char-animation-editor-sync-graph-3.png)

1. 创建主图形。主图形向辅助图形发送控制事件。

   本例的 `syncFeature_Jack` 主动画图中有一个 **Motion1** 节点和一个 **Motion0** 节点。**Motion1** 节点包含 `jack_idle` 动作，而 **Motion0** 节点包含`jack_shoot`动作。

   ![Motion node with associated idle motion.](/images/user-guide/actor-animation/char-animation-editor-sync-graph-4.png)

   ![Motion node with associated animation.](/images/user-guide/actor-animation/char-animation-editor-sync-graph-5.png)

## 3. 在辅助图形中添加参数

现在，您已准备好向辅助图形添加参数，该图形将接收来自主图形的参数更改事件。添加参数后，再添加参数条件，指定动画从一个动作过渡到另一个动作的时间。

**要在辅助图形中添加一个参数，以接收来自主图形的更改事件**

1. 在辅助动画图形的**Parameters**选项卡上，单击加号（**+**）图标，然后选择**Add parameter**。

![Choose Add parameter in Animation Editor.](/images/user-guide/actor-animation/char-animation-editor-sync-graph-6.png)

1. 在**Create Parameter**对话框的**Value type**中，选择要用于参数的数据类型。本例使用 **Boolean (checkbox)** 值类型，因为枪炮触发器要么开要么关。

![Choose a value type for the parameter.](/images/user-guide/actor-animation/char-animation-editor-sync-graph-7.png)

1. 在 **Name** 中，输入参数的名称。本例使用`gunTrigger`。

![Enter a parameter name.](/images/user-guide/actor-animation/char-animation-editor-sync-graph-8.png)

1. 点击**Create**。**Parameters**列表会显示您创建的参数。

![Parameter added to the animation graph.](/images/user-guide/actor-animation/char-animation-editor-sync-graph-9.png)

### 为辅助图形添加参数条件

在本节中，在过渡线上添加参数条件，指定动画何时发生变化。在示例中，条件表示是否已按下喷枪扳机。

**为辅助图形添加参数条件**

1. 单击从 **BindPose0** 节点到 **Motion0** 节点的过渡线。然后，在**Attributes**窗格中，单击**Add condition**，并选择**Parameter Condition**。

![Select the transition line to add a parameter condition.](/images/user-guide/actor-animation/char-animation-editor-sync-graph-10.png)

1. 点击 **Select parameter**。

![Click Select parameter.](/images/user-guide/actor-animation/char-animation-editor-sync-graph-11.png)

1. 在**Parameter Selection Window**中，选择刚刚创建的参数，然后点击 **OK**。

![Choose a parameter for the parameter condition.](/images/user-guide/actor-animation/char-animation-editor-sync-graph-12.png)

   在**Attributes**窗格中，**Parameter Condition**部分显示了您添加的参数。在过渡行上，一个小圆节点表示该行有一个参数条件。

   ![Small round node indicating a parameter condition.](/images/user-guide/actor-animation/char-animation-editor-sync-graph-13.png)

1. 对于 **Test Function**，使用 **param > testValue** 的默认值。在本例中，这意味着如果触发器接收到的值大于 0，枪就会发射。
![Specifying a test function for the parameter condition.](/images/user-guide/actor-animation/char-animation-editor-sync-graph-14.png)

1. 对于 **Test Value**， 保持默认值 0.0。

1. 单击从 **Motion0** 节点到 **BindPose0** 节点的过渡线。然后，在**Attributes**窗格中，单击**Add condition**，并选择**Parameter Condition**。

![Select the opposite transition line to add another parameter condition.](/images/user-guide/actor-animation/char-animation-editor-sync-graph-15.png)

1. 点击 **Select parameter**。

1. 在**Parameter Selection Window**中，选择要使用的参数。示例中使用的是 `gunTrigger` 参数。

1. 对于 **Test Function**，使用 **param == testValue** 的默认值。在本例中，这意味着如果触发值等于零，则运动会转回空闲状态，枪支不再开火。

1. 对于 **Test Value**，保持默认值 0.0。

现在，辅助动画图形已准备好接收来自主图形的信号。

## 4. 在主图表中添加参数和参数条件 

在主图表中添加参数和参数条件，就像在辅助图表中一样。不过，您还可以在主图形中添加辅助（“仆人”）参数动作。这些操作会向辅助图形发出信号，使其模仿主图形的动画。

**向主图形添加参数**

1. 在**Parameters**标签页上，点击 (**+**) 图标，选择 **Add parameter**。

![Adding a parameter to the primary graph.](/images/user-guide/actor-animation/char-animation-editor-sync-graph-16.png)

1. 在**Create Parameter**对话框的**Value type**中，选择要用于参数的数据类型。本例使用 **Boolean (checkbox)** 值类型，因为枪炮触发器要么开要么关。

![Choosing a value type for the parameter.](/images/user-guide/actor-animation/char-animation-editor-sync-graph-17.png)

1. 在 **Name** 中，输入参数的名称。本例使用 `shoot`。

![Enter a name for the parameter for the primary graph.](/images/user-guide/actor-animation/char-animation-editor-sync-graph-18.png)

1. 点击 **Create**。

### 在主图表中添加参数条件

现在，您可以在主图表的过渡线上添加参数条件，就像在辅助图表上一样。

**向主图形添加参数条件**

1. 单击从 **Motion1** 节点到 **Motion0** 节点的过渡线。然后，在**Attributes**窗格中，单击**Add condition**，并选择**Parameter Condition**。

![Click the transition line to add a parameter condition.](/images/user-guide/actor-animation/char-animation-editor-sync-graph-19.png)

1. 点击 **Select parameter**。

1. 在**Parameter Selection Window**中，选择刚刚创建的参数。本例使用 `shoot`。

![Choose a parameter for the parameter condition.](/images/user-guide/actor-animation/char-animation-editor-sync-graph-20.png)

1. 对于 **Test Function**，使用**param > testValue**的默认值。

1. 对于 **Test Value**，使用**0.0**的默认值。

1. 单击从 **Motion0** 节点到 **Motion1** 节点的过渡线。然后，在**Attributes**窗格中，单击**Add condition**，并选择**Parameter Condition**。

![Add a parameter condition to the other transition line.](/images/user-guide/actor-animation/char-animation-editor-sync-graph-21.png)

1. 点击 **Select parameter**。

1. 在**Parameter Selection Window**中，选择要使用的参数。示例中使用的是 `shoot`。

1. 对于 **Test Function**，使用 **param == testValue** 的默认值。

1. 对于 **Test Value**，使用 **0.0** 的默认值。

## 5. 在主图表中添加仆从参数操作

现在，您可以向主图表添加辅助（“仆人”）参数操作。脚本将使用这些操作来同步两个图表。

**向主图表添加仆从参数操作**

1. 在动画编辑器中，再次单击选择第一条过渡线。本例选择了从`jack_idle`节点 **Motion1** 到`jack_shoot`节点 **Motion0** 的线条。

![Click the transition line to add a servant parameter action.](/images/user-guide/actor-animation/char-animation-editor-sync-graph-22.png)

1. In the **Attributes** pane for the primary graph, click **Add action**, **Servant Parameter Action**.

![Click Add action, Servant Parameter Action.](/images/user-guide/actor-animation/char-animation-editor-sync-graph-23.png)

1. For **Servant Parameter Action**, in the **Trigger Mode** box, keep the default **On Enter**. **On Enter** specifies that the action is triggered when the state or transition is entered.

![Use On Enter for Trigger Mode.](/images/user-guide/actor-animation/char-animation-editor-sync-graph-24.png)

1. In the **Servant anim graph** box, click **Browse** (**...**) and choose the secondary animation graph that has the parameter that you want to use. This example chooses a secondary animation graph called **syncFeature\_Gun**.

1. Click **Select parameter** to choose a parameter from the secondary animation graph that you just chose. This example chooses the **gunTrigger** parameter.

![Click Select parameter.](/images/user-guide/actor-animation/char-animation-editor-sync-graph-25.png)

![Choose a parameter for the servant parameter action.](/images/user-guide/actor-animation/char-animation-editor-sync-graph-12.png)

   On the transition line, a small square node indicates that the transition line has a parameter action. The small round node next to it represents the parameter condition that you added earlier.

   ![Small square node indicating a parameter action.](/images/user-guide/actor-animation/char-animation-editor-sync-graph-26.png)

1. For **Trigger value**, specify the value to emit when the action is triggered. Because **Trigger value** is treated as a single float, you can use it for float, Boolean, and integer parameters. This example specifies `1.0`, which is the value when the gun fires.

![Specify a trigger value.](/images/user-guide/actor-animation/char-animation-editor-sync-graph-27.png)

1. Click the line from **Motion0** to **Motion1** and repeat the steps to add a servant parameter action to the remaining transition line.

1. In the **Trigger value** box, specify a different value. The example specifies a trigger value of `0.0`, which is the value when the gun is idle.

Now that the animation graphs are ready, you can perform the next steps: gathering user input and writing Lua scripts to synchronize the graphs.

## 6. Synchronize the Primary and Secondary Graphs 

Synchronizing the primary and secondary graphs involves the following steps:

1. Getting keyboard input from the player.

1. Placing a Lua script component and Lua script on the secondary entity.

1. Placing a Lua script component and Lua script on the primary entity.

The Lua scripts synchonize the two graphs by handling animation graph events in O3DE's [Event Bus (EBus)](/docs/user-guide/programming/messaging/ebus/) system.

### Getting Input from the Player 

In the example, the synchronization state of the primary and secondary graphs and the firing of the gun are controlled by the following keyboard inputs, or keystrokes, from the user. The Event Value Multiplier is the actual value sent to the input system and to Lua script.


****

| Keystroke | Description | Event Value Multiplier |
| --- | --- | --- |
| 1 | Turns on sync mode. | 1 |
| 2 | Turns off sync mode (off by default). | -1 |
| S | Fires the gun. | 1 |
| D | Stops the gun from firing. | -1 |

To gather these inputs, the example adds [Input](/docs/user-guide/components/reference/gameplay/input/) components to the robot entity and to the gun entity.

To use the Input component, you must enable the [Starting Point Input](/docs/user-guide/gems/reference/input/starting-point-input) Gem for your project. The Starting Point Input Gem interprets hardware input and converts it into input events such as `pressed`, `released`, and `held`.

Each Input component references an `.inputbindings` file. An `.inputbindings` file binds a set of inputs to an event. These inputs can come from sources such as a mouse, keyboard, or game controller. To create an input bindings file, you can use the **Input Bindings Editor** in O3DE Editor. For more information, refer to [Using Player Input in Open 3D Engine](/docs/user-guide/interactivity/input/using-player-input).

**Getting Keyboard Input to Control Graph Synchronization**
In the example, the gun entity has an Input component. The Input component uses a `synctest.inputbindings` asset to bind keyboard inputs **1** and **2** to the `SyncControl` event. The `SyncControl` event controls the sync mode, which determines whether or not the gun fires when the robot fires.

The following image shows the corresponding input bindings in the **Input Bindings Editor**.

![Sample input bindings asset for sync control in the Input Bindings Editor.](/images/user-guide/actor-animation/char-animation-editor-sync-graph-28.png)

**Getting Keyboard Input to Control Shooting**
The Input component on the robot uses a `syncgun.inputbindings` asset to bind keyboard inputs **S** and **D** to the `ShootControl` event. The `ShootControl` event controls the firing of the gun.

The following image shows the corresponding input bindings in the **Input Bindings Editor**.

![Sample input bindings asset for shoot control in the Input Bindings Editor.](/images/user-guide/actor-animation/char-animation-editor-sync-graph-29.png)

### Using a Script on the Secondary Graph to Toggle Synchronization 

The example uses Lua Script components on both the primary (robot) and secondary (gun) entities. The script on the primary entity controls the firing of the gun. The script on the secondary entity controls the sync mode. To add a Lua script to an entity, add a [Lua Script component](/docs/user-guide/components/reference/scripting/lua-script/) to the entity and then attach the script to the component.

The Lua script on the gun entity receives the input from the keyboard to toggle the sync mode. To synchronize the secondary graph to the primary graph, the script uses the `SyncAnimGraph` EBus event. In the following example, the `self.entityId` parameter refers to the secondary entity (the gun). The `self.Properties.PrimaryEntity` parameter refers to the robot.

```
AnimGraphComponentRequestBus.Event.SyncAnimGraph(self.entityId,self.Properties.PrimaryEntity);
```

To desynchronize the secondary graph from the primary graph, the same script uses the `DesyncAnimGraph` EBus event.

```
AnimGraphComponentRequestBus.Event.DesyncAnimGraph(self.entityId,self.Properties.PrimaryEntity);
```

The full script shows how the `SyncAnimGraph` and `DesyncAnimGraph` methods handle the `SyncControl` input event.

```
-- syncSample.lua
local syncSample =
{
	Properties =
    {
		PrimaryEntity = { default = EntityId() }
	},
}

function syncSample:OnActivate()
    self.SyncControlInputBusId = InputEventNotificationId("SyncControl");
    self.SyncControlInputBus = InputEventNotificationBus.Connect(self, self.SyncControlInputBusId);
    self.ShootControlInputBusId = InputEventNotificationId("ShootControl");
    self.ShootControlInputBus = InputEventNotificationBus.Connect(self, self.ShootControlInputBusId);
	self.SyncControl = false;
	self.Shooting = false;
end

function syncSample:HandleSyncControl(floatValue)
	if (floatValue > 0 and self.SyncControl == false ) then
		AnimGraphComponentRequestBus.Event.SyncAnimGraph(self.entityId, self.Properties.PrimaryEntity);
		self.SyncControl = true;
	elseif(floatValue < 0 and self.SyncControl == true ) then
		AnimGraphComponentRequestBus.Event.DesyncAnimGraph(self.entityId, self.Properties.PrimaryEntity);
		self.SyncControl = false;
	end
end

function syncSample:HandleInput(floatValue)
	if (InputEventNotificationBus.GetCurrentBusId() == self.SyncControlInputBusId) then
		self:HandleSyncControl(floatValue);
	end
end

function syncSample:OnPressed(floatValue)
	self:HandleInput(floatValue);
end

function syncSample:OnHeld(floatValue)
    self:HandleInput(floatValue);
end

return syncSample;
```

### Using Script on the Primary Graph to Control Shooting 

The example Lua script on the primary entity (the robot) receives the keyboard input that toggles the firing of the gun. The robot entity's animation graph's `shoot` parameter uses the **Boolean (checkbox)** type. When the gun fires, the `shoot` parameter is true. Because `shoot` is a named Boolean parameter, the Lua script on the primary entity uses the `SetNamedParameterBool` function on the `AnimGraphComponentBus`.

The full script shows how the `SetNamedParameterBool` function is used to toggle the shooting status.

```
-- syncGun.lua
local syncGun =
{
}

function syncGun:OnActivate()
    self.ShootControlInputBusId = InputEventNotificationId("ShootControl");
    self.ShootControlInputBus = InputEventNotificationBus.Connect(self, self.ShootControlInputBusId);
	self.Shooting = false;
end

function syncGun:HandleShootControl(floatValue)
	if (floatValue > 0 and self.Shooting == false ) then
		AnimGraphComponentRequestBus.Event.SetNamedParameterBool(self.entityId, "shoot", true);
		self.Shooting = true;
	elseif(floatValue < 0 and self.Shooting == true ) then
		AnimGraphComponentRequestBus.Event.SetNamedParameterBool(self.entityId, "shoot", false);
		self.Shooting = false;
	end
end

function syncGun:HandleInput(floatValue)
	if (InputEventNotificationBus.GetCurrentBusId() == self.ShootControlInputBusId) then
		self:HandleShootControl(floatValue);
	end
end

function syncGun:OnPressed(floatValue)
	self:HandleInput(floatValue);
end

function syncGun:OnHeld(floatValue)
    self:HandleInput(floatValue);
end

return syncGun;
```

## The Example in Action 

The following animated image shows the finished example in action when the sync mode is turned off. The robot fires, but the gun does not.

![Sync mode off](/images/user-guide/actor-animation/char-animation-editor-sync-graph-example-sync-off.gif)

The following animated image shows the example when the sync mode is turned on. The gun fires when the robot fires.

![Sync mode on](/images/user-guide/actor-animation/char-animation-editor-sync-graph-example-sync-on.gif)
