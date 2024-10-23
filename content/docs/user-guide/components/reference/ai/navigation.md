---
linkTitle: Navigation
description: ' 使用Navigation组件可使实体在 Open 3D Engine中查找和跟踪路径。'
title: Navigation 组件
draft: True
---



**Navigation** 组件为人工智能运动（通常是在导航网格上）提供路径查找和路径跟踪功能。

![AI can use navigation to move along a path, typically on a navigation mesh.](/images/user-guide/component/component-navigation-path.png)

## Navigation 组件属性

**Navigation** 组件具有以下属性：

![Navigation component properties](/images/user-guide/component/component-navigation-properties.png)

**Agent Type**
为导航目的指定该人工智能的实体类型。在大型车辆和小型人形机器人有不同导航网格的情况下，定义代理类型将决定实体所遵循的[导航区域](/docs/user-guide/components/reference/ai/nav-area/)。这些代理类型在项目的 `Scripts\AI\Navigation.xml` 文件中定义。
要在导航区域定义代理类型，请参阅 **[Navigation Area](/docs/user-guide/components/reference/ai/nav-area/)** 组件。

**Agent Speed**
设置代理在使用变换或物理移动方法导航时的速度。
默认值: `1`

**Agent Radius**
设置用于导航的实体半径。在不考虑物理或其他碰撞问题的情况下，寻路器可利用此值在有障碍物的区域内移动，同时少走弯路。
默认值: `4`

**Arrival Distance Threshold**
设置当实体运动停止并被认为完成时，与终点的最小距离。
默认值: `0.25`

**Repath Threshold**
在计算实体的新路径之前，设置与之前已知位置的最小距离。
默认值: `1`

**Movement Method**
设置沿路径移动时使用的移动方法。可以时 Transform, Physics, 或 Custom。
默认值: `Transform`
+ **Transform** - 使用变换总线移动该组件所在的实体。这种方法忽略了所有物理特性，因此物体可以穿过墙壁和地形。
+ **Physics** - 如果实体具有PhysX Rigid Body, PhysX Character Controller, Rigid Body Physics 或 Character Physics组件，则使用物理原理移动实体。如果实体没有这些有效的物理组件，则不会移动。
+ **Custom** - 提供路径更新，让游戏逻辑随心所欲地移动实体。当您想移动使用根运动的动画实体时，此方法非常有用。通过监听 `OnTraversalPathUpdate` 通知，您可以将实体沿路径移向下一点。一旦实体到达到达距离阈值，就会收到另一个包含下一个路径位置的 `OnTraversalPathUpdate`通知，以此类推，直到到达路径终点。

**Allow Vertical Navigation**
如果希望导航代理在导航路径时包含垂直速度，则设置为 true；如果只希望将速度限制在 X 和 Y 平面（2D）上，则设置为 false。垂直导航可用于飞行实体或使用变换方法移动但必须垂直移动的实体。启用此属性还有助于防止实体在斜坡或陡峭地形上移动时出现 “楼梯踏步 ”现象。
默认值: `false`

## NavigationComponentRequestBus EBus 接口 

将下列请求函数与 `NavigationComponentRequestBus` 事件总线（EBus）接口结合使用，可与游戏中的其他组件进行通信。

有关使用 EBus 接口的更多信息，请参阅 [使用事件总线（EBus）系统](/docs/user-guide/programming/messaging/ebus/).

### FindPath 

查找请求的路径配置。

**参数**
`request` - 允许请求发出者覆盖该实体的一个、全部或全部寻路配置默认值。

**返回值**
该寻路请求的唯一标识符。

**可脚本化**
否

### FindPathToEntity 

创建寻路请求，向指定实体导航。

**参数**
`EntityId` - 您要导航的实体的 ID。

**返回值**
寻路请求的唯一标识符。

**可脚本化**
是

### FindPathToPosition 

创建寻路请求，向指定的世界位置导航。需要注意的是，虽然这看似是简单明了的寻路选择，但使用带有虚拟实体的 FindPathToEntity 通常更有用，因为如果在寻路完成前将虚拟实体移动到新位置，寻路会自动更新。

**参数**
`Destination` - 您要导航到的世界位置。

**返回值**
寻路请求的唯一标识符。

**可脚本化**
是

### Stop 

停止提供的 `requestId` 的所有寻路操作。使用 ID 确保要取消的请求是当前正在处理的请求。如果指定的 `requestId` 与当前请求的 ID 不同，则 Stop 命令将被忽略。

**参数**
`requestId` - 要取消的请求的 ID。

**返回值**
无

**可脚本化**
是

### GetAgentSpeed 

返回当前 AI Agent 的速度。

**参数**
无

**返回值**
返回当前代理的浮点速度。

**可脚本化**
是

### SetAgentSpeed 

更新 AI Agent 的速度。

**参数**
`agentSpeed` - 以浮点形式指定新的代理速度。

**返回值**
无

**可脚本化**
是

### GetAgentMovementMethod 

返回当前 AI Agent 的移动方式。

**参数**
无

**返回值**
返回当前代理的移动方式。

**可脚本化**
是

### SetAgentMovementMethod 

更新 AI Agent 的移动方式。

**参数**
`movementMethod` - 指定新的代理移动方法 (Transform, Physics 或 Custom)。

**返回值**
无

**可脚本化**
是

## NavigationComponentNotificationBus EBus Interface 

将以下通知函数与`NavigationComponentNotificationBus`事件总线（EBus）接口结合使用，可与游戏中的其他组件进行通信。

有关使用 EBus 接口的更多信息，请参阅 [使用事件总线（EBus）系统](/docs/user-guide/programming/messaging/ebus/)。

### OnSearchingForPath 

表示寻路请求已提交给导航系统。

**参数**
`requestId` - 正在搜索路径的请求的 ID。

**返回值**
无

**可脚本化**
是

### OnPathFound 

表示已为指定请求找到路径。

**参数**
`requestID` - 已找到路径的请求的 ID。
`currentPath` - 寻路器计算出的路径。

**返回值**
返回一个布尔值，表示是否要遍历此路径。

**可脚本化**
否

### OnTraversalStarted 

表示已开始对指定请求进行遍历。

**参数**
`requestId` - 已开始遍历的请求的 ID。

**返回值**
无

**可脚本化**
是

### OnTraversalInProgress 

表示正在对指定请求进行遍历。

**参数**
`requestId` - 正在进行遍历的请求的 ID。
`distanceRemaining` - 这条路径的剩余距离。

**返回值**
无

**可脚本化**
是

### OnTraversalPathUpdate 

表示遍历路径已更新。如果 `nextPathPosition` 和 `inflectionPosition` 相等，则表示路径的终点。

**参数**
`requestId` - 正在进行遍历的请求的 ID。
`nextPathPosition` - 我们在不与任何物体相撞的情况下可以移动到的路径上的最远点。
`inflectionPosition` - 超越 `nextPathPosition` 的路径上偏离直线路径的下一个点。

**返回值**
无

**可脚本化**
是

### OnTraversalComplete 

表示对指定请求的遍历成功完成。

**参数**
`requestId` - 已完成遍历的请求的 ID。

**返回值**
无

**可脚本化**
是

### OnTraversalCancelled 

表示指定请求的遍历在成功完成前被取消。如果找不到路径或请求被游戏停止，路径请求可能会被取消。

**参数**
`requestId` - 被取消遍历的请求的 ID。

**返回值**
无

**可脚本化**
是

## 导航寻路 Cvars 

**ai\_DrawPathFollower**
启用 PathFollower 调试绘图，显示代理路径和安全跟随目标。
`0` - 关闭
`1` - 开启
