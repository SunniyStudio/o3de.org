---
linktitle: 事件消息
title: 事件消息系统
description: 了解 Open 3D Engine (O3DE) 中的事件消息系统。 
weight: 300
---

事件消息系统是**Open 3D Engine (O3DE)** 的核心部分，允许跨系统通信。当您开发的系统需要与 O3DE 的其他部分通信时，您需要使用 O3DE 的事件消息系统。

## 章节主题
1
| 主题 | 说明 | 
| --- | --- |
| [概述](overview) | 概述 O3DE 中的事件消息传递系统及其相互比较。 |
| [EBus](ebus) | **事件总线（EBus）** 系统概述。EBus 是一个单一的全局总线，所有系统都可以使用它来调用请求和发送信息，分别称为 **请求** 总线和 **通知** 总线。 |
| [深入EBus](ebus-design) | 详细了解 EBus 的技术设计和使用方法。 |
| [AZ::Interface](az-interface) | 作为 EBus 的简单替代方案，`AZ::Interface` 创建了用于跨系统调用请求的全局请求总线。 |
| [AZ::Event](az-event) | 作为 EBus 的简单替代方案，`AZ::Event` 发布单值消息，其他组件可以订阅并使用事件处理程序处理这些消息。|
