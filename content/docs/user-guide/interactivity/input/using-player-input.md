---
linkTitle: 使用用户输入
title: 在 Open 3D Engine 中使用玩家输入
description: 有关使用 Open 3D Engine （O3DE） 资产编辑器以及 Script Canvas 或 Lua 配置播放器输入的说明。
---

您可以为 **Input** 组件创建输入绑定资产，并使用 **Open 3D Engine （O3DE） Asset Editor **指定输入事件。使用 Script Canvas 或 Lua 将输入事件映射到游戏逻辑。

## 创建输入绑定资产

要使用 Asset Editor 创建 `.inputbindings` 文件，请执行以下步骤。

1. 在 **O3DE 编辑器 ** 中，创建一个实体。

1. 在视口或**Entity Outliner**中选择实体。

1. 在 **Entity Inspector** 中，点击 **Add Component**，然后添加一个Input组件。

1. 在 Entity Inspector 中，找到Input组件，然后在Asset Editor中点击 {{< icon "open-in-internal-app.svg" >}} 创建新的 `.inputbindings` 文件。

	![The default Input component](/images/user-guide/interactivity/input/input-component.png)

### 创建输入事件组

您可以将不同操作的输入事件组添加到新的 `.inputbindings` 文件中。

![Adding Input Event Groups](/images/user-guide/interactivity/input/new-inputbindings-asset.png)

1. 要创建新的输入事件组，请点击{{< icon "add.svg" >}} 图标。

1. 对于 **Event Name**，为事件输入名称，例如 `Action`。


### 创建事件生成器

创建输入事件组后，您可以向该组添加事件生成器。事件生成器是生成命名事件的处理程序。例如，按下的键、按住的鼠标按钮或游戏控制器上的一系列操作都会产生命名事件。

![Adding Event Generators](/images/user-guide/interactivity/input/new-inputbindings-asset-2.png)

1. 在新的输入事件组中，点击**Event Generators** 旁的 {{< icon "add.svg" >}} 图标。

1. 在 **Class to create** 对话框中，选择 `InputEventMap`，然后选择 **OK**。

1. 指定事件生成器的更改。每个事件生成器都有一组可以自定义的属性。

### 设置事件生成器属性

以下 `.inputbindings`文件为 **Input Device Type** 指定键盘，为 **Input Name** 指定空格键。

   ![Input bindings configuration example in Asset Editor.](/images/user-guide/interactivity/input/inputbindings-example.png)

### 保存输入绑定资产

1. 在 **Asset Editor**中，选择 **File**, **Save**.

1. 为`.inputbindings` 文件输入一个名称，然后选择 **Save**.

1. 在 Entity Inspector 中，在 Input 组件中，对于 **Input to event bindings**, 点击 {{< icon "browse-edit-select-files.svg" >}},，然后选择你的 `.inputbindings` 文件。

## 将输入事件映射到 Gameplay 逻辑

创建 `.inputbindings` 文件并指定输入事件后，您可以使用 Script Canvas 或 Lua 将输入事件映射到游戏逻辑。您可以使用 **Script Canvas 编辑器** 在可视化脚本环境中创建游戏逻辑，或使用 **Lua 编辑器** （Lua IDE） 编写您自己的 Lua 脚本。

### 在 Script Canvas 中使用输入

您可以创建连接到输入事件的 Script Canvas 图表。有关使用 Script Canvas 编写脚本的更多信息，请参阅 [使用 Script Canvas 创建游戏和其他行为](/docs/user-guide/scripting/script-canvas)。

1. 在 O3DE 编辑器中，在视口或 **Entity Outliner**中选择带有Input组件的实体。

1. 在 Entity Inspector 中，点击 **Add Component** 并添加 **Script Canvas** 组件。

1. 在 Script Canvas 组件中，对于 **Script Canvas Asset**，点击 {{< icon "open-in-internal-app.svg" >}}，然后创建一个新的 Script Canvas 图表，如以下示例所示。

### 示例 Script Canvas 图表

在以下图表中， **InputHandler** 节点连接 `Action` 事件到各个 **Transform** 节点。当事件生成器的状态更改时，Input组件发送 **Pressed**, **Held**, 和 **Released** 事件到 **InputHandler** 节点。

![Example Script Canvas graph for the Input component](/images/user-guide/interactivity/input/sc-input-example.png)

### 在Lua中使用输入 

您还可以创建连接到输入事件的 Lua 脚本。有关使用 Lua 编写脚本的更多信息，请参阅 [编写 Lua 脚本](/docs/user-guide/scripting/lua).

1. 在 O3DE 编辑器中，在视口或 **Entity Outliner**中选择带有Input组件的实体。

1. 在 Entity Inspector 中，点击 **Add Component** 并添加 **Lua Script** 组件。

1. 在Lua Script组件中，点击 {{< icon "open-in-internal-app.svg" >}}.

1. 在 Lua 编辑器中，为想要连接脚本的输入事件添加 `OnPressed`, `OnHeld`, 和 `OnReleased`函数的表。 

1. 通过连接到`InputEventNotificationBus`来创建总线处理程序，并将第一个参数设置为输入事件的函数表。第二个参数是 **Event Name**的`InputEventNotificationId`。

### Lua 脚本示例

以下 Lua 脚本将 `Action` 事件连接到各个函数`OnPressed`, `OnHeld`, 和 `OnReleased`。

```lua
local tutorial_input = {
    Properties = {}
}
function tutorial_input:OnActivate()
    self.ActionInputs = {}

    self.ActionInputs.OnPressed = function(_, value)
        TransformBus.Event.SetLocalUniformScale(self.entityId, 2.0)
    end

    self.ActionInputs.OnHeld = function(_, value)
        TransformBus.Event.RotateAroundLocalZ(self.entityId, 0.01)
    end

    self.ActionInputs.OnReleased = function(_, value)
        TransformBus.Event.SetLocalUniformScale(self.entityId, 1.0)
    end

    self.ActionInputHandler = InputEventNotificationBus.Connect(self.ActionInputs, InputEventNotificationId("Action"))
end

function tutorial_input:OnDeactivate()
    self.ActionInputHandler:Disconnect()
end

return tutorial_input
```

## 测试玩家输入

使用 Script Canvas 或 Lua 将`.inputbindings`文件中的输入事件连接到输入处理程序后，您可以在 O3DE 编辑器中测试播放器输入。

1. 在 O3DE Editor 中，在视区或 **Entity Outliner** 中选择具有 Input 和 scripting （输入和脚本） 组件的实体。

    {{< note >}}
在此示例中，实体必须具有在游戏中启用可见性的组件，例如 [Mesh组件](/docs/user-guide/components/reference/atom/mesh)、[White Box 组件](/docs/user-guide/components/reference/shape/white-box) 或 [Shape组件](/docs/user-guide/components/reference/shape)之一。
{{< /note >}}

1. 在 Entity Inspector中，点击**Add Component**，然后添加**Mesh** 组件

1. 在 **Mesh asset**中，指定网格资源文件。这将为您的实体提供在 Game Mode 中可见的形状。

1. 要进入游戏模式，按下**Ctrl+G**。

1. 按 **空格键** 以便您的实体在局部 z 轴上旋转。当从连接到 `Action` 的输入处理程序接收到事件`OnPressed` 时，实体的比例会加倍。当收到 `OnReleased` 事件时，实体的缩放将恢复正常。

1. 要退出游戏模式，按下 **Esc**.
