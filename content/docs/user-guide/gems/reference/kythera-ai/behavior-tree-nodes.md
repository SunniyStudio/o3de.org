---
linkTitle: BT Editor 节点
title: Behavior Tree Editor节点
description: Kythera AI 行为树编辑器中所有可用节点的参考
weight: 800
toc: true
---
这是 [BT 编辑器](behavior-tree-editor)中所有可用的 Behavior Tree （行为树） 节点的参考。

## 叶操作节点

没有子节点的节点

### Ship movement

#### Ship\_Drift

保持飞船以当前速度移动（并允许它受到外部影响）

|     |     |     |     |     |
| --- | --- | --- | --- | --- |
|输入 |类型 |必需 |默认 |描述 |
| **Look** | EntityId, Position | no  |     | 船舶指向的方向 |

**输出：** 无

* * *

#### Ship\_FlyFlourishSpline

将导航样条线作为华丽的飞行。也就是说 - 直接在飞船前面复制样条并飞行

|     |     |     |     |     |
| --- | --- | --- | --- | --- |
|输入 |类型 |必需 |默认 |描述 |
| **Spline** | EntityId | yes | \-  | 要飞的样条曲线 |
| **SpeedOverride** | Float | no  | \-1.00 | 飞样条线的速度。在吊挂样条曲线时，每帧都会读取此数据。默认值 -1 会导致样条线以通常的速度飞行|
| **DisableAvoidance** | Boolean | no  | false | 飞行时，此样条线将关闭碰撞避让 |

**输出：** 无

* * *

#### Ship\_FlySpline

飞行导航样条线

|     |     |     |     |     |
| --- | --- | --- | --- | --- |
|输入 |类型 |必需 |默认 |描述 |
| **Spline** | EntityId | yes | \-  | 要飞的样条曲线 |
| **StartAtNearest** | Boolean | no  | false | 是否从样条曲线上的最近点开始 |
| **AttackTargets** | Boolean | no  | true | 如果为 true（默认），则对样条指定的目标触发 |
| **AttackUsualTarget** | Boolean | no  | false | 如果为 true，则在飞行样条时面向通常由目标选择器选择的目标并开火。如果指定了样条目标并且 AttackTargets 为 true，则在经过这些特定点时，将覆盖此选项 |
| **SpeedOverride** | Float | no  | \-1.00 | 飞样条线的速度。在吊挂样条曲线时，每帧都会读取此数据。默认值 -1 会导致样条线以通常的速度飞行 |
| **ErrorLimit** | Float | no  | \-1.00 | 实体可以远离样条线的最大距离。如果超过此距离，则覆盖实体的位置。负数表示无限制（默认），否则必须为 >= 1m |
| **TeleportToStart** | Boolean | no  | false | 与其飞到样条的起点，不如立即将飞船传送到那里 |
| **AvoidanceMode** | StringHash | no  | "Normal" | 飞行此样条线时，要执行哪种类型的碰撞避免（选项：Off、Normal、Limited） |
| **FailOnJoinFallback** | Boolean | no  | false | 如果联接回退到使用基于样条的方法，是否使此节点失败。通常，在系统拾取样条时，应将其设置为 true，以避免在系统拾取的样条线的路径与某些对象发生碰撞时发生碰撞 |

**输出：** 无

* * *

#### Ship\_GetSplinePoint

从样条曲线获取点

|     |     |     |     |     |
| --- | --- | --- | --- | --- |
|输入 |类型 |必需 |默认 |描述 |
| **Spline** | EntityId | yes | \-  | 要从中查找的样条 |
| **SplineIndex** | Integer | yes | \-  | 查找哪个点 |

|     |     |     |     |
| --- | --- | --- | --- |
|输出 |类型 |必需 |描述 |
| **PointPos** | Position | yes |样条点的位置 |

* * *

#### Ship\_Goto

直奔目的地。每次更新都会重新评估目标

|     |     |     |     |     |
| --- | --- | --- | --- | --- |
|输入 |类型 |必需 |默认 |描述 |
| **Destination** | EntityId, Position | yes | \-  | 跳转到哪里 |
| **AbsoluteSpeed** | Float | no  | 0.00 | 以 m/s 为单位的移动速度，如果设置为 0，则使用 RelativeSpeed|
| **RelativeSpeed** | Float | no  | 1.00 | 移动速度的最大比例 （0.0 - 1.0） |
| **EndDistance** | Float | no  | 0.00 | 从终点到完成位置的最小距离 |
| **AbsoluteSpeedAtDestination** | Float | no  | 0.00 | 到达目标位置时移动的绝对速度，以 m/s 为单位。默认使用 RelativeSpeedAtDestination。当转到实体时，这将被忽略，而是使用实体的当前速度 |
| **RelativeSpeedAtDestination** | Float | no  | 0.00 | 到达目标位置时移动的相对速度 （0.0 - 1.0）。默认为 stop。当转到实体时，这将被忽略，而是使用实体的当前速度 |
| **LookAtDestination** | Boolean | no  | false | 是否将注视方向设置为注视目标 |
| **LookTarget** | EntityId, Position | no  |     | Explicit look target (optional)显式外观目标（可选） |

**输出：** 无

* * *

#### Ship\_MaintainVel

保持船舶以当前速度行驶

|     |     |     |     |     |
| --- | --- | --- | --- | --- |
|输入 |类型 |必需 |默认 |描述 |
| **Velocity** | Velocity | yes | \-  | 飞船移动的速度（读取每一帧） |
| **Look** | EntityId, Position | no  |     | 船舶指向的方向 |

**输出：** 无

* * *

#### Ship\_PathTo

目标的路径。每次更新都会重新评估目标

|     |     |     |     |     |
| --- | --- | --- | --- | --- |
|输入 |类型 |必需 |默认 |描述 |
| **Destination** | EntityId, Position | yes | \-  | 去哪里/要遵循的路径 |
| **AbsoluteSpeed** | Float | no  | 0.00 | 以 m/s 为单位的移动速度，如果设置为 0，则使用 RelativeSpeed |
| **RelativeSpeed** | Float | no  | 1.00 | 移动速度的最大比例 （0.0 - 1.0） |
| **EndDistance** | Float | no  | 0.00 | 从终点到完成位置的最小距离|
| **AbsoluteSpeedAtDestination** | Float | no  | 0.00 | 到达目标位置时移动的绝对速度，以 m/s 为单位。 默认使用 RelativeSpeedAtDestination。当转到实体时，这将被忽略，而是使用实体的当前速度|
| **RelativeSpeedAtDestination** | Float | no  | 0.00 | 到达目标位置时移动的相对速度 （0.0 - 1.0）。默认为 stop。当转到实体时，这将被忽略，而是使用实体的当前速度 |
| **LookTarget** | EntityId, Position | no  |     | 显式外观目标（可选） |

**输出：** 无

* * *

#### Ship\_Roll

滚动飞船，通常与另一个移动节点并行执行。从未完成

|     |     |     |     |     |
| --- | --- | --- | --- | --- |
|输入 |类型 |必需 |默认 |描述 |
| **RollRate** | Float | yes | \-  | 旋转速率（以 rad/sec 为单位） |

**输出：** 无

* * *

#### Ship\_Stop

使实体完全停止

**输入：** 无

**输出：** 无

* * *

#### Ship\_Track

尝试到达目标实体并保持与目标实体的给定距离。Target 仅在进入时进行评估

|     |     |     |     |     |
| --- | --- | --- | --- | --- |
|输入 |类型 |必需 |默认 |描述 |
| **Target** | EntityId, Position | yes | \-  | 要跟踪的实体或位置 |
| **MinAbsoluteSpeed** | Float | no  | 0.00 | 跟踪时移动的最小速度（以 m/s 为单位），如果设置为 0，则使用 MinRelativeSpeed |
| **MinRelativeSpeed** | Float | no  | 0.00 | 跟踪时移动的最高速度的最小比例 （0.0 - 1.0） |
| **MaxAbsoluteSpeed** | Float | no  | 0.00 | 跟踪时移动的最大速度（以 m/s 为单位），如果设置为 0，则使用 MaxRelativeSpeed |
| **MaxRelativeSpeed** | Float | no  | 1.00 | 跟踪时移动的最高速度的最大比例 （0.0 - 1.0） |
| **Distance** | Float | yes | \-  | 尝试远离目标的距离 |
| **LookAtDestination** | Boolean | no  | false | 是否将注视方向设置为注视目标 |

**输出：** 无

* * *

#### Ship\_TurnToTarget

转动飞船以面对目标。读取目标每帧一次，直到完成

|     |     |     |     |     |
| --- | --- | --- | --- | --- |
|输入 |类型 |必需 |默认 |描述 |
| **Target** | EntityId, Position | yes | \-  | 目标实体或船舶指向的位置 |
| **MaintainDirection** | Boolean | no  | false | 如果为 true，则保持恒定速度并旋转以指向目标。 如果为 false，则保持恒定速度，但将速度方向更改为指向目标 |
| **Tolerance** | Float | no  | 1.00 | 在节点完成之前必须达到的方向的容差（以度为单位） |

**输出：** 无

