---
description: ' 在 Open 3D Engine的<guilabel>Track View</guilabel>编辑器中添加和删除轨道上的动画键。 '
title: 在轨道上添加和删除动画关键帧
---

您可以使用以下方法之一为时间线中的轨道添加关键帧：

**双击轨道**
这是向轨道添加动画关键帧的最快方法。这将在您点击的位置添加一个关键帧，并将数据存储在时间轴上的准确位置。

**添加关键帧
在**Keys**工具栏中，点击**Add Keys** 图标![Add Keys button](/images/user-guide/cinematics/cinematic-add-keys-track-view-editor.png)，然后点击时间轴。如果需要在时间轴上添加许多按键，这将非常有用。要停止添加按键，请选择移动、缩放或滑动图标![Move, scale, and slide buttons](/images/user-guide/cinematics/cinematics-move-scale-slide-keys-icon-track-view-editor.png)。

**Record Mode**
在**Play**工具栏中，点击**Record Mode**图标![Record button](/images/user-guide/cinematics/cinematics-record-icon-track-view-editor.png)，然后直接对组件进行更改。
进入记录模式后，您可以更新视口中属于序列一部分的组件实体。动画键会根据时间线播放头的当前位置，自动添加到时间线中相应的节点轨道上。
例如，如果您在三秒时为 **Transform** 组件指定了一个不同的值，则该更新的键就会出现在时间轴上。
要指定动画随时间播放，请将播放头移动到时间轴上的不同位置。否则，当您更新关卡中的组件实体时，将覆盖时间轴同一位置的按键。
要停止录制，请再次单击**Record Mode**图标。
更多信息，请参阅 [使用录制模式](/docs/user-guide/visualization/cinematics/using-record-mode/)。

您可以删除单个动画键，也可以单击并拖动来选择多个动画键。

**删除时间轴上的按键**

1. 在时间轴中，选择一个动画键。

1.  完成以下之一：
   + 右击关键帧，选择 **Delete**。
   + 按下键盘上的 **Delete** 键。
   + 在 **Keys** 工具栏中，点击 **Delete Keys** 图标 ![Delete keys button](/images/user-guide/cinematics/cinematics-delete-keys-icon-track-view-editor.png)。
