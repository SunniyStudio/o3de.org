---
title: Audio系统概述
description: 了解 Open 3D Engine 音频系统。
weight: 100
---

O3DE 音频系统由 Gem、组件和内容组成。

## Gems 

O3DE 提供两个音频 Gem：
+ **AudioSystem**
+ **AudioEngineWwise**

AudioEngineWwise 是 O3DE 为 Audiokinetic Wwise 提供的音频引擎实现。音频引擎实现将通用音频转换层 （ATL） 状态请求转换为对音频中间件 API 的实际调用。它还根据音频中间件的需要为文件 I/O 和内存分配实现低级钩子。

AudioEngineWwise Gem 依赖于 AudioSystem Gem。建议您同时启用这两个 Gem 以启用音频，但 AudioSystem Gem 没有依赖项，可以自行启用。这样就有可能开发和使用其他音频中间件 Gem 来代替 AudioEngineWwise。

O3DE 音频 Gem 具有以下模块：


****

| 模块 | 说明 |
| --- | --- |
| Audio System |  Audio System Gem 的一部分。包含音频转换层 （ATL） 代码，并管理 O3DE 中音频系统的状态。这个模块的大部分运行在 audio 线程上，但它也与主线程同步。  |
| Audio System Editor |  一个 O3DE Editor 插件，也是 Audio System Gem 的一部分。包含用于创建和管理 ATL 控件的 **音频控件编辑器** （ACE）。  |
| Audio Engine Wwise |  Audio Engine Wwise Gem 的一部分。包含 Wwise 的`AudioSystemImplementation`接口的实现。包含所有 Audiokinetic API。这是唯一与 Wwise SDK 链接的模块。可以配置为使用 Wwise LTX 或完整版 Wwise。  |
| Audio Engine Wwise Editor |  一个 O3DE Editor 插件，也是 Audio Engine Wwise Gem 的一部分。这是 O3DE 使用 Wwise 时 **Audio Controls Editor** 加载的附加模块。  |

## 组件 

O3DE Editor 中提供的 Core Audio 组件使您能够触发音效、播放环境音乐、使用 RTPC 更改声音变量、应用环境效果、将听者用作虚拟麦克风等等。有关完整列表，请参阅 [音频组件](./components).

## 内容 

O3DE 音频包含以下内容：


****

| 内容 | 说明 |
| --- | --- |
| Media |  O3DE 在运行时加载 SoundBank 和松散媒体。音频中间件创作工具编译并生成媒体文件。  |
| Project | 音频中间件创作工具使用工程来管理源音频文件、调整声音和设置，以及生成运行时就绪的媒体。Audio Controls Editor 还使用该项目来帮助将 ATL 控件映射到等效的音频中间件。 |
| ATL 库 |  当音频系统将 ATL 控件映射到其等效的音频中间件时，它会创建 ATL 库，这些库将另存为 XML 文件。O3DE 在启动时加载这些库，并使用运行时数据填充 ATL，以便游戏可以控制音频系统。  |
