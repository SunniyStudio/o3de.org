---
linktitle: Audio
title: Adding Audio and Sound Effects
description: 使用 Open 3D Engine 的音频转换层 （ATL） 和音频控制编辑器 （ACE） 来开发游戏音频，而不会影响游戏逻辑。
---

O3DE 使用音频转换层 （ATL） 在 O3DE 和第三方音频中间件之间进行接口，因此您可以在不影响游戏逻辑的情况下开发和更改音频实现。游戏逻辑与 ATL 控件交互，ATL 控件通过 Audio Controls Editor 映射到其等效的音频中间件。例如，为了播放声音，游戏会执行一个 ATL 触发器，该触发器映射到音频中间件中的“Play”事件。

O3DE 支持 Audiokinetic Wave Works 交互式声音引擎 （Wwise），这是一种音频中间件解决方案，您可以使用它为游戏或模拟创建引人入胜的音景。有关如何配置和使用此 Gem 的信息，请参阅 [Wwise Audio Gem](/docs/user-guide/gems/reference/audio/wwise/audio-engine-wwise) 参考文档。

## 音频主题

* [音频系统概述](./overview)
* [Audio 组件](./components)
* [Audio Translation Layer](./audio-translation-layer)
* [ATL 默认控件](./atl-default-controls)
* [Audio Controls Editor](./audio-controls-editor)
* [Audio Console 变量](./console-variables)
* [高级主题](./advanced-topics)
