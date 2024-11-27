---
linkTitle: 在运行时访问 UI 元素
description: ' 了解如何在运行时使用 Open 3D Engine （O3DE） 操作 UI 画布。 '
title: 在运行时访问 UI 元素
weight: 900
---

UI 画布加载后，您可以在运行时通过创建 [Script Canvas](/docs/user-guide/scripting/script-canvas/get-started) 图表、[Lua 脚本](/docs/user-guide/scripting/lua/ebus) 或直接从 C++ 使用 [事件总线](/docs/user-guide/programming/messaging/ebus) 与其 UI 元素进行通信。

## 与附加到其他 UI 元素的组件通信

只有在激活 UI 元素后，才能与 UI 元素进行通信。如果您将 Script Canvas 图形或 Lua 脚本附加到需要获取或设置另一个 UI 元素或其拥有的 UI 画布上的信息，则您的脚本应等待，直到所有 UI 元素激活并初始化其父级和 UI 画布引用。建议连接到 **UI 激活总线** 并等待 **游戏内激活后事件** 后再请求相关信息。

{{< important >}}
为确保 UI 元素在激活之前不被访问，请连接到 **UI 激活总线** 并订阅 **游戏内激活后事件**，该事件仅在所有 UI 元素激活并初始化其父级和 UI 画布引用后执行。
{{< /important >}}

以下示例显示了如何在附加到 UI 元素的 Lua 脚本中使用 UI 初始化总线。

```lua
function samplescript:OnActivate()
    -- Connect to the UI Initialization bus to receive events
    self.initializationHandler = UiInitializationBus.Connect(self, self.entityId);
end

function samplescript:InGamePostActivate()
    -- All UI elements have now been activated and their parent and UI canvas references initialized
    self.initializationHandler:Disconnect()
    self.StartButtonHandler = UiButtonNotificationBus.Connect(self, self.Properties.StartButton)
end
```

以下示例显示如何在附加到 UI 元素的 Script Canvas 图形中使用 UI 初始化总线。

![UI Activation Node](/images/user-guide/interactivity/user-interface/editor/ui-initialization-sc.png)