* * *

### Character movement

#### Character\_ExactGoto

移动到给定的位置和方向

|     |     |     |     |     |
| --- | --- | --- | --- | --- |
|输入 |类型 |必需 |默认 |描述 |
| **Destination** | Position | yes | \-  | 角色应该去哪里 |
| **Direction** | Vector | yes | \-  | 角色在目标位置应面向哪个方向 |
| **Speed** | Float, StringHash | yes | \-  | 角色移动的速度|

**输出：** 无

* * *

#### Character\_Goto

路径查找和跳转目标

|     |     |     |     |     |
| --- | --- | --- | --- | --- |
|输入 |类型 |必需 |默认 |描述 |
| **Destination** | EntityId, Position | yes | \-  | 去哪里 |
| **Speed** | Float, StringHash | yes | \-  | 角色移动的速度 |
| **EndDistance** | Float | no  | 0.00 | 距要完成的路径终点的距离 |

**输出：** 无

* * *

#### Character\_GotoDirectness

以给定的直线接近给定的航点

|     |     |     |     |     |
| --- | --- | --- | --- | --- |
|输入 |类型 |必需 |默认 |描述 |
| **Destination** | EntityId, Position | yes | \-  | 接近的位置 |
| **Speed** | Float, StringHash | yes | \-  | 角色移动的速度 |
| **EndDistance** | Float | no  | 1.00 | 从目的地到完成的距离 |
| **Directness** | Float | no  | 1.00 | 方法的直接性 |

**输出：** 无

* * *

#### Character\_GotoWaypoint

pathfind 添加到给定输入。当接近完成时，重定向到新的航点（如果已给出）

|     |     |     |     |     |
| --- | --- | --- | --- | --- |
|输入 |类型 |必需 |默认 |描述 |
| **Waypoint** | EntityId, Position | yes | \-  | 下一个要去的航点 |
| **Speed** | Float, StringHash | yes | \-  | 角色移动的速度 |
| **EndTolerance** | Float | no  | 1.00 | 在距当前航点多远处读取下一个航点 |

|     |     |     |     |
| --- | --- | --- | --- |
|输出 |类型 |必需 |描述 |
| **DistanceToWaypoint** | Float | yes | 到当前航点的距离 |

* * *

#### Character\_IsPointReachableNow

如果存在从起点/实体到终点/实体的有效路径，则成功，否则失败

|     |     |     |     |     |
| --- | --- | --- | --- | --- |
|输入 |类型 |必需 |默认 |描述 |
| **Start** | EntityId, Position | no  |     | 要从中开始测试的位置/实体。默认值为当前 AI 位置 |
| **End** | EntityId, Position | yes | \-  | 要测试的目标位置/实体 |
| **ClampRadiusXY** | Float | no  | 2.00 | 在 XY 平面中的起点和终点上要钳制的半径 |
| **ClampRadiusZ** | Float | no  | 2.00 | 在 Z 轴上的起点和终点上要夹紧的半径 |
| **MaxPathLength** | Float | no  |     | 要测试的最大路径长度。将默认为从开始位置到结束位置距离的 2 倍 |

**输出：** 无

* * *

#### Character\_PathDistance

获取当前路径的长度。如果没有当前路径，则失败

**输入：** 无

|     |     |     |     |
| --- | --- | --- | --- |
|输出 |类型 |必需 |描述 |
| **Distance** | Float | yes | 距要完成的路径终点的距离 |

* * *

#### Character\_SteeringGoto

让 steering 处理此实体的运动（来自其他输入）

|     |     |     |     |     |
| --- | --- | --- | --- | --- |
|输入 |类型 |必需 |默认 |描述 |
| **Speed** | Float, StringHash | yes | \-  | 角色移动的速度 |

**输出：** 无

* * *

#### Character\_TurnToFace

使角色转向面向某个方向或位置

|     |     |     |     |     |
| --- | --- | --- | --- | --- |
|输入 |类型 |必需 |默认 |描述 |
| **Goal** | Vector, EntityId, Position | yes | \-  | 角色要面对的位置、方向或实体 |
| **DirTolerance** | Float | no  | 5.00 | 目标方向的容差（以度为单位） |
| **TurnRate** | Float | no  | 0.00 | （可选）覆盖字符转动速率（以度/秒为单位）。如果为零（或更小），则忽略 |

**输出：** 无

* * *

### Character control

#### SetStance

设置角色的姿态

|     |     |     |     |     |
| --- | --- | --- | --- | --- |
|输入 |类型 |必需 |默认 |描述 |
| **Stance** | StringHash | no  | "Stand" | 要切换到的姿态 |
| **Strafing** | Boolean | no  | false | 是否允许扫射 |

**输出：** 无

* * *

### Conditional

#### CompareNow

测试 Lua 脚本条件表达式。可以访问实体、个人资料和行为黑板

|     |     |     |     |     |
| --- | --- | --- | --- | --- |
|输入 |类型 |必需 |默认 |描述 |
| **Invert** | Boolean | no  | false | 检查条件是否相反 |
| **Condition** | StringHash | yes | \-  | 要计算的表达式。必须以布尔值计算（例如，它可以有多个表达式，例如 ands 和 ors） |

**输出：** 无

* * *

#### EqualsNow

测试两个输入是否相等 （Lhs == Rhs）

|     |     |     |     |     |
| --- | --- | --- | --- | --- |
|输入 |类型 |必需 |默认 |描述 |
| **Invert** | Boolean | no  | false | 检查条件是否相反 |
| **Lhs** | Any | yes | \-  | 要测试的左手值 |
| **Rhs** | Any | yes | \-  | 要测试的右手值 |

**输出：** 无

* * *

#### GreaterThanEqualsNow

测试一个输入是否大于或等于另一个 (Lhs >= Rhs)

|     |     |     |     |     |
| --- | --- | --- | --- | --- |
|输入 |类型 |必需 |默认 |描述 |
| **Invert** | Boolean | no  | false | 检查条件是否相反 |
| **Lhs** | Integer, Float | yes | \-  | 要测试的左手值 |
| **Rhs** | Integer, Float | yes | \-  | 要测试的右手值 |

**输出：** 无

* * *

#### GreaterThanNow

测试一个输入是否大于另一个 (Lhs > Rhs)

|     |     |     |     |     |
| --- | --- | --- | --- | --- |
|输入 |类型 |必需 |默认 |描述 |
| **Invert** | Boolean | no  | false | 检查条件是否相反 |
| **Lhs** | Integer, Float | yes | \-  | 要测试的左手值 |
| **Rhs** | Integer, Float | yes | \-  | 要测试的右手值 |

**输出：** 无

* * *

#### HasTagNow

检查实体是否具有特定标记

|     |     |     |     |     |
| --- | --- | --- | --- | --- |
|输入 |类型 |必需 |默认 |描述 |
| **Invert** | Boolean | no  | false | 检查条件是否相反 |
| **Tag** | Tag | yes | \-  | 要测试的标签 |
| **EntityId** | EntityId | yes | \-  | 要测试的实体的 ID |

**输出：** 无

* * *

#### HasVariableNow

检查命名变量是否存在

|     |     |     |     |     |
| --- | --- | --- | --- | --- |
|输入 |类型 |必需 |默认 |描述 |
| **Invert** | Boolean | no  | false | 检查条件是否相反 |
| **Name** | StringHash | yes | \-  | 要检查的变量名称（或路径） |

**输出：** 无

* * *

#### IsInFrontNow

如果给定的实体或位置具有点积，且我们的当前方向大于指定的最小值，则为 True。

|     |     |     |     |     |
| --- | --- | --- | --- | --- |
|输入 |类型 |必需 |默认 |描述 |
| **Invert** | Boolean | no  | false | 检查条件是否相反 |
| **Target** | EntityId, Position | yes | \-  | 要测试的实体或位置 |
| **MinDotProduct** | Float | no  | 0.00 | 将目标视为“在前面”的最小点积 |

**输出：** 无

* * *

#### IsInGroupNow

检查实体是否具有特定组的成员身份

|     |     |     |     |     |
| --- | --- | --- | --- | --- |
|输入 |类型 |必需 |默认 |描述 |
| **Invert** | Boolean | no  | false | 检查条件是否相反 |
| **EntityId** | EntityId | yes | \-  | 要测试的实体的 ID |
| **GroupId** | EntityId | yes | \-  | 要测试的实体的 ID |

**输出：** 无

* * *

#### IsInRangeNow

如果两个位置之间的距离在给定范围内，则为 True。

|     |     |     |     |                |
| --- | --- | --- | --- |----------------|
|输入 |类型 |必需 |默认 | 描述             |
| **Invert** | Boolean | no  | false | 检查条件是否相反       |
| **Start** | EntityId, Position | yes | \-  | 要测试的实体或位置      |
| **End** | EntityId, Position | yes | \-  | 要检查距离的实体或位置    |
| **MinRange** | Float | no  | 0.00 | 检查通过的最小距离（可选）  |
| **MaxRange** | Float | no  |     | 检查通过的最大距离（可选）  |

**输出：** 无

* * *

#### IsValidIDNow

