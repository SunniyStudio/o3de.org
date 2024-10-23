---
linkTitle: Attachment
description: ' 在 Open 3D Engine 中，使用 Attachment 组件可将实体的骨骼连接到另一实体骨骼上的骨骼。'
title: Attachment 组件
---



**Attachment** 组件可让一个实体连接到另一个实体骨架上的骨骼。

## Attachment组件属性

![Attachment component properties.](/images/user-guide/component/attachment-component-properties.png)

**Attachment**组件具有以下属性。


****
1
| 名称 | 说明 |
| --- | --- |
| Target entity |  指定要附加的角色实体。 |
| Joint name |  指定要附加到实体的关节。  |
| Position offset |  指定从目标骨骼偏移的 x、y 和 z 本地位置。  |
| Rotation offset |  指定目标骨骼的 x、y 和 z 局部旋转偏移。 |
|  **Scale offset**  |  指定目标骨骼的 x、y 和 z 局部比例偏移。  |
| Attached initially |  指定是否自动附加到目标实体。  |
|  **Scaling**  |  指定对象缩放的确定方式。可以指定以下值：   |

## EBus 请求总线接口

使用事件总线（EBus）接口的下列请求功能可与游戏的其他组件进行通信。

更多信息，请参阅 [使用事件总线（EBus）系统](/docs/user-guide/programming/messaging/ebus/)。


****

| 函数名称 | 说明 | 参数 | 可脚本化 |
| --- | --- | --- | --- |
| Attach |  更改实体的连接目标。实体将从先前的目标脱离。  |  `targetEntityId` - 要附加的实体的 ID。 `targetBoneName` - 要附加实体的骨骼名称。如果找不到骨骼，则附加到目标实体的变换原点。 `offsetTransform` - 附件与目标的偏移量。  | 是 |
| Detach |  将目标与实体分离。 | 无 | 是 |
| SetAttachmentOffset |  更新实体与目标的偏移量。  | offsetTransform - 附件与目标的偏移量。 | 是 |

## EBus 通知总线接口 

使用 EBus 接口的下列通知功能可与游戏的其他组件进行通信。

更多信息，请参阅[使用事件总线（EBus）系统](/docs/user-guide/programming/messaging/ebus/)。


****

| 函数名称 | 说明 | 参数 | 可脚本化 |
| --- | --- | --- | --- |
| OnAttached |  表示实体已连接到目标。  | targetEntityId - 所连接目标的 ID。 | 是 |
| OnDetached |  表示实体正在脱离目标。  |  `targetEntityId` - 正在被分离的目标的 ID。 | 是 |
