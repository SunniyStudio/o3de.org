---
title: ATL 默认控件
description: 了解音频转换层 （ATL） 中定义的默认控件。
weight: 350
---

游戏使用 Audio Translation Layer (ATL) 控件与 Audio 中间件通信。ATL 控件映射到中间件创作工具中创作的各种数据。此抽象层使您可以灵活地快速更改映射，而无需更新游戏的控件集成。

若要查看音频控件类型的列表，请参阅 [ATL 控件窗格](/docs/user-guide/interactivity/audio/audio-controls-editor/atl-controls-pane).

## ATL 默认控件

**Audio Controls Editor** 默认情况下，会自动创建以下 ATL 控件。您可以在 'default_controls' 文件夹中找到控件。

![ATL controls that the Audio Controls Editor automatically creates by default.](/images/user-guide/audio/audio-atl-editor-default.png)

| 名称 | 说明 |
| --- | --- |
| do_nothing |  用作空白事件的触发器，其中可以分配 `play`/`stop` 触发器对。如果您在 `stop` 触发器上设置 `do_nothing`，则 `play` 触发器不会自动停止。  |
| get_focus |  当 O3DE Editor 中的应用程序窗口获得焦点时调用的触发器。  |
| lose_focus |  当 O3DE Editor 中的应用程序窗口失去焦点时调用的触发器。 要禁用 `get_focus` 和 `lose_focus` 触发器，请使用控制台命令 `s_IgnoreWindowFocus` = `1`。这在远程连接 Wwise Profiler 时非常有用，这样就可以在 Wwise 设计工具应用程序获得焦点时继续播放音频。   |
| mute_all |  单击位于 O3DE Editor 下方菜单栏上的 **Mute Audio** 时调用的触发器。 |
| unmute_all |  单击位于 O3DE Editor 下方菜单栏上的 **Mute Audio** 时调用的触发器。  |
| object_speed |  根据关卡中关联实体的速度进行更新的 RTPC 控件。您可以使用 `object_velocity_tracking` 控件启用基于每个实体的速度计算。   |
| object_velocity_tracking |  Switch 用于启用或禁用基于每个实体的 `object_speed` 值的计算。您无需将此开关连接到音频中间件，因为它会传达特定于 O3DE 的数据。   |
| ObstructionOcclusionCalculationType |  用于设置实体的声障和声笼计算方法的开关。开关状态值为`Ignore`, `SingleRay`, 和 `MultiRay`。您无需将此开关连接到音频中间件，因为它会传达特定于 O3DE 的数据。  |
