---
linkTitle: Virtual Gamepad
title: Virtual Gamepad Gem
description: Virtual Gamepad Gem提供在 Open 3D Engine （O3DE） 项目的触摸屏设备上模拟游戏手柄的控件。
toc: true
---

使用 Virtual Gamepad Gem 在移动设备上为项目的 UI 提供触摸屏功能。启用 Virtual Gamepad Gem 后，您可以在 **UI Editor** 中将虚拟游戏手柄组件添加到项目的 UI 中。

Virtual Gamepad Gem 包括一个示例 UI 画布，您可以针对您的游戏进行自定义，也可以将其用作新 UI 画布的示例。要在 UI Editor 中查看此画布，请打开`Gems\VirtualGamepad\Assets\UI\Canvases\VirtualGamepad\VirtualGamepad.uicanvas`。

可以包含虚拟游戏手柄组件的活动 UI 画布的数量没有限制。这意味着您可以创建许多虚拟游戏手柄画布，以便在适当的场景中显示，甚至可以同时显示。例如，您可以在虚拟游戏手柄的每一半上显示不同的 UI 画布。

## 配置虚拟游戏手柄行为

您可以在 UI Editor 中配置虚拟游戏手柄的行为。要配置虚拟游戏手柄行为，请执行以下操作：

1. 在 UI Editor中，添加UI组件 **VirtualGamepadButton** 和 **VirtualGamepadThumbStick** 到 UI 画布。

1. 对于每个组件，选择 一个 **Input Channel**.。

![Input Channel selection for the VirtualGamepadButton](/images/user-guide/gems/gems-system-gem-virtualgamepad-2.png)

有关 O3DE 中输入的更多信息，请参阅 Open 3D Engine 中的输入。

## 虚拟游戏手柄组件属性

Virtual Gamepad Gem 具有两个组件，您可以使用它们来自定义移动游戏的输入：

**VirtualGamepadButton** 有一个属性。**Input Channel**。选择适当的输入。

![Input Channel selection for the VirtualGamepadButton.](/images/user-guide/gems/gems-system-gem-virtualgamepad-properties-1.png)

**VirtualGamepadThumbstick** 具有以下属性：

![VirutalGamepadThumbStick component properties](/images/user-guide/gems/gems-system-gem-virtualgamepad-properties-2.png)

## 在运行时显示虚拟游戏手柄

您可以通过创建加载虚拟游戏手柄 UI 画布的指令来为运行时启用虚拟游戏手柄。您可以使用 C++、Lua 或 Script Canvas 执行此操作。有关更多信息，请参阅 UICanvasManager 和 UICanvasComponent。

以下示例 Lua 脚本在检测到触摸屏支持时显示虚拟游戏手柄 UI 画布。

```lua
local touchDevice =
    InputDeviceRequestBus.Event.GetInputDevice(InputDeviceId(InputDeviceTouch.name))
if (touchDevice and touchDevice:IsSupported()) then
    self.virtualGamepadCanvasId = UiCanvasManagerBus.Broadcast.LoadCanvas("UI/Canvases/VirtualGamepad/virtualgamepad.uicanvas");
end
```

以下示例 Lua 脚本检查是否连接了物理游戏手柄。如果找到，Lua 脚本将禁用虚拟游戏手柄。

```lua
local gamepadDevice =
    InputDeviceRequestBus.Event.GetInputDevice(InputDeviceId(InputDeviceGamepad.name))
if (gamepadDevice and gamepadDevice:IsConnected()) then
    UiCanvasBus.Event.SetEnabled(self.virtualGamepadCanvasId, false);
end
```

以下 Lua 脚本在物理游戏手柄断开连接时启用虚拟游戏手柄，并在连接物理游戏手柄时禁用虚拟游戏手柄。

你可以在`lumberyard_version\dev\SamplesProject\AnimationSamples\Advanced_RinLocomotion\Scripts\Advanced_RinLocomotion.lua`文件中找到此Lua脚本的示例。

```lua
function Example:OnActivate()
    self.inputDeviceNotificationBus = InputDeviceNotificationBus.Connect(self);
end

function Example:OnInputDeviceConnectedEvent(inputDevice)
    if (inputDevice.deviceName == InputDeviceGamepad.name) then
        UiCanvasBus.Event.SetEnabled(self.virtualGamepadCanvasId, false);
    end
end

function Example:OnInputDeviceDisonnectedEvent(inputDevice)
    if (inputDevice.deviceName == InputDeviceGamepad.name) then
        UiCanvasBus.Event.SetEnabled(self.virtualGamepadCanvasId, true);
    end
end

function Example:OnDeactivate()
    if (self.inputDeviceNotificationBus) then
        self.inputDeviceNotificationBus:Disconnect();
    end
end
```
