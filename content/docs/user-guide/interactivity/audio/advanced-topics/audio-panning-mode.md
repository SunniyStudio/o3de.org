---
title: 设置平移模式
description: 在 Open 3D Engine 中设置音频平移。
---

可以在引擎中设置两种常见的扬声器平移模式：Speakers （扬声器） 和 Headphones （耳机）。

将声像调整模式设置为 Speakers 会将前 L/R 声像调整设置为相距 60 度。

将平移模式设置为 Headphones 会将前 L/R 平移设置为相距 180 度。

这些声像摆位角度将改变输出混音，以更好地与真实听者周围的扬声器的物理布置配合。

要设置扬声器平移模式，请使用 C++ 向音频系统发送请求。

## C++ 示例：请求设置平移模式

```cpp
if (auto audioSystem = AZ::Interface<Audio::IAudioSystem>::Get();
    audioSystem != nullptr)
{
    Audio::SystemRequest::SetPanningMode setPanMode;
    setPanMode.m_panningMode = Audio::PanningMode::Headphones; // or PanningMode::Speakers

    audioSystem->PushRequest(AZStd::move(setPanMode));
}
```
