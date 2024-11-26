---
title: 在运行时获取声音持续时间
description: 在运行时加载 Open 3D Engine 音频系统中的音效信息。
---

为了在运行时获取声音的持续时间，需要先将播放该声音的事件发送到音频中间件。发布后，中间件将回调到音频系统并提供持续时间信息。如果您对此信息感兴趣，只需通过使用的 ATL 触发器 ID 连接到`AudioTriggerNotificationBus`即可。

通过覆盖`ReportDurationInfo`函数，您将收到该信息的通知。

## C++ 示例：获取声音的持续时间

```cpp
class MyClass
    : protected AudioTriggerNotificationBus::Handler
{
public:
    MyClass()
    {
        TAudioControlID triggerId = ...;
        AudioTriggerNotificationBus::Handler::BusConnect(TriggerNotificationIdType{ this });
 
        // execute the m_triggerId ...
        if (auto audioSystem = AZ::Interface<IAudioSystem>::Get();
            audioSystem != nullptr)
        {
            // Example 1: via raw Audio Request
            Audio::ObjectRequest::ExecuteTrigger execTrigger;
            execTrigger.m_triggerId = triggerId;
            execTrigger.m_owner = this;
            audioSystem->PushRequest(AZStd::move(execTrigger));

            // Example 2: via AudioProxy
            IAudioProxy* proxy = audioSystem->GetAudioProxy();
            if (proxy)
            {
                // The second parameter is the "owner" override, which matches
                // what Id is connected to on the notification bus.
                proxy->Initialize("Example Sound", this);
                proxy->ExecuteTrigger(triggerId);
                proxy->Release();
            }
        }
    }
 
    ~MyClass()
    {
        AudioTriggerNotificationBus::Handler::BusDisconnect();
    }
 
protected:
    void ReportDurationInfo(
        TAudioControlID triggerId,
        TAudioEventID eventId,
        float duration,
        float estimatedDuration) override
    {
        // ...
    }
};
```
