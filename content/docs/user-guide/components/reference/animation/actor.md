---
linkTitle: Actor
title: Actor 组件
description: 在 Open 3D Engine (O3DE) 中使用Actor组件为实体添加Actor文件。
---

您可以使用**Actor**组件为您的游戏创建角色。将角色文件从 DCC 工具导入 O3DE 后，您就可以创建一个实体并为其添加 Actor 组件。您必须使用 Actor组件才能为游戏创建可控制的角色。

## 提供者

[EMotionFX](/docs/user-guide/gems/reference/animation/emotionfx)

## 依赖

您还必须添加以下组件之一：
+ **[Simple Motion](./simple-motion)** 组件 - 对Actor使用单个动作。
+ **[AnimGraph](./animgraph)** 组件 - 使用动画图形来控制Actor的动作。

## Actor 组件属性

![Actor component properties.](/images/user-guide/components/reference/animation/actor-component.png)

| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **Actor asset** | 为该组件设置Actor资产。 | Actor 资产 | 无 |
| **Attach To - Attachment type** | 设置附加到目标实体时使用的附加类型。| `None`, `Skin attachment` | `None` |
| **Attach To - Target entity** | <p>设置要附加的实体。</p><p>*此字段仅在 ** Attachment type** 设置为 `Skin attachment`时可用。*</p> | EntityId | None |
| **Render options - Draw skeleton** | 绘制Actor的关节。 | Boolean | `Disabled` |
| **Render options - Draw character** | 绘制Actor的网格。 | Boolean | `Enabled` |
| **Render options - Draw bounds** | 绘制角色的边界框。 | Boolean | `Disabled` |
| **Render options - Skinning method** | 设置Actor的蒙皮方法。 | `Dual quat skinning`, `Linear skinning` | `Dual quat skinning` |
| **Out of view - Force update Joints** | 当Actor离开摄像机视角时更新关节变换。 | Boolean | `Disabled` |
| **Out of view - Bounding box configuration - Bounds type** | 设置用于计算角色边框的方法。 | `Static`, `Bone position-based`, `Mesh vertex-based` | `Static` |
| **Out of view - Bounding box configuration - Automatically update bounds?** | <p>如果 `False`， 则只有在激活或显式调用重新计算时才会计算角色边界。 如果 `True`，则将根据下面的 **Update frequency** 和 **Update item skip factor**属性确定的固定时间间隔计算行为体边界。</p><p>*只有当**Bounds type**设置为 `Bone position-based` 或 `Mesh vertex-based`时，该字段才可用。*</p>| Boolean | `Enabled` |
| **Out of view - Bounding box configuration - Update frequency** | <p>以赫兹为单位设置边界框的更新频率。</p><p>*只有当**Bounds type**设置为`Bone position-based` 或 `Mesh vertex-based`时，该字段才可用。*</p> | 0.0 到 无限 | `0.0` |
| **Out of view - Bounding box configuration - Update item skip factor** | <p>设置边框更新只根据每*n<sup>th</sup>*个项目（骨骼或顶点）计算边框，其中*n*是**Update item skip factor**。</p><p>*只有当 **Bounds type** 设置为 `Bone position-based` 或 `Mesh vertex-based`时，该字段才可用。*</p> | 1 到 无限 | `1` |
| **Out of view - Bounding box configuration - Expand by** | 按一定百分比扩大计算边框的尺寸。| -99.999 到 无限 | `25.0` |

## ActorComponentRequestBus

| 请求名称 | 说明 | 参数 | 返回值 | 可脚本化 |
|-|-|-|-|-|
| **AttachToEntity** | 在特定关节索引处将附件附加到目标实体。 | Target: EntityId, Joint Index: Integer | None | Yes |
| **DebugDrawRoot** | 绘制Actor的根。 | Boolean | None | Yes |
| **DetachFromEntity** | 将附件从其所连接的实体上分离。 | None | None | Yes |
| **GetJointIndexByName** | 返回特定关节的关节点索引。| Joint Name: String | Joint Index: Integer | Yes |
| **GetJointTransform** | 返回特定关节的变换。 | Joint Index: Integer, Joint Space: Integer | Transform: Quaternion | Yes |
| **GetRenderCharacter** | 如果角色已渲染，则返回 `True`。 | None | Boolean | Yes |
| **SetRenderCharacter** | 若为`True`，则渲染Actor。 | Boolean | None | Yes |

## ActorComponentNotificationBus

| 请求名称 | 说明 | 参数 | 返回值 | 可脚本化 |
|-|-|-|-|-|
| **OnActorInstanceCreated** | 创建角色实例时通知侦听器。 | None | Actor 实例 | Yes |
| **OnActorInstanceDestroyed** | 当角色实例被销毁时通知侦听器。 | None | Actor 实例 | Yes |

## ActorNotificationBus

| 通知名称 | 说明 | 参数 | 返回值 | 可脚本化 |
|-|-|-|-|-|
| **OnMotionEvent** | 在动作事件开始时通知侦听器。 | None | Motion Event: Motion | Yes |
| **OnMotionLoop** | 当动作开始新的循环时通知监听器。 | None | Motion Name: String | Yes |
| **OnStateEntered** | 完成向特定状态的转换时通知侦听器。 | None | State: String | Yes |
| **OnStateEntering** | 开始过渡到特定状态时通知侦听器。 | None | State: String | Yes |
| **OnStateExited** | 当完成从特定状态的转换时，通知侦听器。 | None | State: String | Yes |
| **OnStateExiting** | 从特定状态开始过渡时通知侦听器。 | None | State: String | Yes |
| **OnStateTransitionEnd** | 当状态转换完成时通知侦听器。 | None | New State: String, Old State: String | Yes |
| **OnStateTransitionStart** | 在状态转换开始时通知侦听器。 | None | New State: String, Old State: String | Yes |


## 相关链接
+ [为Actor使用多个蒙皮附件](/docs/user-guide/visualization/animation/actor-multiple-skin)
+ [设置Actor实体](/docs/user-guide/visualization/animation/actor-component-entity-setup)
