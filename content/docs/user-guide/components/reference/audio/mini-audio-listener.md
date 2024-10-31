---
linkTitle: MiniAudio Listener 
title: MiniAudio Listener 组件
description: 使用 MiniAudio Listener 组件在 O3DE 游戏中播放音频。
toc: true
---

**MiniAudio Listener**组件可指定用于空间音频播放的实体位置，或让用户通过 `MiniAudioListenerRequestBus` 手动控制音频监听器的位置。 通常情况下，带有活动摄像头的实体会被指定为音频监听器实体，这样空间音频播放就会相对于观看者的位置和方向。

## MiniAudio Listener 属性

MiniAudio Listener 组件具有以下属性：

| 名称 | 说明 | 默认值 |
|------|-------------|---------|
| **Follow Entity** | 用作空间音频播放监听器的实体。 | `<Empty>` |
| **Listener Index** | MiniAudio Listener索引。 当声音被空间化时，它将相对于最近的Listener进行空间化。  | `0` |

## EBus 请求总线接口

使用 `MiniAudioListenerRequestBus` EBus 接口与 MiniAudio Listener组件通信。

有关使用事件总线（EBus）接口的更多信息，请参阅 [使用事件总线（EBus）系统](/docs/user-guide/programming/messaging/ebus)。

| 名称 | 说明 | 参数 | 返回值 | 可脚本化 |
|------|-------------|------------|--------|------------|
| SetFollowEntity | 为音频监听器设置用于位置和方向的实体。 | `followEntity` - 要跟踪的实体的 EntityId。 | None | Yes |
| SetPosition | 设置音频监听器的位置。 | `position` - 要使用的世界坐标 Vector3。 | None | Yes |
