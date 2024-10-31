---
linkTitle: Audio Switch
title: Audio Switch 组件
description: 使用Audio Switch组件为 O3DE 中的实体设置默认开关和开关状态。
toc: true
---

**Audio Switch**组件提供了基本的[音频转换层（ATL）](/docs/user-guide/interactivity/audio/audio-translation-layer) 开关功能。通过开关（和开关状态），您可以指定实体的状态。音频中间件会解释状态、修改声音行为并播放相应的声音。

![Audio Switch component](/images/user-guide/component/audio/component-audio-switch1.png)

## Audio Switch 属性

| 名称 | 说明 | 默认值 |
|------|-------------|---------|
| **Default Switch** | 输入默认使用的音频交换机名称。您可以将任何音频交换机与实体关联。 | `<Empty>` |
| **Default State** | 输入默认使用的音频开关状态名称。使用 [Audio Controls Editor](/docs/user-guide/interactivity/audio/audio-controls-editor) （音频控制编辑器）为开关分配状态。激活此组件时，**Default Switch**将设置为**Default State**。 | `<Empty>` |

## EBus 请求总线接口

使用 EBus 接口的下列请求功能可与游戏的其他组件进行通信。

有关使用事件总线（EBus）接口的更多信息，请参阅 [使用事件总线（EBus）系统](/docs/user-guide/programming/messaging/ebus)。

| 名称 | 说明 | 参数 | 返回值 | 可脚本化 |
|------|-------------|------------|--------|------------|
| **SetState** | 设置默认开关的指定状态。 | `stateName` - 要设置的状态名称  | None | Yes |
| **SetSwitchState** | 将指定开关设置为指定状态。 | `switchName` - 要设置的开关名称； `stateName` - 要设置的状态名称 | None | Yes |
