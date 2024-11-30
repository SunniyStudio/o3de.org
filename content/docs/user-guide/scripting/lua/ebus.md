---
linkTitle: 在 Lua 中使用事件总线
title: 在 Lua 中使用事件总线
description: 编写 Lua 脚本，使用事件总线在 Open 3D Engine 中的组件之间进行通信。
toc: true
weight: 450
---

组件提供了允许脚本向它们发送信息并在发生某些操作时接收通知的接口。通过在 Lua 中创建两个不同的对象来建立通信：sender 和 handlers。发送方或处理程序是 [EBus](/docs/user-guide/programming/messaging/ebus) 的接口，后者是 O3DE 引擎中广泛使用的通信系统。创建发送方时，它可以调用函数，而函数又会将信息发送到组件。创建处理程序时，该组件会调用 Lua 脚本定义的某些函数。这些发送者和处理程序是使用实体 ID 创建的。您可以使用实体 ID 与附加到实体的组件进行通信，而不是运行脚本本身的实体。主脚本表始终提供一个名为`entityId`的字段，该字段包含脚本附加到的实体的 ID。其他实体 ID 可以通过`Properties`接口传递给脚本。

## 组件激活顺序

请记住以下有关 Lua 组件激活顺序的要点：
+ 在所有 C++ 组件都激活后激活 Lua 组件。
+ 如果一个实体有多个 Lua 组件，则无法保证首先激活哪个 Lua 组件。

## 与组件通信

当 Lua 脚本在 `OnActivate`函数中创建处理程序对象时，它会通知附加到实体的组件，当某些事件发生时，它应该调用脚本处理程序函数。随后，处理程序在`OnDeactivate`函数中显式断开连接并设置回 nil。这可确保在附加到脚本的实体未处于活动状态时不会浪费处理时间。只要实体处于活动状态，组件就会在适当的时间调用函数。

您可以从某些返回值的事件发送函数中请求信息。以下示例脚本使用`TransformBus` 获取实体的当前本地转换，并使用`GetLocalTM()`函数返回转换对象。此对象存储在主脚本表中的变量中。再次使用 `TransformBus` 将对象的转换重置为标识。

以下示例展示了如何使用 transform 总线。

```lua
function samplescript:OnActivate()

    -- Retrieve the object's local transform and store it for later use
    self.myOldTransform = TransformBus.Event.GetLocalTM(self.entityId)

    -- Reset the object's local transform to the identity matrix
    TransformBus.Event.SetLocalTM(self.entityId, Transform.CreateIdentity())
end
```

## 与附加到其他实体的组件通信

您还可以发送事件并创建处理程序，以便与附加到其他实体的组件进行通信。以下示例在 properties 表中定义父实体并请求其转换。这允许它将其 transform 设置为另一个实体的 transform。

```lua
local ParentScriptSample = {
    Properties = {
        ParentEntity = {default = EntityId()}
    }
}

function ParentScriptSample:OnActivate()
    if self.Properties.ParentEntity:IsValid() then
        self.entityBusHandler = EntityBus.Connect(self, self.Properties.ParentEntity)
    end
end

function ParentScriptSample:OnEntityActivated()
    local parentTransform = TransformBus.Event.GetLocalTM(self.Properties.ParentEntity)
    TransformBus.Event.SetLocalTM(self.entityId, parentTransform)
end

return ParentScriptSample
```

{{< important >}}
如果您有一个 Lua 脚本附加到需要从另一个实体获取信息的实体，则您的脚本必须订阅目标实体的  `OnEntityActivated` 事件。您的脚本应等待目标实体被激活，然后再请求相关信息。否则，您的脚本可能会返回 nil。
{{< /important >}}

## 使用 AZStd::vector 和 AZStd::array 

Lua 中的 vector 和 array 的行为与 table 非常相似，但有一些限制。vector 和 array 都具有以下功能。

**Length 运算符 `#`**
您可以通过在集合名称前加上长度运算符 `#` 来获取集合的长度，如以下示例所示。

```lua
#myCollection
```

**索引 \[\]**
若要获取集合中的元素，请在方括号中使用索引，如以下语法所示。索引是从 1 开始的，就像 Lua 表一样。

```lua
myCollection[index]
```

`Vector` 还有以下方法用于更改集合。

**push\_back**
使用 `push_back` 方法将元素追加到向量，如以下示例所示。

```lua
myCollection:push_back(5)
```

**pop\_back**
使用 `pop_back` 方法删除向量的最后一个元素，如以下示例所示。

```lua
myCollection:pop_back()
```

**clear**
使用 `clear` 方法从向量中删除所有元素，如以下示例所示。

```lua
myCollection:clear()
```

## 使用 AZStd::any 

你可以将任何 Lua 基元类型（不包括表）传递给任何将 `AZStd::any` 作为参数\(例如，`GameplayNotificationBus::OnEventBegin`\)。你也可以传递从 C++ 反映的任何类型的 \(例如，向量或`EntityId` \)的值。将值作为 `any` 传递不需要语法 - 只需调用总线或函数即可。

以下示例显示了 `AZStd::any` 的用法。

```lua
GameplayNotificationBus.Broadcast.OnEventBegin(self.eventId, "The value I'd like to pass to the handler")
```
