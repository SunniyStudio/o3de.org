---
linkTitle: Gestures
title: Gestures Gem
description: Gestures Gem为 Open 3D Engine （O3DE） 中常见的基于手势的输入操作提供检测。
toc: true
---

您可以使用 Gestures Gem 来识别常见的基于手势的输入，包括以下内容：

* **轻敲** 或 **点击** - 单点触控、离散手势
* **拖拽** 或 **平移** - 单点触控、连续手势
* **按住** 或 **按下** - 单点触控、连续手势
* **滑动** - 单点触控，离散手势
* **捏合** - 多点触控、连续手势
* **旋转** - 多点触控、连续手势

可以通过触摸或鼠标输入来检测单点触控手势（例如点击、拖动、按住和滑动）。但是，多点触控手势（例如捏合和旋转）只能在支持多点触控的设备（如 iOS 或 Android）上识别。您可以扩展基础 C++ 手势识别框架以支持您的自定义手势识别器。

在项目中启用 Gem 以自动开始接收您可以响应的手势输入事件。

{{< note >}}
目前只能在 Gem 的源中配置手势识别器。没有可用于修改它们的 UI。
{{< /note >}}

## 响应手势输入

**Gestures** 系统组件公开的每个手势识别器对应于属于手势输入设备的手势输入通道。

您可以像使用 C++、Lua 或 Script Canvas 中的其他输入通道一样使用手势输入通道。您可以使用 **Input** 组件将手势输入通道映射到游戏操作。

要向实体添加输入，请参阅 [Input 组件](/docs/user-guide/components/reference/gameplay/input)。

**Lua 脚本示例**
以下脚本侦听并响应默认的双击手势。

```lua
function GestureExample:OnActivate()
    self.inputChannelNotificationBus = InputChannelNotificationBus.Connect(self);
end

function GestureExample:OnInputChannelEvent(inputChannel)
    if (inputChannel.channelName == InputDeviceGestures.gesture_double_press) then
        -- Respond to the default double press gesture
    end
end

function GestureExample:OnDeactivate()
    if (self.inputChannelNotificationBus) then
        self.inputChannelNotificationBus:Disconnect();
    end
end
```

有关详细信息，请参阅 [Open 3D Engine 中的输入](/docs/user-guide/interactivity/input/)。
