---
title: Middleware Controls Pane
title: Audio Engine Middleware Controls Pane
description: 使用 Audio Controls Editor 的音频引擎中间件控件窗格来筛选和选择要分配给 Open 3D Engine 中的 ATL 控件的特定于中间件的控件。
weight: 300
toc: true
---

**Wwise** 此处显示为音频引擎中间件的 Controls 窗格的示例。

## 筛选显示的控件

* 在 **Audio Controls Editor** 的音频引擎中间件控件窗格中，在 **Search** 栏中输入您的搜索词。

## 隐藏已分配的控件

* 选择 **Hide Assigned**。未分配的控件以橙色文本显示。

## 在 ATL 控件和特定于中间件的控件之间创建连接

* 在音频引擎中间件控件窗格中，选择控件并将其拖动到 **Inspector** 窗格的 **Connected Controls** 区域。

![Drag the selected control to the Connected Controls area of the Inspector pane](/images/user-guide/audio/audio-atl-editor-connected.png)

## 创建控件

1. 在音频引擎中间件控件窗格中，选择一个控件并将其拖动到 **ATL Controls** 窗格。

  ![Drag a middleware control directly into the ATL Controls pane to create a new control.](/images/user-guide/audio/audio-atl-editor-new.png)

  这将创建一个新控件，该控件与中间件控件具有相同的名称。中间件控件和 ATL 控件也会自动连接。

1. 要预览控件，选择 **File**, **Save All**。
