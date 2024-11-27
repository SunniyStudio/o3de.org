---
description: ' 了解如何在 Open 3D Engine 中使用游戏输入系统。 '
title: Input
---

使用本节中的信息了解 Open 3D Engine 中的游戏输入。

{{< note >}}
有关 **Input** 组件的信息，请参阅 [Input](/docs/user-guide/components/reference/gameplay/input)。
{{< /note >}}

输入是交互式体验与所有其他娱乐媒体的区别。无论类型、操作系统或设备如何，每个游戏都是由玩家与物理输入设备的某种形式的交互驱动的。有许多不同类型的输入设备，每种设备都可以生成各种输入数据。此外，此数据传送到应用程序的方式在设备或操作系统之间很少一致。作为跨操作系统和设备的游戏引擎，O3DE 的目标是提供对来自任何受支持设备的输入数据的独立访问。目标是提供具有以下品质的通用接口：
+ 操作系统和设备无关
+ 可扩展
+ 高效

此外，输入数据应具有以下属性：
+ 必须可通过订阅输入设备事件来获取
+ 必须通过轮询输入设备的状态或值按需获取
+ 可以是自定义负载，但不使用时不会增加开销

## AZ 框架输入

O3DE 输入接口称为 AZ 框架输入。AZ 框架已经为特定于操作系统的功能（如文件 I/O 和应用程序生命周期管理）提供了抽象接口，因此 AZ 框架代码位置是自然而然的选择。

AZ 框架输入接口使用`AZCore`事件总线系统，并定义以下类和事件总线：
+ `InputDevice` - 表示物理输入设备的基类。
+ `InputDeviceId` - 设备名称和设备索引，它们共同唯一标识`InputDevice`。
+ `InputDeviceNotificationBus` - 一个 EBus 接口，用于在输入设备连接或断开连接时订阅来自输入设备的事件。
+ `InputDeviceRequestBus` - 一个 EBus 接口，用于查询输入设备的关联输入通道或当前连接状态。
+ `InputChannel` - 表示输入数据的特定源（例如，鼠标左键）的基类。输入设备通常具有多个输入通道。
+ `InputChannelId` - 唯一标识 `InputChannel` 的名称。
+ `InputChannelNotificationBus` - 一个事件总线接口，当通道处于活动状态或者其状态或值发生变化时，该接口会订阅来自输入通道的事件。
+ `InputChannelRequestBus` - 一个 EBus 接口，用于从输入通道的 ID 获取输入通道。然后，您可以直接查询 input 通道的当前状态或值。
+ `InputChannelEventListener` - 继承自 `InputChannelNotificationBus` 但提供其他功能的事件监视器。订阅者可以使用其他功能按优先级顺序接收事件，或根据源设备或通道筛选事件。订阅者还可以使用事件，以便事件不会传递给优先级较低的监视器。

### 处理输入

要处理输入，您只需从 `AzFramework::InputChannelEventListener` 继承即可。您还可以创建一个过滤器，以便仅从特定输入设备或通道接收事件，然后覆盖 `OnInputChannelEvent` 来处理输入数据。

如果要直接查询当前输入状态，请使用 `AzFramework`::`InputChannelRequestBus` 获取输入通道的 ID。然后查询 input 通道的当前状态或值。

#### 文本输入接口

以下接口用于处理文本输入。输入以 UTF-8 码位的完整字符串形式提供。这样就无需跟踪和解释单个代码单元或从其他编码进行转换。
+ `InputTextEventNotificationBus` - 一个 EBus 接口，用于订阅来自输入设备或输入通道的文本事件。
+ `InputTextEntryRequestBus` - 发送文本输入请求的 EBus 接口。这些请求通知输入设备用户希望开始或停止输入文本。
+ `InputTextEventListener` - 继承自 `InputTextNotificationBus` 但提供其他功能的事件监视器。订阅者可以使用其他功能按优先级顺序接收事件或使用事件，以便事件不会传递给优先级较低的监视器。

#### 辅助输入接口