如果给定的实体 ID 不为空且实体存在，则为 True

|     |     |     |     |           |
| --- | --- | --- | --- |-----------|
|输入 |类型 |必需 |默认 | 描述        |
| **Invert** | Boolean | no  | false | 检查条件是否相反  |
| **EntityId** | EntityId | yes | \-  | Entity ID |

**输出：** 无

* * *

#### LessThanEqualsNow

测试一个输入是否小于或等于另一个 (Lhs <= Rhs)

|     |     |     |     |     |
| --- | --- | --- | --- | --- |
|输入 |类型 |必需 |默认 |描述 |
| **Invert** | Boolean | no  | false | 检查条件是否相反 |
| **Lhs** | Integer, Float | yes | \-  | 要测试的左手值 |
| **Rhs** | Integer, Float | yes | \-  | 要测试的右手值 |

**输出：** 无

* * *

#### LessThanNow

测试一个输入是否小于另一个 (Lhs < Rhs)

|     |     |     |     |     |
| --- | --- | --- | --- | --- |
|输入 |类型 |必需 |默认 |描述 |
| **Invert** | Boolean | no  | false | 检查条件是否相反 |
| **Lhs** | Integer, Float | yes | \-  | 要测试的左手值 |
| **Rhs** | Integer, Float | yes | \-  | 要测试的右手值 |

**输出：** 无

* * *

#### RandomChanceNow

在给定的概率下为 True

|     |     |     |     |     |
| --- | --- | --- | --- | --- |
|输入 |类型 |必需 |默认 |描述 |
| **Invert** | Boolean | no  | false | 检查条件是否相反 |
| **Probability** | Float | yes | \-  | 成功概率（范围 0-1) |

**输出：** 无

* * *

#### SignalHasParameterNow

如果给定信号包含具有给定名称的参数，则为 True

|     |     |     |     |     |
| --- | --- | --- | --- | --- |
|输入 |类型 |必需 |默认 |描述 |
| **Invert** | Boolean | no  | false | 检查条件是否相反 |
| **Signal** | Blackboard | yes | \-  | 要查询的信号 |
| **ParameterName** | String | yes | \-  | 要查找的参数名称，如果信号包含子黑板，则可能是路径 |

**输出：** 无

* * *

#### TimeGreaterThanNow

测试自时间戳以来是否经过了超过某个时间间隔。如果未设置时间戳，则为 False

|     |     |     |     |     |
| --- | --- | --- | --- | --- |
|输入 |类型 |必需 |默认 |描述 |
| **Invert** | Boolean | no  | false | 检查条件是否相反|
| **Timestamp** | Timestamp | yes | \-  | 事件的时间戳 |
| **Interval** | Float | yes | \-  | 自事件开始以来的最短时间 |

**输出：** 无

* * *

#### TimeLessThanNow

测试自时间戳以来经过的时间间隔是否小于特定时间间隔。如果未设置时间戳，则为 False

|     |     |     |     |     |
| --- | --- | --- | --- | --- |
|输入 |类型 |必需 |默认 |描述 |
| **Invert** | Boolean | no  | false | 检查条件是否相反 |
| **Timestamp** | Timestamp | yes | \-  | 事件的时间戳 |
| **Interval** | Float | yes | \-  | 自事件以来的最长时间 |

**输出：** 无

* * *

### Math

#### Add

将两个输入相加。输入必须是有意义的类型 (int, float, vector)

|     |     |     |     |     |
| --- | --- | --- | --- | --- |
|输入 |类型 |必需 |默认 |描述 |
| **InputA** | Integer, Float, Vector, Position, Velocity | yes | \-  | 任何类型的首次输入 |
| **InputB** | Integer, Float, Vector, Position, Velocity | yes | \-  | 与 InputA 类型相同的第二个输入 |

|     |     |     |     |
| --- | --- | --- | --- |
|输出 |类型 |必需 |描述 |
| **Result** | Any | yes | InputA + InputB |

* * *

#### Divide

将一个输入除以另一个输入。分子输入必须是可整除的类型 （int， float， vector）。分母必须是一个数字 (int, float)

|     |     |     |     |     |
| --- | --- | --- | --- | --- |
|输入 |类型 |必需 |默认 |描述 |
| **InputA** | Integer, Float, Vector | yes | \-  | InputA - 分子输入|
| **InputB** | Integer, Float | yes | \-  | InputB - 分母输入 |

|     |     |     |     |
| --- | --- | --- | --- |
|输出 |类型 |必需 |描述 |
| **Result** | Any | yes | InputA/InputB |

* * *

#### Dot

计算两个向量的点积

|     |     |     |     |     |
| --- | --- | --- | --- | --- |
|输入 |类型 |必需 |默认 |描述 |
| **InputA** | Vector | yes | \-  |第一个向量 |
| **InputB** | Vector | yes | \-  | 第二个向量|

|     |     |     |     |
| --- | --- | --- | --- |
|输出 |类型 |必需 |描述 |
| **Result** | Float | yes | InputA . InputB |

* * *

#### Length

获取向量或速度的长度

|     |     |     |     |     |
| --- | --- | --- | --- | --- |
|输入 |类型 |必需 |默认 |描述 |
| **Vector** | Vector, Velocity | yes | \-  | 向量或速度 |

|     |     |     |     |
| --- | --- | --- | --- |
|输出 |类型 |必需 |描述 |
| **Result** | Float | yes | 向量的长度 |

* * *

#### Multiply

将两个输入相乘。输入的类型必须有意义相乘 (int, float, vector)

|     |     |     |     |     |
| --- | --- | --- | --- | --- |
|输入 |类型 |必需 |默认 |描述 |
| **InputA** | Integer, Float, Vector, Position, Velocity | yes | \-  | 第一个输入数字 |
| **InputB** | Integer, Float, Vector, Position, Velocity | yes | \-  | 第二个输入数字 |

|     |     |     |     |
| --- | --- | --- | --- |
|输出 |类型 |必需 |描述 |
| **Result** | Any | yes | InputA \* InputB |

* * *

#### Normalize

规范化向量。如果向量没有长度，则返回 （0,0,0）

|     |     |     |     |     |
| --- | --- | --- | --- | --- |
|输入 |类型 |必需 |默认 |描述 |
| **Vector** | Vector | yes | \-  | 向量 |

|     |     |     |     |
| --- | --- | --- | --- |
|输出 |类型 |必需 |描述 |
| **Result** | Vector | yes | 与 Vector 方向相同的单位长度向量 |

* * *

#### Subtract

从一个输入中减去另一个输入。输入必须是有意义的减去类型 (int, float, vector)

|     |     |     |     |     |
| --- | --- | --- | --- | --- |
|输入 |类型 |必需 |默认 |描述 |
| **InputA** | Integer, Float, Vector, Position, Velocity | yes | \-  | 任何类型的首次输入|
| **InputB** | Integer, Float, Vector, Position, Velocity | yes | \-  | 与 InputA 类型相同的第二个输入|

|     |     |     |     |
| --- | --- | --- | --- |
|输出 |类型 |必需 |描述 |
| **Result** | Any | yes | InputA - InputB |

* * *

### Search

#### Character\_PredictPosition

预测实体在 x 秒内的位置，假设它们在导航网格上继续以恒定速度移动

|     |     |     |     |     |
| --- | --- | --- | --- | --- |
|输入 |类型 |必需 |默认 |描述 |
| **Entity** | EntityId | yes | \-  | 要预测的实体 |
| **Time** | Float | yes | \-  | 提前多远预测位置 |

|     |     |     |     |
| --- | --- | --- | --- |
|输出 |类型 |必需 |描述 |
| **Position** | Position | yes | 预测位置 |

* * *

#### Character\_RandomPointInRange

在代理的给定导航距离内（或任意位置）查找随机点

|     |     |     |     |     |
| --- | --- | --- | --- | --- |
|输入 |类型 |必需 |默认 |描述 |
| **Range** | Float | yes | \-  | 到返回点的最大路径距离 （近似值） |
| **MinDistance** | Float | no  | 0.00 | 到返回点的最小直线距离 （近似值） |
| **Angle** | Float | no  | 0.00 | 用于限制搜索方向的线段角度（以度为单位）。如果零（或更小）则忽略，如果大于 180，则被钳制 |
| **Direction** | Vector | no  |     | 将搜索方向限制为有关此向量的线段 |
| **Center** | EntityId, Position | no  |     | 将搜索集中在此位置，而不是代理的当前位置 |

|     |     |     |     |
| --- | --- | --- | --- |
|输出 |类型 |必需 |描述 |
| **Point** | Position | yes | 生成的点 |
| **Distance** | Float | no  | 到点的近似非字符串拉取路径距离 |

* * *

#### Character\_RandomPointWithDirectness

找到最接近给定方向的随机点

|     |     |     |     |                                       |
| --- | --- | --- | --- |---------------------------------------|
|输入 |类型 |必需 |默认 | 描述                                    |
| **Destination** | EntityId, Position | yes | \-  | 要计算其直接性的目标位置或实体                       |
| **Directness** | Float | yes | \-  | 要为其生成点的目标方向                           |
| **Range** | Float | yes | \-  | 到返回点的最大路径距离 （近似值）                     |
| **MinDistance** | Float | no  | 0.00 | 到返回点的最小直线距离 （近似值）。默认值 = 0.0f          |
| **DirectionBias** | Vector | no  |     | 得分时的偏差方向 |

