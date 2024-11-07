---
linkTitle: MiniAudio 
title: MiniAudio Gem
description: MiniAudio Gem支持在 Open 3D Engine （O3DE） 项目中使用 MiniAudio 进行音频播放。
toc: true
---

MiniAudio Gem 支持在 **Open 3D Engine （O3DE）** 项目中使用 [MiniAudio](https://miniaud.io) 进行音频播放和定位。 支持多种音频格式，无需进入游戏模式即可在 Editor 中预览声音。

有关使用此 Gem 提供的组件的更多信息，请参阅 [[MiniAudio Playback 组件](/docs/user-guide/components/reference/audio/mini-audio-playback.md)  和 [MiniAudio Playback 组件](/docs/user-guide/components/reference/audio/mini-audio-listener.md) 文档。

## 支持的音频格式
* FLAC
* MP3
* OGG 
* WAV

## 全局音量控制

全局音量级别可以使用 '`MiniAudioRequestBus`' 进行控制，目前只能从 c++ 访问。
