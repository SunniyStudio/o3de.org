---
title: ATL 控件窗格
description: 使用音频控件编辑器的 ATL 控件窗格添加新的音频控件，并在 Open 3D Engine 中选择和编辑音频控件。
weight: 100
---

**ATL Control** 面板具有以下控件类型。关联的图标指定控件的类型。您可以自定义控件的名称。

|音频控制类型 |图标 |描述 |
| --- | --- | --- |
| Trigger | ![Trigger control](/images/user-guide/audio/audio_atl_trigger.png) |  连接到音频中间件中事件的容器。您可以编写事件来完成播放或停止声音、将总线静音或取消静音等操作。要预览触发器，请右键单击并选择 **Execute Trigger**，或按空格键。 |
| RTPC | ![RTPC control](/images/user-guide/audio/audio_atl_rtpc.png) | Real-Time Parameter Control （RTPC） 是一个浮点变量，它由 Game Logic 随时间推移而更新。RTPC 连接到音频中间件中的参数，这些参数用于驱动和调制声音特性。 |
| Switch | ![Switch control](/images/user-guide/audio/audio_atl_switch.png) | 一个变量，可以处于游戏逻辑可以设置的多种状态（称为 switch 状态）之一。例如，SurfaceType 开关可以具有 Rock、Sand 或 Grass 的值。 |
| Environment | ![Environment control](/images/user-guide/audio/audio_atl_environment.png) | 可以在音频对象上设置环境，从而控制环境效果（如混响和回声）的数量。 |
| Preload | ![Preload control](/images/user-guide/audio/audio_atl_preload.png) |预加载连接到 Sound SoundBank，声音库是包含打包音频数据的音频文件。此音频数据包含信号内容和元数据。 |

**显示控件类型的有限子集**

1. 在 **Audio Controls Editor** 中， 点击 **Filters**.

1. 选择或清除控件类型。

![Select or clear the control types](/images/user-guide/audio/audio-atl-editor-filter.png)

**添加新控件**

1. 点击 **Add**.

1. 选择要添加的控件类型。

![Select the type of control you want to add](/images/user-guide/audio/audio-atl-editor-add.png)