|     |     |     |     |
| --- | --- | --- | --- |
|输出 |类型 |必需 |描述 |
| **Point** | Position | yes | 生成的点 |

* * *

#### CountEntitiesWithTags

搜索所有实体并查找与指定标签匹配的实体。返回匹配实体的数组。如果没有实体匹配，则节点失败

|     |     |     |     |     |
| --- | --- | --- | --- | --- |
|输入 |类型 |必需 |默认 |描述 |
| **Tag** | Tag | yes | \-  | 要搜索的标记 |
| **ExtraTag** | Tag | no  | ""  | 可选的 extra 标记（如果指定）也必须存在 |
| **ExcludeId** | EntityId | no  |     | 要排除的可选实体 |
| **Range** | Float | no  | 0.00 | 从实体的当前位置开始搜索的最大距离 |

|     |     |     |     |
| --- | --- | --- | --- |
|输出 |类型 |必需 |描述 |
| **Count** | Integer | yes | 找到的实体数 |

* * *

#### FindEntitiesWithTags

搜索所有实体并查找与指定标签匹配的实体。返回匹配实体的数组。如果没有实体匹配，则节点失败

|     |     |     |     |     |
| --- | --- | --- | --- | --- |
|输入 |类型 |必需 |默认 |描述 |
| **Tags** | StringHash, BlackboardArray | yes | \-  | 要搜索的标记或标记数组 |
| **ExcludeId** | EntityId | no  |     | 要排除的可选实体 |
| **Range** | Float | no  | 0.00 | 从实体的当前位置开始搜索的最大距离 |

|     |     |     |     |
| --- | --- | --- | --- |
|输出 |类型 |必需 |描述 |
| **Results** | BlackboardArray | yes | 所选实体的 ID |

* * *

#### FindNearestEntityWithTags

搜索 Range 中的所有实体，并查找与指定标签匹配的实体。返回集合中最近的实体。如果没有实体匹配，则节点失败

|     |     |     |     |     |
| --- | --- | --- | --- | --- |
|输入 |类型 |必需 |默认 |描述 |
| **Tag** | Tag | yes | \-  | 要搜索的标记 |
| **ExtraTag** | Tag | no  | ""  | 可选的额外标签也可以搜索 |
| **ExcludeId** | EntityId | no  |     | 要排除的可选实体 |
| **Range** | Float | no  | 0.00 | 从实体的当前位置开始搜索的最大距离 |

|     |     |     |     |
| --- | --- | --- | --- |
|输出 |类型 |必需 |描述 |
| **EntityId** | EntityId | yes | 最近实体的 ID|
| **Distance** | Float | no  | 到最近实体的距离 |

* * *

#### FindRandomEntityWithTags

搜索所有实体并查找与指定标签匹配的实体。返回从集合中随机选择的实体。如果没有实体匹配，则节点失败

|     |     |     |     |     |
| --- | --- | --- | --- | --- |
|输入 |类型 |必需 |默认 |描述 |
| **Tag** | Tag | yes | \-  | 要搜索的标记 |
| **ExtraTag** | Tag | no  | ""  | 可选的额外标签也可以搜索 |
| **ExcludeId** | EntityId | no  |     | 要排除的可选实体 |
| **Range** | Float | no  | 0.00 | 要从实体的当前位置搜索的最大距离。值为零表示没有距离限制 |

|     |     |     |     |
| --- | --- | --- | --- |
|输出 |类型 |必需 |描述 |
| **EntityId** | EntityId | yes | 所选实体的 ID|

* * *

#### GetNextNavPoint

查找导航路线上下一个点的 ID。如果没有下一个点，则失败

|     |     |     |     |     |
| --- | --- | --- | --- | --- |
|输入 |类型 |必需 |默认 |描述 |
| **CurrentPoint** | EntityId | yes | \-  | 当前点的 id|

|     |     |     |     |
| --- | --- | --- | --- |
|输出 |类型 |必需 |描述 |
| **NextPoint** | EntityId | yes | 下一个点的 id |

* * *

#### Ship\_RandomPointInRange

在代理的给定导航距离（或任意位置）内查找八叉树中的随机点

|     |     |     |     |     |
| --- | --- | --- | --- | --- |
|输入 |类型 |必需 |默认 |描述 |
| **Range** | Float | yes | \-  | 到返回点的最大路径距离 （近似值） |
| **MinDistance** | Float | no  | 0.00 | 到返回点的最小直线距离 （近似值） |
| **Center** | EntityId, Position | no  |     | 将搜索集中在此位置，而不是代理的当前位置 |
| **ClampStartPoint** | Boolean | no  | false | 将原点搜索限制为可导航的八叉树 |

|     |     |     |     |
| --- | --- | --- | --- |
|输出 |类型 |必需 |描述 |
| **Point** | Position | yes | 生成的点 |

* * *

### Utility

#### AddTag

向实体添加标签

|     |     |     |     |     |
| --- | --- | --- | --- | --- |
|输入 |类型 |必需 |默认 |描述 |
| **Tag** | Tag | yes | \-  | 要添加的标签 |
| **EntityId** | EntityId | yes | \-  | 要将标签添加到的实体的 ID |

**输出：** 无

* * *

#### ArrayBreak

跳出当前迭代数组循环，另请参阅 BTIterateOverArray

|     |     |     |     |     |
| --- | --- | --- | --- | --- |
|输入 |类型 |必需 |默认 |描述 |
| **BreakResult** | StringHash | yes | \-  | 判断此断点是成功还是失败 |

**输出：** 无

* * *

#### Character\_AdjustSpeedToTargetDist

根据与目标的距离计算速度

|     |     |     |     |     |
| --- | --- | --- | --- | --- |
|输入 |类型 |必需 |默认 |描述 |
| **Start** | EntityId, Position | no  |     | 可选 position 或 id 来计算距离，默认为 self |
| **End** | EntityId, Position | yes | \-  | 要计算距离的位置或 id |
| **FarSpeed** | Float, StringHash | no  |     | 速度的可选名称或以 m/s 为单位的速度值，用于最远的距离，默认为当前速度 |
| **FarDistance** | Float | yes | \-  | 开始向 CloseSpeed 进行插值速度的距离 |
| **CloseSpeed** | Float, StringHash | no  |     | 在近距离内使用的速度可选名称或以 m/s 为单位的速度值，默认为当前速度 |
| **CloseDistance** | Float | yes | \-  | 完成朝 CloseSpeed 的插值速度的距离 |

|     |     |     |     |
| --- | --- | --- | --- |
|输出 |类型 |必需 |描述 |
| **Result** | Float | yes | 计算出的速度值 |

* * *

#### Character\_Speed

将命名速度（例如 'Walk'、'Run'）转换为此实体的数值，并应用可选的百分比修饰符

|     |     |     |     |     |
| --- | --- | --- | --- | --- |
|输入 |类型 |必需 |默认 |描述 |
| **Speed** | Float, StringHash | no  |     | 速度的可选名称或以 m/s 为单位的速度值，默认为当前速度 |
| **Stance** | StringHash | no  |     | 要映射的可选姿态，默认为当前姿态 |
| **Multiplier** | Float | no  | 1.00 | 应用于速度的可选乘数（以简化计算，例如比步行速度快 10%） |

|     |     |     |     |
| --- | --- | --- | --- |
|输出 |类型 |必需 |描述 |
| **Result** | Float | yes | 计算出的速度值 |

* * *

#### ClaimEntity

声明实体的所有权并删除其“avaliable”标签

|     |     |     |     |     |
| --- | --- | --- | --- | --- |
|输入 |类型 |必需 |默认 |描述 |
| **EntityId** | EntityId | yes | \-  | 要声明的实体的 ID |
| **ObjectSlot** | StringHash | yes | \-  | 实体所有权黑板上存储对象 ID 的位置。每个 API 中只能存储一个对象 |

**输出：** 无

* * *

#### ClearTimestampVariable

清除时间戳的值，以便任何大于或小于比较的值将始终返回 false

|     |     |     |     |     |
| --- | --- | --- | --- | --- |
|输入 |类型 |必需 |默认 |描述 |
| **Name** | StringHash | yes | \-  | 要清除的时间戳变量的名称 |

**输出：** 无

* * *

#### Compute

评估 Lua 脚本条件表达式。可以访问 Entity、Profile 和 Behavior 黑板

|     |     |     |     |     |
| --- | --- | --- | --- | --- |
|输入 |类型 |必需 |默认 |描述 |
| **Expression** | StringHash | yes | \-  | 要计算的表达式 |

|     |     |     |     |
| --- | --- | --- | --- |
|输出 |类型 |必需 |描述 |
| **Result** | Any | yes | 表达式的结果 |

* * *

#### Copy

将值（常量或变量）复制到变量

