---
title: 常见任务、EBus 和处理程序
description: 使用 EBus 实现一些常见的游戏编程任务。
draft: true
---

下面是一些常见的游戏编程任务，以及可以用来实现这些任务的 EBus 和处理程序。

## 检测鼠标、键盘或其他按钮事件


****

|  |  |
| --- |--- |
| Bus | AZ::InputEventNotificationBus |
| Events | OnPressed, OnHeld, OnReleased |
| File | InputEventBus.h |

使用这些事件来检测鼠标、键盘或其他按钮何时被按下、按住或释放。

## 检测实体或组件是否就绪


****

|  |  |
| --- |--- |
| Bus | LmbrCentral::MeshComponentNotificationBus |
| Events | OnMeshCreated, OnMeshDestroyed |
| File | MeshComponentBus.h |

即使在创建实体并激活其组件后，也可能无法完全加载可视化数据。当网格创建完成时，会发生 `OnMeshCreated` 事件。如果您想访问底层的 `ICharacterInstance` 和 `ISkeletonAnim`成员以播放动画，这将非常有用。更一般地说，无论这对您的应用程序意味着什么，将一个组件或实体声明为 “活着 ”或游戏准备就绪都是非常有用的。

## 获取和设置物理特性


****

|  |  |
| --- |--- |
| Bus | LmbrCentral::PhysicsComponentRequestBus |
| Methods | AddImpulse, GetMass, SetMass, GetVelocity, SetVelocity, etc. |
| File | PhysicsComponentBus.h |

`PhysicsComponentRequestBus` 包含获取或设置物体物理特性（如质量、密度、速度和水阻尼）的有用方法。

## 获取动画事件通知


****

|  |  |
| --- |--- |
| Bus | LmbrCentral::CharacterAnimationNotificationBus |
| Event | OnAnimationEvent |
| File | CharacterAnimationBus.h |

如果您在 Geppetto 的 `.animevents` 文件中设置了动画，则在动画播放过程中，每个动画事件都会调用一个 `OnAnimationEvent` 事件。您可以监控该事件以获取动画事件通知。在 Geppetto 中为动画事件配置的字符串保存在 `LmbrCentral::AnimationEvent::m_animName` 变量中。

## 获取或设置实体在世界中的位置 


****

|  |  |
| --- |--- |
| Bus | AZ::TransformBus |
| Methods | GetWorldX, SetWorldX, GetWorldY, SetWorldY, etc. |
| File | TransformBus.h |

`TransformBus` 包含许多有用的方法，用于获取或设置实体在世界上的位置，如 xyz 轴位置。

## 手动播放动画 

****

|  |  |
| --- |--- |
| Bus | LmbrCentral::SkinnedMeshComponentRequestBus |
| Method | GetCharacterInstance |
| File | SkinnedMeshComponent.h |

要手动播放动画，请使用角色实例中的 `ISkeletonAnim`。要从`ICharacterInstance`中获取 `ISkeletonAnim`，请使用`ICharacterInstance::GetISkeletonAnim()`。

## 使用来自其他组件的EBus


****

|  |  |
| --- |--- |
| Bus | AZ::EntityBus |
| Events | OnEntityActivated, OnEntityDeactivated |
| File | EntityBus.h |

`OnEntityActivated` 和 `OnEntityDeactivated`事件是在实体的所有组件的`Activate()` 或 `Deactivate()` 函数被调用后调用的。如果您想让您的组件使用另一个组件已在其 `Activate()` 函数中设置好的 EBus，这些事件将非常有用。

## 使用 Tick 事件 


****

|  |  |
| --- |--- |
| Bus | AZ::TickBus |
| Event | OnTick |
| File | TickBus.h |

tick 是组件应用程序生成的时间单位。`OnTick`事件是应用程序发出 tick 的信号，每帧都会被调用。默认情况下，处理程序根据组件初始化的顺序接收事件，但也可以覆盖这一顺序。更多信息，请参阅[Tick 总线和组件](/docs/user-guide/programming/components/tick)。
