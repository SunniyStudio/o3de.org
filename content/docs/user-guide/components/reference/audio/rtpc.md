---
linkTitle: Audio RTPC
title: Audio RTPC 组件
description: 使用Audio RTPC 组件可在运行时设置数值，从而实时调整 Open 3D Engine 中的声音。
toc: true
---

**Audio RTPC** 组件提供基本的 [实时参数控制（RTPC）](/docs/user-guide/interactivity/audio/atl-default-controls)功能。RTPC 是一个命名变量，音频系统可以用多种不同的方式对其进行解释。它允许游戏开发人员在运行时从游戏中设置值，从而对声音进行实时调整。

![Audio Rtpc component](/images/user-guide/component/audio/component-audio-rtpc1.png)

## Audio RTPC 组件属性

| 名称 | 说明 | 默认值 |
|------|-------------|---------|
| **Default Rtpc** | 输入默认使用的音频 RTPC 名称。您可以将任何 RTPC 名称与实体关联，通常是要影响特定触发器的名称。 | `<Empty>` |

## EBus  请求总线接口

使用 EBus 接口的下列请求功能可与游戏的其他组件进行通信。

有关使用事件总线（EBus）接口的更多信息，请参阅 [使用事件总线（EBus）系统](/docs/user-guide/programming/messaging/ebus)。

| 名称 | 说明 | 参数 | 返回值 | 可脚本化 |
|------|-------------|------------|--------|------------|
| **SetValue** | 设置默认 RTPC 的值。 | `value` - RTPC 的浮点数值 | None | Yes |
| **SetRtpcValue** | 设置指定 RTPC 的值。 | `rtpcName` - 要设置的 RTPC 名称； `value` - 要设置的浮点数值 | None | Yes |