|     |     |     |     |    |
| --- | --- | --- | --- |----|
|输入 |类型 |必需 |默认 | 描述 |
| **Input** | Any | yes | \-  | 值  |

|     |     |     |     |
| --- | --- | --- | --- |
|输出 |类型 |必需 |描述 |
| **Output** | Any | yes | 值的副本 |

* * *

#### DistanceBetweenPoints

获取两点之间的距离，其中两点都可以指定为 KytPos 或实体 ID

|     |     |     |     |     |
| --- | --- | --- | --- | --- |
|输入 |类型 |必需 |默认 |描述 |
| **Point1** | EntityId, Position | yes | \-  | 第一个位置或 id |
| **Point2** | EntityId, Position | yes | \-  | 第二个位置或 id |

|     |     |     |     |
| --- | --- | --- | --- |
|输出 |类型 |必需 |描述 |
| **Distance** | Float | yes | 点之间的距离 |

* * *

#### DistanceToBoundsEdge

获取实体到边界对象最近边缘的距离，如果超出边界，则为负数

|     |     |     |     |     |
| --- | --- | --- | --- | --- |
|输入 |类型 |必需 |默认 |描述 |
| **BoundsId** | EntityId | yes | \-  | bounds 对象的 id |

|     |     |     |     |
| --- | --- | --- | --- |
|输出 |类型 |必需 |描述 |
| **Distance** | Float | yes | 到边界边缘的距离，如果超出边界，则为负数|

* * *

#### EraseTag

从实体中删除标记

|     |     |     |     |     |
| --- | --- | --- | --- | --- |
|输入 |类型 |必需 |默认 |描述 |
| **Tag** | Tag | yes | \-  | 要擦除的标记 |
| **EntityId** | EntityId | yes | \-  | 要从中擦除标签的实体的 ID |
| **IncludeChildren** | Boolean | no  | false | 是否同时擦除此标签的子项 |

**输出：** 无

* * *

#### EraseVariable

擦除指定的变量

|     |     |     |     |     |
| --- | --- | --- | --- | --- |
|输入 |类型 |必需 |默认 |描述 |
| **Name** | StringHash | yes | \-  | 要擦除的变量的名称 |

**输出：** 无

* * *

#### Execute

执行 Lua 脚本表达式。可以访问 Entity、Profile 和 Behavior 黑板

|     |     |     |     |     |
| --- | --- | --- | --- | --- |
|输入 |类型 |必需 |默认 |描述 |
| **Expression** | StringHash | yes | \-  | 要执行的表达式 |

**输出：** 无

* * *

#### Fail

什么都不做;首次更新时返回失败

**输入：** 无

**输出：** 无

* * *

#### GenerateRandom2dDirection

在 x-y 平面上向前方向的 +/- 度内生成随机 2d 方向

|     |     |     |     |     |
| --- | --- | --- | --- | --- |
|输入 |类型 |必需 |默认 |描述 |
| **Forward** | Vector | yes | \-  | 要生成的正向 |
| **Angle** | Float | no  | 180.00 | 向前方向两侧的最大允许角度（以度为单位）（允许的范围 0 - 180） |

|     |     |     |     |
| --- | --- | --- | --- |
|输出 |类型 |必需 |描述 |
| **Direction** | Vector | yes | 生成的随机方向 |

* * *

#### GenerateRandomDirectionOnPlane

在给定平面法线的平面上生成随机方向

|     |     |     |     |     |
| --- | --- | --- | --- | --- |
|输入 |类型 |必需 |默认 |描述 |
| **Normal** | Vector | yes | \-  | 平面法线 |

|     |     |     |     |
| --- | --- | --- | --- |
|输出 |类型 |必需 |描述 |
| **Direction** | Vector | yes | 生成的随机方向 |

* * *

#### GenerateRandomFloat

生成从 Min 到 Max 的随机浮点值，可选择缩放

|     |     |     |     |     |
| --- | --- | --- | --- | --- |
|输入 |类型 |必需 |默认 |描述 |
| **Min** | Float | yes | \-  | 要生成的最小值（含）值 |
| **Max** | Float | yes | \-  | 要生成的最大值（含）值 |
| **Scale** | Float | no  | 1.00 | 缩放随机生成的整数的值，例如 MinVal = 1.0、MaxVal = 4.0、Scale = 2.0：返回值是从 2.0 到 8.0 的数字 |

|     |     |     |     |
| --- | --- | --- | --- |
|输出 |类型 |必需 |描述 |
| **Result** | Float | yes | 生成的随机数 |

* * *

#### GenerateRandomInt

生成一个从 Min 到 Max 的随机整数值，可选择缩放

|     |     |     |     |     |
| --- | --- | --- | --- | --- |
|输入 |类型 |必需 |默认 |描述 |
| **Min** | Integer | yes | \-  | 要生成的最小值（含）值 |
| **Max** | Integer | yes | \-  | 要生成的最大值（含）值 |
| **Scale** | Integer | no  | 1   | 缩放随机生成的整数的值，例如 MinVal = 1、MaxVal = 4、Scale = 2：返回值是从 2 到 8 的数字 |

|     |     |     |         |
| --- | --- | --- |---------|
|输出 |类型 |必需 | 描述      |
| **Result** | Integer | yes | 生成的随机数  |

* * *

#### GenerateRandomPosition

在每个轴的 Min 和 Max 约束范围内，从参考位置生成随机位置偏移

|     |     |     |     |     |
| --- | --- | --- | --- | --- |
|输入 |类型 |必需 |默认 |描述 |
| **MinX** | Float | no  | 0.00 | 要在 X 轴上生成的最小值（含） |
| **MaxX** | Float | no  | 0.00 | 要在 X 轴上生成的最大值（含） |
| **MinY** | Float | no  | 0.00 | 要在 Y 轴上生成的最小值（含） |
| **MaxY** | Float | no  | 0.00 | 要在 Y 轴上生成的最大值（含） |
| **MinZ** | Float | no  | 0.00 | 要在 Z 轴上生成的最小值（含） |
| **MaxZ** | Float | no  | 0.00 | 要在 Z 轴上生成的最大值（含） |
| **ReferencePos** | Position | yes | \-  | 生成随机偏移量的位置 |

|     |     |     |     |
| --- | --- | --- | --- |
|输出 |类型 |必需 |描述 |
| **Result** | Position | yes | 生成的随机位置 |

* * *

#### GetArraySize

获取实体的 sate 树并访问其黑板。 \[WARNING\] 不要尝试更改此节点接收到的黑板的值

|     |     |     |     |     |
| --- | --- | --- | --- | --- |
|输入 |类型 |必需 |默认 |描述 |
| **Array** | BlackboardArray | yes | \-  | 要返回大小的数组 |

|     |     |     |     |
| --- | --- | --- | --- |
|输出 |类型 |必需 |描述 |
| **ArraySize** | Integer | yes | 给定数组的大小 |

* * *

#### GetDirection

获取两点之间的标准化方向向量，其中任一点都可以指定为 KytPos 或实体 ID

|     |     |     |     |     |
| --- | --- | --- | --- | --- |
|输入 |类型 |必需 |默认 |描述 |
| **Start** | EntityId, Position | yes | \-  | 第一个位置或 id |
| **End** | EntityId, Position | yes | \-  | 第二个位置或 id |

|     |     |     |     |
| --- | --- | --- | --- |
|输出 |类型 |必需 |描述 |
| **Result** | Vector | yes | 单位长度向量 |

* * *

#### GetEntityDirection

获取实体指定局部轴在世界空间中的方向

|     |     |     |     |     |
| --- | --- | --- | --- | --- |
|输入 |类型 |必需 |默认 |描述 |
| **EntityId** | EntityId | yes | \-  | 实体的 ID |
| **Direction** | StringHash | yes | \-  | 所需本地方向的名称 |

|     |     |     |     |
| --- | --- | --- | --- |
|输出 |类型 |必需 |描述 |
| **WorldDirection** | Vector | yes | 世界空间中相关方向的单位向量 |

* * *

#### GetEntityPos

获取实体的位置

|     |     |     |     |     |
| --- | --- | --- | --- | --- |
|输入 |类型 |必需 |默认 |描述 |
| **EntityId** | EntityId | yes | \-  | 实体的 ID |

|     |     |     |     |
| --- | --- | --- | --- |
|输出 |类型 |必需 |描述 |
| **Position** | Position | yes | 实体的位置 |

* * *

#### GetEntityStateTree

获取实体的 sate 树并访问其黑板。\[WARNING\] 请勿尝试更改此节点接收到的黑板的值

|     |     |     |     |     |
| --- | --- | --- | --- | --- |
|输入 |类型 |必需 |默认 |描述 |
| **EntityId** | EntityId | yes | \-  | 实体的 ID |

|     |     |     |     |
| --- | --- | --- | --- |
|输出 |类型 |必需 |描述 |
| **StateTree** | Blackboard | yes | 返回 Entity State Tree，以访问其黑板 |

* * *

#### GetEntityTargetBlackboard

返回指向实体的目标黑板的指针。如果不存在 blackboard，则失败

|     |     |     |     |     |
| --- | --- | --- | --- | --- |
|输入 |类型 |必需 |默认 |描述 |
| **EntityId** | EntityId | yes | \-  | 要从中获取黑板的实体的 ID |

