---
linkTitle: Audio Listener
title: Audio Listener 组件
description: 使用 Audio Listener 组件在Open 3D Engine环境中添加虚拟麦克风。
toc: true
---

通过**Audio Listener**组件，您可以在环境中放置一个虚拟麦克风。音频监听器充当虚拟世界中的声源汇，三维音频渲染会根据监听器的世界变换进行处理。您可以独立指定音频监听器的位置和旋转。

## Audio Listener 属性

**Audio Listener** 组件具有以下属性：

**Rotation Entity**
将此属性链接到音频监听器采用变换旋转部分的实体。如果未指定值，则使用当前实体。

**Position Entity**
将此属性链接到音频监听器采用变换位置部分的实体。如果未指定值，则使用当前实体。

**Fixed Offset**
将此属性链接到音频监听器采用偏移部分变换的实体。如果未指定值，则使用当前实体。

**Listener Enabled**
控制监听器的初始状态。

## 使用 Audio Listener 组件

一个游戏只支持一个音频监听器。您可以将**Audio Listener**组件添加到包含游戏摄像头的实体中。

**设置Audio Listener组件**

1. 在 O3DE 编辑器中，在关卡中右击视口，选择 **Create new component entity**。

1. 点击 **Tools**, **Entity Inspector**。确保在视口中选中你的新的组件实体。

1. 在 **Entity Inspector**中，点击 **Add Component**, **Audio**, **Audio Listener**。

## EBus 请求总线接口

使用 EBus 接口的下列请求功能可与游戏的其他组件进行通信。

有关使用事件总线（EBus）接口的更多信息，请参阅 [使用事件总线（EBus）系统](/docs/user-guide/programming/messaging/ebus/)。

### SetRotationEntity

指定音频监听器将采用的具有变换旋转部分的实体。

**参数**
`entityId` - 用于变换旋转部分的实体

**返回值**
None

**可脚本化**
Yes

### SetPositionEntity

指定具有音频监听器将采用的变换位置部分的实体。

**参数**
`entityId` - 用于变换位置部分的实体

**返回值**
None

**可脚本化**
Yes

### SetFullTransformEntity

指定音频监听器将采用的具有完整变换的实体。

**参数**
`entityId` - 用于转换的实体

**返回值**
None

**可脚本化**
No
