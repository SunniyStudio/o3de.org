---
title: Audio Translation Layer
description: 了解 Open 3D Engine 音频转换层 （ATL）。
weight: 300
---

Audio Translation Layer (ATL)管理音频系统的状态，并将游戏请求中继到底层音频中间件。ATL 是一个通用层，不包含特定于中间件的代码，这使得可以使用不同的音频中间件实现。ATL 管理音频对象及其状态，以及任何加载的音频文件，例如 SoundBank。

**Audio Controls Editor** (ACE) 是一个 O3DE Editor 插件，用于管理项目的 ATL 控件与其等效音频中间件之间的映射。

ATL 控件支持项目的灵活工作流，例如以下情况：

* 您已准备好将声音集成到游戏中，但尚未创建音频中间件内容。您可以先创建 ATL 控件并将其集成到游戏中。音频内容完成后，您可以将现有的 ATL 控件连接到新的中间件控件。

* 稍后在项目中对中间件控件进行更改，这将断开与中间件控件的连接。您可以修复 ATL 和中间件之间的连接，而不是查找使用声音的每个实例。这会自动修复断开的连接。