|     |     |     |     |
| --- | --- | --- | --- |
|输出 |类型 |必需 |描述 |
| **TargetBB** | Blackboard | yes | 将黑板定位到输入实体 |

* * *

#### GetSignalParameter

获取 signal 参数的值

|     |     |     |     |     |
| --- | --- | --- | --- | --- |
|输入 |类型 |必需 |默认 |描述 |
| **Signal** | Blackboard | yes | \-  | 要查询的信号 |
| **ParameterName** | StringHash | yes | \-  | 要从 signal 中检索的参数名称，如果 signal 包含子黑板，则可能是 path |

|     |     |     |     |
| --- | --- | --- | --- |
|输出 |类型 |必需 |描述 |
| **Value** | Any | yes | 参数包含的值 |

* * *

#### InitializeVariable

创建初始化为默认值的行为变量

|     |     |     |     |     |
| --- | --- | --- | --- | --- |
|输入 |类型 |必需 |默认 |描述 |
| **Name** | StringHash | yes | \-  | 要创建的变量名称 |
| **Type** | StringHash | yes | \-  | 行为变量的类型 |

**输出：** 无

* * *

#### Log

将消息写入日志

|     |     |     |     |     |
| --- | --- | --- | --- | --- |
|输入 |类型 |必需 |默认 |描述 |
| **Message** | String | yes | \-  | 要写入日志的消息 |
| **Level** | StringHash | no  | "Normal" | 消息的严重性 |

**输出：** 无

* * *

#### Noop

在中断之前不执行任何操作

**输入：** 无

**输出：** 无

* * *

#### OverrideEntityPhysics

覆盖给定帧的实体的物理特性

|     |     |     |     |     |
| --- | --- | --- | --- | --- |
|输入 |类型 |必需 |默认 |描述 |
| **EntityId** | EntityId | yes | \-  | 要覆盖的实体 |
| **Position** | Position | no  |     | 覆盖实体的位置（可选） |
| **Velocity** | Vector, Velocity | no  |     | 覆盖实体的速度 （可选） |

**输出：** 无

* * *

#### PersonalLog

将消息写入实体的个人日志

|     |     |     |     |     |
| --- | --- | --- | --- | --- |
|输入 |类型 |必需 |默认 |描述 |
| **Message** | String | yes | \-  | 要写入个人日志的消息 |

**输出：** 无

* * *

#### PopArrayValue

从数组的末尾弹出最后一个值

|     |     |     |     |     |
| --- | --- | --- | --- | --- |
|输入 |类型 |必需 |默认 |描述 |
| **Array** | BlackboardArray | yes | \-  | 要从中弹出值的数组 |
| **Method** | StringHash | no  | "Last" | 要从数组中弹出的值 |

|     |     |     |     |
| --- | --- | --- | --- |
|输出 |类型 |必需 |描述 |
| **Value** | Any | yes | 从数组中弹出的值 |

* * *

#### PushArrayValue

从数组的末尾弹出最后一个值

|     |     |     |     |     |
| --- | --- | --- | --- | --- |
|输入 |类型 |必需 |默认 |描述 |
| **Array** | StringHash | yes | \-  | 将数组 push 值推送给 |
| **Value** | Any | yes | \-  | 要推送到数组中的值 |

**输出：** 无

* * *

#### Raycast

执行光线投射，如果未命中任何内容，则成功

|     |     |     |     |     |
| --- | --- | --- | --- | --- |
|输入 |类型 |必需 |默认 |描述 |
| **From** | EntityId, Position | yes | \-  | 要从中进行光线投射的实体或位置 |
| **To** | EntityId, Position | yes | \-  | 要光线投射到的实体或位置 |

|     |     |     |     |
| --- | --- | --- | --- |
|输出 |类型 |必需 |描述 |
| **HitPos** | Position | no  | 命中位置（仅在节点失败时有效） |

* * *

#### ReleaseEntity

释放对象的所有权并添加 'Available' 标签

|     |     |     |     |     |
| --- | --- | --- | --- | --- |
|输入 |类型 |必需 |默认 |描述 |
| **ObjectSlot** | StringHash | yes | \-  | 对象在实体所有权 Blackboard 上的存储位置 |

**输出：** 无

* * *

#### ReplaceTag

将实体上的一个标签替换为另一个标签

|     |     |     |     |     |
| --- | --- | --- | --- | --- |
|输入 |类型 |必需 |默认 |描述 |
| **OldTag** | Tag | yes | \-  | 要删除的标签 |
| **NewTag** | Tag | yes | \-  | 要添加的标签 |
| **EntityId** | EntityId | yes | \-  | 要更改标记的实体的 ID |

**输出：** 无

* * *

#### SendResponseSignal

向行为树处理的信号发送响应

|     |     |     |     |     |
| --- | --- | --- | --- | --- |
|输入 |类型 |必需 |默认 |描述 |
| **Signal** | Blackboard | yes | \-  | 要响应的信号 |
| **Result** | StringHash | yes | \-  | 要返回的结果，例如 Success 或 Failed |

**输出：** 无

* * *

#### SendSignal

发送信号

|     |     |     |     |     |
| --- | --- | --- | --- | --- |
|输入 |类型 |必需 |默认 |描述 |
| **Name** | StringHash | yes | \-  | 要发送的信号的名称 |
| **Target** | EntityId | no  |     | 要向其发送信号的实体的 ID（默认为 self） |
| **Key1** | StringHash | no  | ""  | 信号上第一个数据参数的可选键 |
| **Value1** | Any | no  |     | 第一个数据参数的可选值 |
| **Key2** | StringHash | no  | ""  | 信号上第二个数据参数的可选键 |
| **Value2** | Any | no  |     | 第二个 data 参数的可选值 |

**输出：** 无

* * *

#### SendSignalToGroup

向组中的所有实体发送信号

|     |     |     |     |     |
| --- | --- | --- | --- | --- |
|输入 |类型 |必需 |默认 |描述 |
| **Name** | StringHash | yes | \-  | 要发送的信号的名称 |
| **Category** | StringHash | yes | \-  | 要将信号发送到的组的类别 |
| **Key1** | StringHash | no  | ""  | 信号上第一个数据参数的可选键 |
| **Value1** | Any | no  |     | 第一个数据参数的可选值 |
| **Key2** | StringHash | no  | ""  | 信号上第二个数据参数的可选键|
| **Value2** | Any | no  |     | 第二个 data 参数的可选值 |

**输出：** 无

* * *

#### SetBranchTag

删除给定分支中的所有标签并添加新标签

|     |     |     |     |     |
| --- | --- | --- | --- | --- |
|输入 |类型 |必需 |默认 |描述 |
| **BranchTag** | Tag | yes | \-  | 要修剪的 branch 标签 |
| **Tag** | Tag | yes | \-  | 要添加的标签 |
| **EntityId** | EntityId | yes | \-  | 要更改标签的实体的 ID |

**输出：** 无

* * *

#### SetTimestampVariable

将 timestamp 的值设置为 now，如果尚不存在，则创建

|     |     |     |     |     |
| --- | --- | --- | --- | --- |
|输入 |类型 |必需 |默认 |描述 |
| **Name** | StringHash | yes | \-  | 要设置为 now 的 timestamp 变量的名称 |

**输出：** 无

* * *

#### SetVariable

将变量设置为特定值

|     |     |     |     |     |
| --- | --- | --- | --- | --- |
|输入 |类型 |必需 |默认 |描述 |
| **Name** | StringHash | yes | \-  | 要将值写入的变量名称（或路径） |
| **Value** | Any | yes | \-  | 要分配给变量的值 |

**输出：** 无

* * *

#### Ship\_NavRaycast

测试两个位置之间是否有一条直线可导航

|     |     |     |     |     |
| --- | --- | --- | --- | --- |
|输入 |类型 |必需 |默认 |描述 |
| **From** | EntityId, Position | yes | \-  | 要从中进行光线投射的实体或位置|
| **To** | EntityId, Position | yes | \-  | 要光线投射到的实体或位置|

|     |     |     |     |
| --- | --- | --- | --- |
|输出 |类型 |必需 |描述 |
| **HitPos** | Position | yes | 命中位置（仅在节点失败时有效） |

* * *

#### SmoothSpeedToTargetDist

根据与目标的距离计算速度，在帧上平滑以减少突然的变化

|     |     |     |     |     |
| --- | --- | --- | --- | --- |
|输入 |类型 |必需 |默认 |描述 |
| **Start** | EntityId, Position | no  |     | 可选 position 或 id 来计算距离，默认为 self |
| **End** | EntityId, Position | yes | \-  | 要计算距离的位置或 id |
| **FarSpeed** | Float, StringHash | no  |     | 速度的可选名称或以 m/s 为单位的速度值，用于最远的距离，默认为当前速度 |
| **FarDistance** | Float | yes | \-  | 开始向 CloseSpeed 进行插值速度的距离 |
| **CloseSpeed** | Float, StringHash | no  |     | 在近距离内使用的速度可选名称或以 m/s 为单位的速度值，默认为当前速度 |
| **CloseDistance** | Float | yes | \-  | 完成朝 CloseSpeed 的插值速度的距离 |
| **LowSpeedClamp** | Float, StringHash | no  | 0.00 | 速度的可选名称或以 m/s 为单位的速度值，低于该值时不要去 |
| **SmoothingDecayRate** | Float | no  | 0.10 | 平滑衰减，值越高越平滑，例如 0.1 在 1 秒内向目标值移动 90% |
| **LastResult** | Float | yes | \-  | 上次运行中计算的速度或初始速度值 |
| **LastUpdate** | Timestamp | yes | \-  | 上次运行该节点的时间戳，或现在用于首次运行 |

