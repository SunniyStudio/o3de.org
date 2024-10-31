---
linkTitle: MiniAudio Playback 
title: MiniAudio Playback 组件
description: 使用 MiniAudio Playback 组件在 O3DE 中播放游戏中的音频。
toc: true
---

**MiniAudio Playback** 组件提供了在全局或相对于音频监听器以特定音量播放声音文件中的音频的功能。

## 支持的音频格式
* FLAC
* MP3
* OGG 
* WAV

## MiniAudio Playback 属性

MiniAudio Playback 组件具有以下属性。

| 名称 | 说明 | 默认值 |
|------|-------------|---------|
| **Play Sound** | 在编辑器中播放当前声音。 | |
| **Stop Sound** | 在编辑器中停止当前声音。 | |
| **Sound Asset** | 调用 **'play'**时播放音频文件的路径。 | `<Empty>` |
| **Autoplay** | 选择该选项可在编辑器或游戏中激活实体时自动播放音频。 | `False` |
| **Volume** | 0.0 至 10.0 范围内的线性（非分贝）音频电平。 | `1.0` |
| **Auto-follow** | 声音的位置是否应该移动以匹配实体的位置。| `False` |
  | **Loop** | 声音是否循环播放。 | `False` |
| **Spatialization** | 相对于当前音频聆听者，声音是否应在世界中具有 3D 位置。 | `False` |
| **Attenuation** | 当听音者远离声音时使用的衰减模型、 `Inverse`, `Exponential` 或 `Linear`. | `Inverse` |
| **Min Distance** | 衰减的最小距离（米）。 | `3.0` |
| **Max Distance** | 衰减的最大距离（米）。 | `3.0` |

## EBus 请求总线接口

使用 `MiniAudioPlaybackRequestBus` EBus 接口与 MiniAudio Playback 组件通信。

有关使用事件总线（EBus）接口的更多信息，请参阅 [使用事件总线（EBus）系统](/docs/user-guide/programming/messaging/ebus)。

| 名称 | 说明 | 参数 | 返回值 | 可脚本化 |
|------|-------------|------------|--------|------------|
| Play | 如果已设置，则播放声音。 | None | None | Yes |
| Stop | 如果设置，则停止声音。 | None | None | Yes |
| SetVolume | 设置声音的音量。 | `volume` - 0.0 至 10.0 范围内的线性（非分贝）音量水平 | None | Yes |
| SetLooping | 设置声音是否循环播放。 | `loop` - True 为循环，或 False 为不循环 | None | Yes |
| IsLooping | 返回声音是否设置为循环。 | None | 循环时为 True，不循环时为 False | Yes |
| SetSoundAsset | 设置要播放的声音资产。 | `soundAsset` - 声音资产在项目或 gem 资产文件夹中的 SoundAssetRef。 | None | Yes |
| GetSoundAsset | 获取当前声音资产。 | None | 当前 SoundAssetRef | Yes |
