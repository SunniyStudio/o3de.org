---
title: Inspector Pane
description: 使用 Audio Controls Editor 的 Inspector 窗格在 Open 3D Engine 中编辑当前所选控件的属性。
weight: 200
---

在 **Inspector** 窗格中，您可以编辑在 **ATL Controls 窗格中选择的控件的属性。您可以修改控件的 **Name**，选择 **Scope**，并修改 **Connected Controls**。

![Modify controls in the Inspector pane of the Audio Controls Editor](/images/user-guide/audio/audio-atl-editor-inspector.png)

下表描述了您可以在 Inspector 窗格中修改的 Inspector 属性。

| 属性 | 说明 |
| --- | --- |
| Name |  控件的名称。您可以在 **ATL Controls** 窗格中自定义名称。   |
| Scope |  控件可以存在于全局范围或按级别范围内。只要游戏正在运行，并且无论是否在当前关卡中使用该控件，具有全局范围的控件就存在。当特定级别定义为范围时，控件仅在加载该级别时存在。此设置在内存不足的系统中非常有用，因为控件仅在需要它们的级别中加载。  |
| Auto Load |  仅适用于预加载。如果选择 **Auto Load**，则使用此控件预加载的元素将被引用计数;仅创建它们的一个副本，该副本在所有用户之间共享。   |
| Preloaded Soundbanks |  仅适用于预加载。与 preload 连接的 soundBank 对于不同的平台可能有所不同。可以将不同的 SoundBank 添加到不同的组中，然后在 **Platforms** 字段中，您可以选择为目标中的每个平台加载哪个组。  |
| Platforms |  仅适用于预加载。您可以指定要为每个平台加载的 SoundBank 组。您可以在多个平台之间共享群组。  |
| Connected Controls |  包含连接到您的控件的中间件控件。  |

{{< note >}}
**Auto Load**、**Preloaded Soundbanks** 和 **Platforms** 属性仅在控件为 **Preload** 时显示。
{{< /note >}}
