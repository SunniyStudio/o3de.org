---
linkTitle: Audio Environment
title: Audio Environment 组件
description: 使用 Audio Environment 组件为 Open 3D Engine中的实体设置默认环境。
toc: true
---

**Audio Environment**组件提供对[Audio Translation Layer (ATL)](/docs/user-guide/interactivity/audio/audio-translation-layer)环境功能的访问。环境用于应用混响或回声等环境效果。

![Audio Environment component](/images/user-guide/component/audio/component-audio-environment1.png)

## Audio Environment 属性

| 名称 | 说明 | 默认值 |
|------|-------------|---------|
| **Default Environment** | 输入设置量时默认使用的音频环境名称。 | `<Empty>` |

## EBus请求总线接口

使用 EBus 接口的下列请求功能可与游戏的其他组件进行通信。

有关使用事件总线（EBus）接口的更多信息，请参阅 [使用事件总线（EBus）系统](/docs/user-guide/programming/messaging/ebus/)。

| 名称 | 说明 | 参数 | 返回值 | 可脚本化 |
|------|-------------|------------|--------|------------|
| **SetAmount** | 设置应用于默认环境的环境**'发送'**量（如果已设置）。 | `amount` - 要设置的量的浮点数值 | None | Yes |
| **SetEnvironmentAmount** | 设置应用于指定环境的环境**'发送'**量。 | `environmentName` - 要设置量的 ATL 环境名称； `amount` - 要设置的量的浮点数值 | None | Yes |