以下 AZ 框架输入辅助接口仅由某些输入设备实现。您可以使用这些接口来查询或发布与设备活动（如振动效果、运动传感器或鼠标光标）相关的数据。
+ `InputHapticFeedbackBus` - 一个 EBus 接口，用于向连接的输入设备发送触觉反馈请求。
+ `InputMotionSensorRequestBus` - 一个 EBus 接口，用于将运动传感器请求发送到连接的输入设备。
+ `InputSystemCursorRequestBus` - 一个 EBus 接口，用于查询或更改系统光标的状态、位置或外观。

### 输入设备

所有输入设备类都继承自`AzFramework::InputDevice`。虽然可以存在同一类的多个实例，但每个输入设备都由唯一的`InputDeviceId`标识。

#### O3DE 中包含的输入设备

基类的设计使您可以从它继承以实现新型的输入设备。但是，O3DE 已经包含以下设备的实现：


****

|设备类型 |支持的操作系统 |
|--- |--- |
|鼠标 |Windows、macOS |
|键盘 |Windows 和 macOS |
|游戏手柄 |Windows、macOS、iOS、Android |
|触摸 |iOS、Android |
|运动 |iOS、Android |
|虚拟键盘 |iOS、Android |
|虚拟现实控制器 |Oculus、OpenVR |

这组核心输入设备由`AzFramework::InputSystemComponent`管理。您可以使用此组件配置应用程序在启动时创建的输入设备的类型和数量。

#### 创建输入设备

您可以通过从`AzFramework::InputDevice`继承并根据需要创建或销毁新类的实例来创建和实现新类型的输入设备。

每个输入设备的实现细节因设备类型和操作系统而异。但是，几乎所有设备都遵循类似的模式。它们使用特定于操作系统的 API 来获取每帧的原始输入数据，然后相应地更新设备的所有关联输入通道。

当您实现输入设备时，请仅使用设备的`TickInput` 函数来更新输入通道的值。这可确保每帧的同一时间传递输入事件。

根据输入设备获取特定于操作系统的原始输入的方式，您可能必须使用以下技术之一：
+ 在每次调用输入设备的 `TickInput` 函数时轮询底层输入 API。 例如，见`\dev\Code\Framework\AzFramework\AzFramework\Input\Devices\Gamepad`目录中的所有`InputDeviceGamepad`实现。
+ 从应用程序的主消息循环对输入数据进行排队或合并，直到下次调用`TickInput`。例如，见`\dev\Code\Framework\AzFramework\AzFramework\Input\Devices\Mouse`目录中的所有`InputDeviceMouse`实现。
+ 确保在主线程上未收到原始 input 时以线程安全的方式更新 input 通道。有关示例，请参阅`\dev\Code\Framework\AzFramework\AzFramework\Input\Devices\Touch`目录中的`InputDeviceTouchAndroid`。

### 输入通道

输入设备可以有多个输入通道。每个通道都表示一个离散的输入数据源，该数据源由 `InputChannelId` 唯一标识。

输入通道使用`AzFramework::InputChannelNotificationBus::OnInputChannelEvent`来广播事件。当通道的状态或值从一帧更改为下一帧，或者通道仍处于活动状态或保持时，通道将广播事件。还可以直接访问输入通道，并随时轮询以查询其当前状态和值。

#### 创建 Input Channel

与`AzFramework::InputDevice`基类一样，可以从`AzFramework::InputChannel`继承来实现新类型的输入数据源。

#### O3DE 中包含的输入通道

O3DE 提供以下输入通道实现，供 [输入设备](#input-intro-devices) 部分中列出的设备使用。您还可以将这些实现用于新的输入设备。


****

|输入通道 |示例用法 |
| --- | --- |
| InputChannelAnalog | 游戏手柄触发器 |
| InputChannelAnalogWithPosition2D | 在某个位置用压力触摸 |
| InputChannelAxis1D | 游戏手柄控制杆 x 或 y |
| InputChannelAxis2D | 游戏手柄控制杆 x 和 y 一起 |
| InputChannelAxis3D | 运动传感器加速度、旋转或磁场 |
| InputChannelDelta | 鼠标滚轮 |
| InputChannelDeltaWithSharedPosition2D | 鼠标移动 |
| InputChannelDigital | 游戏手柄按钮或键盘键 |
| InputChannelDigitalWithPosition2D | 在某个位置无压力地触摸 |
| InputChannelDigitalWithSharedPosition2D | 鼠标按钮位于某个位置 |
| InputChannelQuaternion | 运动传感器方向 |
