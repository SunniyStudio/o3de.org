---
description: ' 在 Open 3D Engine 的<guilabel>Track View</guilabel>编辑器中控制播放头。 '
title: 控制播放头
---

Track View的**Play**工具栏包含控制序列时间线播放头的主要控件。

![Control the playhead in the Track View.](/images/user-guide/cinematics/cinematics-track-view-editor-play-toolbar.png)

**Play** 工具栏具有以下控件。


****

| 工具栏选项 | 说明 |
| --- | --- |
|  **Go to start of sequence**  |  播放头会移动到序列的**In**标记处。有关更多信息，请参阅 [设置输入和输出标记](#setting-the-in-and-out-markers).  |
|  **Play**  |  激活播放模式，使播放头在序列时间轴上向前移动。点击下拉菜单控制播放速度。 默认速度为 **1x**。其他选项包括 **2x**, **1/2x**, **1/4x**, 和 **1/8x**.  |
|  **Stop**  |  停止播放头。点击下拉菜单可指定默认的**Stop**或**Stop with Hard Reset**，后者会将播放头返回到序列的起点。  |
|  **Pause**  |  在播放模式下，将播放头停留在时间线的当前点。再次选择时，序列将继续播放。  |
|  **Go to end of sequence**  |  播放头移至序列的**Out**标记处。 更多信息，请参阅 [设置输入和输出标记](#setting-the-in-and-out-markers).  |
|  **Start Animation Recording**  |  也称为 “记录模式”，您可以在关卡内操作对象，动画键会自动添加到序列中。 更多信息，请参阅 [使用记录模式](/docs/user-guide/visualization/cinematics/using-record-mode/).  |
|  **Start Auto Recording**  |  选中时，也会激活录制模式。播放头会根据下拉菜单中指定的设置在时间轴上自动移动。 默认值为 **1 sec**。其他选项包括 **1/2 sec**, **1/5 sec**, **1/10 sec**, **1/25** sec, **1/50** sec, 和 **1/100** sec.  |
| Loop |  在播放模式下，当播放头到达 **End** 标记时，它会返回到序列的 **In** 标记处，并再次继续播放序列。  |
|  **Playhead Location**  |  显示播放头在时间线中的当前位置和指定帧频的相关信息。  |
|  **Frame Rate**  |  指定序列的帧频。该数字也用于帧捕捉。  |
|  **Active Camera/Camera Name**  |  只有当**Editor Viewport Camera**设置为**Sequence Camera**时，摄像机名称才会更新，以显示从**Director Node**激活的摄像机。  |
|  **Undo**  |  还原之前的操作。  |
|  **Redo**  |  应用之前的操作。 |

{{< note >}}
您还可以沿时间线手动调整播放头。在序列时间轴的顶部，选择并拖动播放头至您喜欢的位置。
{{< /note >}}

## 设置 In 和 Out标记

**In** 和 **Out**标记是序列时间轴顶部的红色小三角形。默认情况下，**In** 和 **Out**标记被设置为序列的开始和结束。您可以在列出帧/秒的时间轴顶部单击右键移动标记。根据右键单击的位置，轨迹视图会识别哪个标记最接近鼠标光标，并移动该标记。

例如，如果序列长度为 300 帧，而您右键单击的是 50 帧刻度线，则 **In** 标记将移动到该位置。如果在 200 帧刻度处单击右键，**Out** 标记将移动到该位置。

![Set the in and out markers in the timeline for a sequence.](/images/user-guide/cinematics/cinematics-track-view-editor-timeline.png)

{{< note >}}
当您移动**In**和**Out**标记时，将把更新的范围应用到**Go to start of sequence**、**Play**、**Go to end of Sequence**和**Loop**设置中。例如，如果您将**In**标记更改为两秒标记，**Go to start of sequence**图标现在将把播放头移到两秒标记处。
{{< /note >}}
