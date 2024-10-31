---
description: ' 在 Open 3D Engine 中使用Input 组件 EBus（事件总线）。 '
title: Input 组件 EBus 接口
---



输入子组件是与组件具有相同生命周期的对象，必须覆盖 `Activate` 和 `Deactivate`。

```
//////////////////////////////////////////////////////////////////////////
// InputSubComponent
void Activate(const AZ::InputEventNotificationId& channelId) override;
void Deactivate(const AZ::InputEventNotificationId& channelId) override;
```

您可以使用 `GameplayNotificationBus`与 `InputSubComponent` 协同工作。你可以在`Gems\StartingPointInput\Assets\Scripts\Input` 目录中找到示例 Lua 脚本和代码。

## Input时间通知总线 

通过事件通知总线接口使用以下通知功能与游戏的其他组件进行通信。

有关使用事件总线（EBus）接口的更多信息，请参阅 [使用事件总线（EBus）系统](/docs/user-guide/programming/messaging/ebus/)。

###  


****

| 请求名称 | 说明 | 参数 | 返回值 | 可脚本化 |
| --- | --- | --- | --- | --- |
| OnPressed |  当输入超过阈值时发出的事件。  | Float | None | Yes |
| OnHeld |  当输入持续超过阈值时发出的事件。  | Float | None | Yes |
| OnReleased |  当输入不再超过阈值时发出的事件。  | Float | None | Yes |

**示例**

```
local held =
{
    Properties =
    {
        IncomingInputEventName = "",
        OutgoingGameplayEventName = "",
    },
}

function held:OnActivate()
    local inputBusId = InputEventNotificationId(self.Properties.IncomingInputEventName)
    self.inputBus = InputEventNotificationBus.Connect(self, inputBusId)
end


function held:OnHeld(floatValue)
    GameplayNotificationBus.Event.OnEventUpdating(GameplayNotificationId(self.entityId, self.Properties.OutgoingGameplayEventName), floatValue)
end

function held:OnReleased(floatValue)
    GameplayNotificationBus.Event.OnEventEnd(GameplayNotificationId(self.entityId, self.Properties.OutgoingGameplayEventName), floatValue)
end

function held:OnDeactivate()
    self.inputBus:Disconnect()
end

return held
```

## Input 请求总线 

使用输入请求总线接口的下列功能可与游戏的其他组件进行通信。

有关使用事件总线（EBus）接口的更多信息，请参阅 [使用事件总线（EBus）系统](/docs/user-guide/programming/messaging/ebus/)。


****

| 请求名称 | 说明 | 参数 | 返回值 | 可脚本化 |
| --- | --- | --- | --- | --- |
| PushContext |  将新的上下文推入堆栈，成为活动上下文。  | String | None | Yes |
| PopContext |  从输入上下文堆栈中移除顶层上下文。 | None | None | Yes |
| PopAllContexts |  清除上下文堆栈，活动上下文变为空字符串： `""`  | None | Empty string | Yes |
| GetCurrentContext |  返回堆栈顶部的上下文。如果堆栈为空，则返回： `""`  | None | String | Yes |

**输入上下文**示例
您可以使用**Input contexts**参数来指定可用的资产绑定上下文。例如，您可以切换上下文，使角色在水下时的输入事件与角色在地面上时的输入事件不同。
例如，对于**Input**组件，您可以指定**Input contexts**，如空字符串、`"under water"`和`"run"`。在实体中添加**Lua**组件时，您可以在 Lua 脚本中指定不同的**Input contexts**、`"under water"`, 和 `"run"`。
这意味着当您的 Lua 脚本运行时，Lua 脚本会告诉 **Input** 组件使用哪种上下文。

```
local foo
{
    Properties =
    {
        Context {default = "", description = "A context to push onto the input stack."},
    }
}

function foo:OnActivate()
    -- by default the context is blank ""
    InputRequestBus.Broadcast.PushContext(self.Properties.Context) -- context stack is now 1)user defined property
    InputRequestBus.Broadcast.PushContext("under water") -- context stack is now 1)user defined property, 2) "under water"
    Debug.Log(InputRequestBus.Broadcast.GetCurrentContext()) -- prints "under water"
    InputRequestBus.Broadcast.PushContext("run") -- context stack is now 1)use defined property, 2) "under water", 3) "run"
    InputRequestBus.Broadcast.PopContext() -- context stack is now 1)user defined property, 2) "under water"
    InputRequestBus.Broadcast.PopAllContexts() context stack is now empty
end

return foo
```
