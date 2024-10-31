---
linkTitle: Audio Trigger
title: Audio Trigger 组件
description: 在 O3DE 中使用Audio Trigger组件在游戏中设置播放和停止触发器。
toc: true
---

**Audio Trigger** 组件提供了基本的播放和停止功能，因此您可以设置[音频翻译层（ATL）](/docs/user-guide/interactivity/audio/audio-translation-layer)播放和停止触发器，以便按需执行。有了音频触发器，你还可以让播放器在实体上按名称运行或停止音频触发器。

## Audio Trigger 属性

Audio Trigger 组件具有以下属性。

**Default 'play' Trigger**
输入该组件在调用 **'play'**时运行的音频触发器名称。您可以更改此属性，指定不同的默认音频触发器。

**Default 'stop' Trigger**
输入调用 **'stop'**时该组件运行的音频触发器名称。您可以在此指定任何触发器；您不需要指定**'stop'**触发器来停止音频，但最好将两个触发器配对使用。如果将此设置留空，**'stop'** 触发器只会停止为 **'play'** 指定的音频触发器。

**Obstruction Type**
为用于计算阻塞和闭塞的射线信号选择一个选项。

* **Ignore**
* **SingleRay**
* **MultiRay**

**Play immediately**
选择此选项可在组件激活时运行音频**'play'**触发器。

## EBus 请求总线接口

使用 EBus 接口的下列请求功能可与游戏的其他组件进行通信。

有关使用事件总线（EBus）接口的更多信息，请参阅 [使用事件总线（EBus）系统](/docs/user-guide/programming/messaging/ebus)。

### Play

运行默认的 **'play'** 触发器（如果已设置）。

**参数**
None

**返回值**
None

**可脚本化**
Yes

### Stop

Runs the default **'stop'** trigger, if set. If no **'stop'** trigger is set, ends the default **'play'** trigger.

**参数**
None

**返回值**
None

**可脚本化**
Yes

### ExecuteTrigger

运行指定的音频触发器。

**参数**
`triggerName` - 要运行的音频触发器名称。

**返回值**
None

**可脚本化**
Yes

### KillTrigger

取消指定的音频触发。

**参数**
`triggerName` - 要取消的音频触发器名称。

**返回值**
None

**可脚本化**
Yes

### KillTrigger

取消实体上激活的所有音频触发器。

**参数**
None

**返回值**
None

**可脚本化**
Yes

### SetMovesWithEntity

指定触发器是否应在实体移动时更新位置。

**参数**
`shouldTrackEntity` - Boolean 表示触发器是否应跟踪实体的位置。

**返回值**
None

**可脚本化**
Yes

## EBus 响应总线接口

使用 EBus 接口的下列响应功能可与游戏的其他组件进行通信。

有关使用事件总线（EBus）接口的更多信息，请参阅 [使用事件总线（EBus）系统](/docs/user-guide/programming/messaging/ebus/)。

### OnTriggerFinished

通知所有听众音频触发器已播放完毕（声音已结束）。

**参数**
`triggerId` - 成功执行的触发器的 ID。

**返回值**
None

**可脚本化**
Yes