|     |     |     |     |
| --- | --- | --- | --- |
|输出 |类型 |必需 |描述 |
| **SmoothedSpeed** | Float | yes | 计算出的速度值 |
| **UpdateTimestamp** | Timestamp | yes | 此更新的时间戳（以便下次传回） |

* * *

#### Success

什么都不做;首次更新时返回成功

**输入：** 无

**输出：** 无

* * *

#### Wait

等待指定时间，不执行任何操作

|     |     |     |     |     |
| --- | --- | --- | --- | --- |
|输入 |类型 |必需 |默认 |描述 |
| **WaitTime** | Float | no  | 0.00 | 秒等待，零秒永远等待 |

**输出：** 无

* * *

#### WaitForSignal

侦听指定名称的信号，然后保留信号的副本

|     |     |     |     |     |
| --- | --- | --- | --- | --- |
|输入 |类型 |必需 |默认 |描述 |
| **Name** | StringHash | yes | \-  | 要侦听的信号类型的名称 |
| **FilterKey1** | StringHash | no  | ""  | 可选过滤器，用于仅侦听包含此键的信号|
| **FilterValue1** | Any | no  |     | 可选筛选器，用于仅侦听 FilterKey 字段中具有此值的信号 |
| **FilterKey2** | StringHash | no  | ""  | 可选过滤器，用于仅侦听包含此键的信号 |
| **FilterValue2** | Any | no  |     | 可选筛选器，用于仅侦听 FilterKey 字段中具有此值的信号 |

|     |     |     |     |
| --- | --- | --- | --- |
|输出 |类型 |必需 |描述 |
| **Signal** | Blackboard | no  | A copy of the signal received |

* * *

#### WaitRandom

等待最小和最大指定值之间的随机时间长度，然后不执行任何操作

|     |     |     |     |     |
| --- | --- | --- | --- | --- |
|输入 |类型 |必需 |默认 |描述 |
| **MinWaitTime** | Float | yes | \-  | 等待的最小秒数|
| **MaxWaitTime** | Float | yes | \-  | 等待的最大秒数 |

**输出：** 无

* * *

### Groups

#### AddEntityToGroup

Add an entity to a group

|     |     |     |     |     |
| --- | --- | --- | --- | --- |
|输入 |类型 |必需 |默认 |描述 |
| **GroupId** | EntityId | yes | \-  | 要将实体添加到的组的 ID |
| **EntityId** | EntityId | yes | \-  | 要添加的实体的 ID |
| **IsLeader** | Boolean | no  | false | 此节点是否被指定为组的领导者。如果此实体死亡，则该组织将被解散 |

|     |     |     |     |
| --- | --- | --- | --- |
|输出 |类型 |必需 |描述 |
| **Count** | Integer | yes | 添加实体后组中的实体数 |

* * *

#### CreateGroup

创建新的组实体

|     |     |     |     |     |
| --- | --- | --- | --- | --- |
|输入 |类型 |必需 |默认 |描述 |
| **Type** | StringHash | yes | \-  | 组的类型 |

|     |     |     |     |
| --- | --- | --- | --- |
|输出 |类型 |必需 |描述 |
| **GroupId** | EntityId | yes | 已创建的组实体的 ID |

* * *

#### EraseGroup

擦除现有组实体

|     |     |     |     |     |
| --- | --- | --- | --- | --- |
|输入 |类型 |必需 |默认 |描述 |
| **EntityId** | EntityId | yes | \-  | 要擦除的实体的 ID |

**输出：** 无

* * *

#### GetEntityInGroup

获取组中的第 n 个实体

|     |     |     |     |     |
| --- | --- | --- | --- | --- |
|输入 |类型 |必需 |默认 |描述 |
| **GroupId** | EntityId | yes | \-  | 实体所属组的 ID |
| **Index** | Integer | yes | \-  | 要从组中获取的实体的从 0 开始的索引 |

|     |     |     |     |
| --- | --- | --- | --- |
|输出 |类型 |必需 |描述 |
| **EntityId** | EntityId | yes | 实体的 ID |

* * *

#### GetGroupCount

获取组中的实体计数

|     |     |     |     |     |
| --- | --- | --- | --- | --- |
|输入 |类型 |必需 |默认 |描述 |
| **GroupId** | EntityId | yes | \-  | 要将实体添加到的组的 ID |

|     |     |     |     |
| --- | --- | --- | --- |
|输出 |类型 |必需 |描述 |
| **Count** | Integer | yes | 添加实体后组中的实体数 |

* * *

#### GetGroupFromEntity

获取实体所属的组

|     |     |     |     |     |
| --- | --- | --- | --- | --- |
|输入 |类型 |必需 |默认 |描述 |
| **EntityId** | EntityId | yes | \-  | 要获取其组的实体的 ID |
| **Type** | StringHash | yes | \-  | 要检查其成员身份的组的类型 |

|     |     |     |     |
| --- | --- | --- | --- |
|输出 |类型 |必需 |描述 |
| **GroupId** | EntityId | yes | 实体所属组的 ID |

* * *

#### RemoveEntityFromGroup

从组中删除实体

|     |     |     |     |     |
| --- | --- | --- | --- | --- |
|输入 |类型 |必需 |默认 |描述 |
| **GroupId** | EntityId | yes | \-  | 要将实体添加到的组的 ID |
| **EntityId** | EntityId | yes | \-  | 要添加的实体的 ID |

|     |     |     |     |
| --- | --- | --- | --- |
|输出 |类型 |必需 |描述 |
| **Count** | Integer | yes | 删除实体后组中的实体数 |

* * *

### Exception handling

#### ThrowException

引发异常

|     |     |     |     |     |
| --- | --- | --- | --- | --- |
|输入 |类型 |必需 |默认 |描述 |
| **Type** | StringHash | yes | \-  | 要引发的异常类型 |

**输出：** 无

* * *

### Debug

#### DD\_DrawLine

在两点之间画一条线

|     |     |     |     |     |
| --- | --- | --- | --- | --- |
|输入 |类型 |必需 |默认 |描述 |
| **Point1** | EntityId, Position | yes | \-  | 第一个位置或 id |
| **Point2** | EntityId, Position | yes | \-  | 第二个位置或 id |
| **Color** | StringHash | no  | "White" | 线条的颜色|

**输出：** 无

* * *

#### DD\_DrawSphere

在半径为的点处绘制球体

|     |     |     |     |     |
| --- | --- | --- | --- | --- |
|输入 |类型 |必需 |默认 |描述 |
| **Center** | EntityId, Position | yes | \-  | 中心位置 |
| **Radius** | Float | yes | \-  | 球体半径 |
| **Color** | StringHash | no  | "White" | 线条的颜色 |

**输出：** 无

* * *

#### HDV2D\_DrawRandomPoints

在导航网格上生成和渲染随机点

|     |     |     |     |     |
| --- | --- | --- | --- | --- |
|输入 |类型 |必需 |默认 |描述 |
| **NumPoints** | Integer | yes | \-  | 要生成的点数（最大 10000） |
| **MinRadius** | Float | yes | \-  | 允许最小距离点到中心 |
| **MaxRadius** | Float | yes | \-  | 允许点到中心的最大距离 |

**输出：** 无

* * *

### SQS

#### SpatialQuerySimple

使用最简单的设置运行 SQS 查询

|     |     |     |     |     |
| --- | --- | --- | --- | --- |
|输入 |类型 |必需 |默认 |描述 |
| **Query** | StringHash | yes | \-  | 要处理的 SQS 查询的名称 |
| **Origin** | EntityId, Position | yes | \-  | 查询源的实体或位置 |
| **Reference** | EntityId, Position | no  |     | 查询引用的实体或位置 |

|     |     |     |     |
| --- | --- | --- | --- |
|输出 |类型 |必需 |描述 |
| **SQSPoint** | Position | yes | 查询的顶点位置 |
| **SQSPointId** | EntityId | no  | 查询的顶端 ID |
| **SQSResult** | Blackboard | no  | 包含有关结果的更多数据的黑板 |

* * *

### State Machine

#### SendTransitionSignal

向此实体发送信号，以在父 State Machine 中引起状态转换。如果状态机在此节点发送信号后没有从当前状态转换出来，则此节点将引发错误

|     |     |     |     |     |
| --- | --- | --- | --- | --- |
|输入 |类型 |必需 |默认 |描述 |
| **Name** | StringHash | yes | \-  | 转换信号的名称|

**输出：** 无

