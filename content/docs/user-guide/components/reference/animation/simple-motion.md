---
linkTitle: Simple Motion
title: Simple Motion 组件
description: ' 在Open 3D Engine (O3DE)中使用Simple Motion组件为Actor添加运动效果。'
---

您可以使用**Simple Motion**组件来播放一个没有动画图形的动作。将此组件添加到带有**[Actor](/docs/user-guide/components/reference/animation/actor/)** 组件的实体中，就可以为您的Actor使用单一动作。如需复杂的动作，请参考  **[AnimGraph](/docs/user-guide/components/reference/animation/animgraph/)** 组件。

## 提供者

[EMotionFX](/docs/user-guide/gems/reference/animation/emotionfx)

## 依赖

[Actor 组件](./actor)

## Simple Motion 属性 

![Add the Simple Motion component to an entity to assign a motion for the actor.](/images/user-guide/components/reference/animation/simple-motion-component.png)

| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **Preview In Editor** | 在**Open 3D Engine (O3DE) 编辑器**中播放动作。 | Boolean | `Disabled` |
| **Motion** | 	指定要为Actor制作动画的运动资产。 | Motion asset | None |
| **Loop motion** | 连续运行动画。 | Boolean | `Disabled` |
| **Retarget motion** | 允许在配置了特定骨骼长度的Actor身上创建的动作在另一个具有不同骨骼长度的Actor身上执行。应用时，动作不会影响骨骼长度。骨骼必须遵循相同的层次结构，骨骼名称必须相同才能正常工作。 | Boolean | `Disabled` |
| **Reverse motion** | 反向播放动画。 | Boolean | `Disabled` |
| **Mirror motion** | 镜像角色身体部位的动画。例如，如果Actor踢右腿时左腿着地，镜像效果会使右腿着地时左腿踢地。 | Boolean | `Disabled` |
| **Play speed** | 指定播放动作的速率。 | 0 to Infinity | `1.0` |
| **Blend In Time** | 指定动作的混合时间（秒）。您可以为要启动的动作设置该参数，该动作是由之前的动作混合而成的。| 0 到 无限 | `0.0` |
| **Blend Out Time** | 指定动作的混合时间（秒）。您可以为当前正在播放并将与下一个动作混合的动作设置该参数。 | 0 到 无限 | `0.0` |
| **Play on active** | 当实体被激活时启动动画。 | Boolean | `Enabled` |
| **In-place** | 删除动画中根节点的位置或旋转变化。 | Boolean | `Disabled` |

## SimpleMotionComponentRequestBus ##

将以下请求函数与 `SimpleMotionComponentRequestBus` EBus 接口结合使用，可在游戏中与简单运动组件进行通信。更多信息，请参阅 [使用事件总线（EBus）系统](/docs/user-guide/programming/messaging/ebus/).
1
| 请求名称| 说明 | 参数 | 返回值 | 可脚本化 |
|-|-|-|-|-|
| `BlendInTime` | 设置动作的 **Blend In Time**。 | Time: Float | None | Yes |
| `BlendOutTime` | 设置动作的 **Blend Out Time**。 | Time: Float | None | Yes |
| `GetBlendInTime` | 返回动作的 **Blend In Time**。 | None | Time: Float | Yes |
| `GetBlendOutTime` | 返回动作的**Blend Out Time**。 | None | Time: Float | Yes |
| `GetLoopMotion` | 如果动作被设置为循环，返回`True`。 | None | Boolean | Yes |
| `GetMotion` | 返回动作的AssetId。| None | Motion: AssetId | Yes |
| `GetPlaySpeed` | 返回**Play speed**。 | None | Speed: Float | Yes |
| `GetPlayTime` | 返回当前动作开始后所经过的时间。 | None | Time: Float | Yes |
| `LoopMotion` | 如果 `True`，设置动作为循环。 | Boolean | None | Yes |
| `MirrorMotion` | 如果 `True`， 镜像动作。 | Boolean | None | Yes |
| `Motion` | 设置动作资产。 | Motion: AssetId | None | Yes |
| `PlayMotion` | 开始播放动作。 | None | None | Yes |
| `PlayTime` | 从特定时间开始播放动作。 | Time: Float | None | Yes |
| `RetargetMotion` | 如果 `True`， 可以将运动重定向到当前Actor。 | Boolean | None | Yes |
| `ReverseMotion` | 如果 `True`，将动作设置为反向播放。 | Boolean | None | Yes |
| `SetPlaySpeed` | 设置 **Play speed**。 | Speed: Float | None | Yes |
