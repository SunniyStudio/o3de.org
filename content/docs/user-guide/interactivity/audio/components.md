---
title: Audio 组件
description:  使用 Open 3D Engine 中的音频组件将声音添加到关卡中。
weight: 200
---

使用以下音频组件在关卡中添加和配置声音。

| 组件 | 说明 |
| --- | --- |
| [Audio Area Environment](/docs/user-guide/components/reference/audio/area-environment) | 使在形状中移动的实体能够将环境效果应用于它们触发的任何声音。您还必须添加 shape 组件才能使用 Audio Area Environment 组件。 |
| [Audio Environment](/docs/user-guide/components/reference/audio/environment) | 提供对音频转换层 （ATL） 环境功能的访问。环境用于应用环境效果，例如混响或回声。 |
| [Audio Listener](/docs/user-guide/components/reference/audio/listener) | 在环境中放置虚拟麦克风。音频侦听器充当虚拟世界中声源的接收器，并根据侦听器的世界变换处理 3D 音频渲染。您可以单独指定音频侦听器的位置和旋转。 |
| [Audio Preload](/docs/user-guide/components/reference/audio/preload) | 加载和卸载 ATL 预加载，其中包含对 SoundBank 的引用。 |
| [Audio Proxy](/docs/user-guide/components/reference/audio/proxy) | 如果向实体添加多个音频组件，则为必需依赖项。它充当包装在组件中的代理音频对象。例如，如果同一实体上有一个 Audio Trigger 组件和一个 Audio RTPC 组件，则它们会使用此 Audio Proxy 组件与同一音频对象通信。 |
| [Audio RTPC](/docs/user-guide/components/reference/audio/rtpc) | 提供基本的实时参数控制 （RTPC） 功能。RTPC 是一个命名变量，音频系统可以通过多种不同的方式对其进行解释。它允许游戏开发人员在运行时设置游戏的值，以产生实时的声音调整。 |
| [Audio Switch](/docs/user-guide/components/reference/audio/switch) | 提供基本的音频转换层 （ATL） 开关功能。使用 switch 和 switch states，您可以指定实体的状态。音频中间件解释状态，修改声音的行为，并播放适当的声音。 |
| [Audio Trigger](/docs/user-guide/components/reference/audio/trigger) | 提供基本的播放和停止功能，以便您可以设置可按需执行的音频转换层 （ATL） 播放和停止触发器。使用音频触发器，您还可以使播放器能够在实体上按名称运行或停止音频触发器。 |
| [Multi-position Audio](/docs/user-guide/components/reference/audio/multi-position) | 控制从关卡中的多个位置播放的音频。 |

## 相关主题

* [Microphone Gem](/docs/user-guide/gems/reference/audio/microphone)