* * *

## Composite Nodes

具有一个或多个子项的节点

### Basic

#### IfThenElse

在进入时测试 Lua 条件，如果 true 执行其第一个子项，如果 false 执行第二个子项（如果存在），否则失败

|     |     |     |     |     |
| --- | --- | --- | --- | --- |
|输入 |类型 |必需 |默认 |描述 |
| **Condition** | StringHash | yes | \-  | 要计算的 Lua 表达式必须计算为布尔值（例如，它可以有多个包含 ands 和 ors 的测试） |

**输出：** 无

* * *

#### ParallelUntilAllComplete

同时运行所有子项，所有子项都已完成。如果任何失败，则返回失败

**输入：** 无

**输出：** 无

* * *

#### ParallelUntilAnyComplete

同时运行所有子项，直到一个子项完成

**输入：** 无

**输出：** 无

* * *

#### ParallelUntilFailure

同时运行所有子项，直到一个子项失败

**输入：** 无

**输出：** 无

* * *

#### Selector

一个接一个地运行子项，直到一个子项成功或全部失败

**输入：** 无

**输出：** 无

* * *

#### Sequence

连续运行的节点序列。一旦任何子项失败，就会停止并失败

**输入：** 无

**输出：** 无

* * *

### State Machine

#### StateMachine

具有由 signals 控制的 transitions 的状态机。如果子状态运行完成，则此节点完成

**输入：** 无

**输出：** 无

* * *

### Priority

#### Priority

控制一组有序的子项，每个子项都有一个布尔条件，用于是否执行。将持续评估并执行具有 true 条件的第一个子项

**输入：** 无

**输出：** 无

* * *

## Decorator Nodes

只有一个子节点

### Flow control

#### RepeatUntilFails

不断重复子节点，直到它失败

|     |     |     |     |     |
| --- | --- | --- | --- | --- |
|输入 |类型 |必需 |默认 |描述 |
| **Iterations** | Integer | no  | 0   | 要运行的迭代次数（默认为无限） |

**输出：** 无

* * *

#### RepeatUntilSucceeds

不断重复子节点，直到成功

|     |     |     |     |     |
| --- | --- | --- | --- | --- |
|输入 |类型 |必需 |默认 |描述 |
| **Iterations** | Integer | no  | 0   | 要运行的迭代次数（默认为无限） |

**输出：** 无

* * *

#### Repeater

无论结果如何，都不断重复子节点

|     |     |     |     |     |
| --- | --- | --- | --- | --- |
|输入 |类型 |必需 |默认 |描述 |
| **Iterations** | Integer | no  | 0   | 要运行的迭代次数（默认为无限） |

**输出：** 无

* * *

#### Timer

运行子节点最多指定时间

|     |     |     |     |     |
| --- | --- | --- | --- | --- |
|输入 |类型 |必需 |默认 |描述 |
| **RunTime** | Float | yes | \-  | 运行秒数 |
| **FailOnTimeout** | Boolean | no  | false | 如果计时器在子节点完成之前过期，节点是否应该失败 |

**输出：** 无

* * *

### Return code manipulation

#### Failer

总是失败

**输入：** 无

**输出：** 无

* * *

#### Inverter

反转子节点的返回值

**输入：** 无

**输出：** 无

* * *

#### Succeeder

始终成功，除非有异常

**输入：** 无

**输出：** 无

* * *

### Exception handling

#### HandleException

通过中断树，然后失败或成功来处理异常

|     |     |     |     |     |
| --- | --- | --- | --- | --- |
|输入 |类型 |必需 |默认 |描述 |
| **Result** | StringHash | no  | "Succeed" | 节点收到异常时应返回什么 |
| **Type** | StringHash | no  | ""  | 要处理的异常类型，或未指定以处理所有异常 |

**输出：** 无

* * *

#### ThrowOnFail

如果子节点失败，则引发异常

|     |     |     |     |     |
| --- | --- | --- | --- | --- |
|输入 |类型 |必需 |默认 |描述 |
| **Type** | StringHash | yes | \-  | 要引发的异常类型 |

**输出：** 无

* * *

### Utility

#### CallSubtree

调用已注册的可插拔 BT 子树

|     |     |     |     |     |
| --- | --- | --- | --- | --- |
|输入 |类型 |必需 |默认 |描述 |
| **Label** | StringHash | yes | \-  | 要调用的子树的名称 |

**输出：** 无

* * *

#### ClearTimestampVariableOnExit

在 exit 时清除 timestamp 的值，以便任何大于或小于的比较将始终返回 false

|     |     |     |     |     |
| --- | --- | --- | --- | --- |
|输入 |类型 |必需 |默认 |描述 |
| **Name** | StringHash | yes | \-  | 要清除的时间戳变量的名称 |

**输出：** 无

* * *

#### EraseVariableOnExit

在子节点完成时擦除变量

|     |     |     |     |     |
| --- | --- | --- | --- | --- |
|输入 |类型 |必需 |默认 |描述 |
| **Name** | StringHash | yes | \-  | 要擦除的变量名称（或路径） |

**输出：** 无

* * *

#### HandleRequestSignal

在处理请求信号（如任务切换或脚本命令）时管理信号响应的发送

|     |     |     |     |     |
| --- | --- | --- | --- | --- |
|输入 |类型 |必需 |默认 |描述 |
| **Signal** | Blackboard | yes | \-  | 正在响应的信号 |
| **ResponseOnSuccess** | StringHash | no  | "Success" | 子树成功时返回的响应 |
| **ResponseOnFail** | StringHash | no  | "Failed" | 子树失败时返回的响应 |
| **ResponseOnEnter** | StringHash | no  |     | 进入子树时要返回的可选响应 |

**输出：** 无

* * *

#### IterateOverArray

迭代数组中的元素，另请参阅 BTArrayBreak

|     |     |     |     |     |
| --- | --- | --- | --- | --- |
|输入 |类型 |必需 |默认 |描述 |
| **Array** | BlackboardArray | yes | \-  | Array 到 iterator over |
| **OnComplete** | StringHash | no  | "Succeed" | 在到达数组末尾时返回值以传播 |

|     |     |     |     |
| --- | --- | --- | --- |
|输出 |类型 |必需 |描述 |
| **Element** | Any | yes | 当前正在迭代的 Array 元素 |

* * *

#### ReleaseEntityOnExit

退出时释放已声明实体的所有权

|     |     |     |     |     |
| --- | --- | --- | --- | --- |
|输入 |类型 |必需 |默认 |描述 |
| **ObjectSlot** | StringHash | yes | \-  | 实体所有权黑板上存储的对象 ID |

**输出：** 无

* * *

#### SetControlledEntity

指定要应用行为树的实体。仍然保持相同的行为 Blackboard

|     |     |     |     |     |
| --- | --- | --- | --- | --- |
|输入 |类型 |必需 |默认 |描述 |
| **EntityId** | EntityId | no  |     | 要控制的实体的 ID（可选，默认当前实体） |

**输出：** 无

* * *

#### SetTimestampVariableOnExit

将 timestamp 变量设置为退出时的当前时间

|     |     |     |     |     |
| --- | --- | --- | --- | --- |
|输入 |类型 |必需 |默认 |描述 |
| **Name** | StringHash | yes | \-  | 要设置的 timestamp 变量的名称 |

**输出：** 无

* * *

#### SetVariableOnExit

在子节点完成时将变量设置为特定值

|     |     |     |     |     |
| --- | --- | --- | --- | --- |
|输入 |类型 |必需 |默认 |描述 |
| **Name** | StringHash | yes | \-  | 要设置的变量名称（或路径）|
| **Value** | Any | yes | \-  | 要分配给变量的值 |

**输出：** 无

* * *

### Character movement

#### Character\_DisableAvoidanceForEntity

针对特定实体的避让开关。通常用于您的目标。当此节点终止时，将重新启用避障

|     |     |     |     |     |
| --- | --- | --- | --- | --- |
|输入 |类型 |必需 |默认 |描述 |
| **Entity** | EntityId | yes | \-  | 不再避免的实体 |

**输出：** 无

* * *

#### OverrideObstacleScale

临时更改避让障碍物的比例，直到子树完成

|     |     |     |     |     |
| --- | --- | --- | --- | --- |
|输入 |类型 |必需 |默认 |描述 |
| **Scale** | Float | yes | \-  | 要使用的新秤 |

**输出：** 无

* * *

#### Ship\_DisableAvoidanceForEntity

关闭针对特定实体的避障。通常用于您的目标。当此节点终止时，将重新启用避障

|     |     |     |     |     |
| --- | --- | --- | --- | --- |
|输入 |类型 |必需 |默认 |描述 |
| **Entity** | EntityId | yes | \-  | 不再避免的实体 |

**输出：** 无

* * *

#### Ship\_ToggleAvoidance

开启或关闭避障。当此节点终止时，将重置避障

|     |     |     |     |     |
| --- | --- | --- | --- | --- |
|输入 |类型 |必需 |默认 |描述 |
| **Avoidance Enabled** | Boolean | yes | \-  | 回避是启用还是禁用？ |

**输出：** 无

* * *
