---
title: Audio Controls Editor
description: 使用 Open 3D Engine 中的 Audio Controls Editor 为您的项目创建声音效果。
weight: 400
---

您的游戏使用 Audio Translation Layer （ATL） 控件将所有操作、事件和参数传达给音频系统。这些 ATL 控件将映射到所选中间件（如 Wwise）中的一个或多个控件。使用 **Audio Controls Editor**，您可以创建控件并在 ATL 控件和中间件控件之间建立连接。

**打开 Audio Controls Editor**

* 在 O3DE 编辑器中，选择 **Tools**, **Other**, **Audio Controls Editor**.

Audio Controls Editor 有三个区域：

1. [ATL Controls Pane](./atl-controls-pane) - 项目中存在的控件的分层视图。显示的图标指定控件的类型。

1. [Inspector Pane](./inspector-pane) - 在 ATL Controls 窗格中选择的控件的属性。

1. [Audio Engine Middleware Controls Pane](./middleware-pane) - 在音频中间件创作应用程序中创建的控件。

![Audio Controls Editor (ACE) panes: ATL Controls pane, Inspector pane, and Audio Engine Middleware Controls pane](/images/user-guide/audio/audio_controls_browser_main.png)
