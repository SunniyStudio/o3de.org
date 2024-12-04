---
title: 在 Lua 中使用脚本事件
description: 了解如何在 Open 3D Engine （O3DE） 中将脚本事件与 Lua 结合使用。
weight: 300
---

Lua 脚本可以使用脚本事件相互通信。有两个示例脚本显示此通信，它们都位于`Gems\ScriptEvents\Assets\Scripts\Example`目录中。它们分别称为 `ScriptEvents_Addressable.lua` 和 `ScriptEvents_Broadcast.lua`。如果对事件总线进行寻址，则事件将发送到特定的地址 ID。全局广播的事件将在所有地址接收。有关更多信息，请参阅 [Open 3D Engine 事件总线 （EBus） 系统](/docs/user-guide/programming/messaging/ebus/)。

下表仅显示了一些可用的 return 和 input 值。它并不完整。

|类型 |语法 |输入 |返回 |
| --- | --- | --- | --- |
| string | typeid("") | yes | yes |
| number | typeid(0) | yes | yes |
| entity id | typeid(EntityId()) | yes | yes |

提示：如果你正在努力解决 input 和 return 限制，请考虑使用 JSON/字符串序列化器/反序列化器。

---

### ScriptEvents_Addressable.lua

`ScriptEvents_Addressable.lua`示例脚本为脚本事件实现一个处理程序，该处理程序需要一个地址才能调用处理程序。它广播一个方法，但只有连接到与事件中指定的地址匹配的地址的处理程序才能调用它。

**示例 1：一个包含结果验证的 Lua 文件**

```lua
-- ScriptEvents_Addressable.lua
function ScriptTrace(txt)
    Debug.Log(txt)
end

function ScriptExpectTrue(condition, msg)

    if (not condition) then
        ScriptTrace(msg)
    end

end

-- This example shows how to implement a handler for a Script Event that requires an address
-- in order for a handler to be invoked.
luaScriptEventWithId = {

    -- This method will be broadcast, but only handlers connected to the matching address
    -- as the one specified in the event will invoke it.
    MethodWithId0 = function(self, param1, param2)
        ScriptTrace("Handler: " ..  tostring(param1) .. " " .. tostring(param2))

        ScriptExpectTrue(typeid(param1) == typeid(0), "Type of param1 must be "..tostring(typeid(0)))
        ScriptExpectTrue(typeid(param2) == typeid(EntityId()), "Type of param2 must be "..tostring(typeid(EntityId())))
        ScriptExpectTrue(param1 == 1, "The first parameter must be 1")
        ScriptExpectTrue(param2 == EntityId(12345), "The received entity Id must match the one sent")

        ScriptTrace("MethodWithId0 handled")

        return true
    end,

    MethodWithId1 = function(self)
        ScriptTrace("MethodWithId1 handled")
    end
}

-- "Script_Event" will be the name of the callable Script Event. It requires the address type to be a string.
local scriptEventDefinition = ScriptEvent("Script_Event", typeid("")) -- Event address is of string type

-- Define some methods for handlers to implement.
local method0 = scriptEventDefinition:AddMethod("MethodWithId0", typeid(false)) -- Return value is Boolean
method0:AddParameter("Param0", typeid(0))
method0:AddParameter("Param1", typeid(EntityId()))

-- NOTE: Types are specified using the typeid keyword with a VALUE of the type you want. For example, typeid("EntityId")
-- produces the type id for a string, not the EntityId type.

scriptEventDefinition:AddMethod("MethodWithId1") -- No return, no parameters.

-- After the Script Event is defined, call Register to enable it. Typically this should be done within the OnActivate method.
scriptEventDefinition:Register()

-- At this point, the script event is usable. Use the Script_Event.Connect method to connect a handler to it. The following example
-- uses luaScriptEventWithId as the handler that implements the methods defined earlier. Notice that the call connects using
-- the string "ScriptEventAddress" as the address for this event. Methods sent to a different address will not be handled by
-- this handler.
scriptEventHandler = Script_Event.Connect(luaScriptEventWithId, "ScriptEventAddress")

-- Now invoke the event and specify "ScriptEventAddress" as the address. The handler previously
-- connected will be able to handle this event.
local returnValue = Script_Event.Event.MethodWithId0("ScriptEventAddress", 1, EntityId(12345))

-- "Method0" should return true. Verify the result.
ScriptExpectTrue(returnValue, "Method0's return value must be true")

-- Finally, send MethodWithdId1. This method does not require any parameters, but does require the address.
Script_Event.Event.MethodWithId1("ScriptEventAddress")
```

下一个示例重点介绍了一个 Lua 脚本中 Script Event 方法/函数定义之间的逻辑分离;以及如何在另一个 Lua 脚本中调用它。Addressable Script Event 是一个不错的选择，当你需要在两个不同的 Lua 脚本之间进行通信时。换句话说，一对一 （1：1） 关系。

**示例 2：在一个 Lua 脚本中定义脚本事件;在另一个 Lua 脚本中调用它 （1：1）**

```lua
-- -- This is the first Lua file. This is where you define the Addressable Script Event and respective method(s)

-- -- just a function/method
function ScriptTrace(txt)
    Debug.Log(txt);
end

-- -- table with the method/function
luaScriptEventWithId =
{
    MethodWithId0 = function(self, param1, param2)
        ScriptTrace("MethodWithId0 handled");
        return true;
    end
}

-- -- When this Lua script is first activated, it will define the Script Event
function OnActivate()

    -- -- Script Event name = "Script_Event"! Address type = string, typeid("")
    local scriptEventDefinition = ScriptEvent("Script_Event", typeid(""));
    
    -- -- Custom method name = "MethodWithId0"! Return value = Boolean, typeid(false)
    local method0 = scriptEventDefinition:AddMethod("MethodWithId0", typeid(false));
    -- -- Input pameter name "Param0" of type number, typeid(0)
    method0:AddParameter("Param0", typeid(0));
    -- -- Input pameter name "Param1" of type Id, typeid(EntityId())
    method0:AddParameter("Param1", typeid(EntityId()));
    
    -- -- Register to enable the Script Event
    scriptEventDefinition:Register();
    
    -- -- Connect using a string address: "ScriptEventAddress"
    scriptEventHandler = Script_Event.Connect(luaScriptEventWithId, "ScriptEventAddress");
end

-- -- Optional. Disconnect when this Lua Script Deactivates
function OnDeactivate()
    -- scriptEventHandler:Disconnect();
end

```

```lua
-- -- This is the second Lua file. This is where you invoke/call/consume/get-return-value
-- -- NOTE: In some UI cases (uicanvas), you should avoid calling the Script Event inside the OnActivate function.

-- -- "ScriptEventAddress" is the address you defined earlier
function FunctionInAnotherLuaFile()
    local returnValue = Script_Event.Event.MethodWithId0("ScriptEventAddress", 1, EntityId(12345));
end

```

---

### ScriptEvents_Broadcast.lua

`ScriptEvents_Broadcast.lua` 示例脚本实现广播脚本事件的处理程序。由于广播脚本事件不指定地址类型，因此任何处理程序都可以连接到它们。

**示例 3：一个包含结果验证的单个文件**

```lua
-- ScriptEvents_Broadcast.lua
function ScriptTrace(txt)
    Debug.Log(txt)
end

function ScriptExpectTrue(condition, msg)

    if (not condition) then
        ScriptTrace(msg)
    end

end

-- This example shows how to implement a handler for a broadcast script event.
-- Because broadcast script events do not specify an address type, they can be handled
-- by simply connecting to them.
luaScriptEventBroadcast = {

    -- The following method is called as a result of a broadcast call on the script event.
    BroadcastMethod0 = function(self, param1, param2)
        ScriptTrace("Handler: " ..  tostring(param1) .. " " .. tostring(param2))

        ScriptExpectTrue(typeid(param1) == typeid(0), "Type of param1 must be "..tostring(typeid(0)))
        ScriptExpectTrue(typeid(param2) == typeid(EntityId()), "Type of param2 must be "..tostring(typeid(EntityId())))
        ScriptExpectTrue(param1 == 2, "The first parameter must be 2")
        ScriptExpectTrue(param2 == EntityId(23456), "The received entity Id must match the one sent")

        ScriptTrace("BroadcastMethod0 Called")

        return true
    end,

    BroadcastMethod1 = function(self)
        ScriptTrace("BroadcastMethod1 Called")
    end
}

local scriptEventDefinition = ScriptEvent("Script_Broadcast") -- Script_Broadcast is the name of the callable script event.

-- Define methods for Script_Broadcast
local method0 = scriptEventDefinition:AddMethod("BroadcastMethod0", typeid(false)) -- To add a method, provide a method name and an optional return type.
method0:AddParameter("Param0", typeid(0))
method0:AddParameter("Param1", typeid(EntityId()))

-- NOTE: Types are specified using the typeid keyword with a VALUE of the type you want. For example, typeid("EntityId")
-- produces the type id for a string, not the EntityId type.

scriptEventDefinition:AddMethod("BroadcastMethod1")

-- After the Script Event is defined, call Register to enable it. Typically this should be done within the OnActivate method.
scriptEventDefinition:Register()

-- At this point, the script event is usable. Use the Script_Broadcast.Connect method to connect a handler to it. The following example
-- uses luaScriptEventWithId as the handler that implements the methods defined earlier.
scriptEventHandler = Script_Broadcast.Connect(luaScriptEventBroadcast)

-- To test the event, broadcast BroadcastMethod0. As defined, the method expects two parameters and returns a Boolean value.
local returnValue = Script_Broadcast.Broadcast.BroadcastMethod0(2, EntityId(23456))

-- BroadcastMethod0 should return true. Verify the result.
ScriptExpectTrue(returnValue, "BroadcastMethod0's return value must be true")

-- To broadcast an event without a return or parameters, invoke BroadcastMethod1.
Script_Broadcast.Broadcast.BroadcastMethod1()
```

下一个示例重点介绍了一个 Lua 脚本中 Script Event 方法/函数定义之间的逻辑分离;以及如何在多个/不同的 Lua 脚本中调用它。当您需要在两个以上的 Lua 脚本之间进行通信时，广播脚本事件是一个不错的选择。换句话说，一对多 （1：M） 关系。

**示例 4：在一个 Lua 脚本中定义脚本事件;在其他 Lua 脚本中调用它 （1：M）**

```lua
-- -- This is the first Lua file. This is where you define the Broadcast Script Event and respective method(s)

-- -- just a function/method
function ScriptTrace(txt)
    Debug.Log(txt);
end

-- -- table with the method/function
luaScriptEventBroadcast =
{
    BroadcastMethod0 = function(self, param1, param2)
        ScriptTrace("BroadcastMethod0 Called"):
        return true;
    end
}

-- -- When this Lua script is first activated, it will define the Script Event
function OnActivate()

    -- -- Script Event name = "Script_Broadcast"! Note that there is NO ADDRESS
    local scriptEventDefinition = ScriptEvent("Script_Broadcast");
    
    -- -- Custom method name = "BroadcastMethod0"! return value = Boolean, typeid(false)
    local method0 = scriptEventDefinition:AddMethod("BroadcastMethod0", typeid(false));
    -- -- Input parameter name "Param0" of type number, typeid(0)
    method0:AddParameter("Param0", typeid(0));
    -- -- Input parameter name "Param1" of type id, typeid(EntityId())
    method0:AddParameter("Param1", typeid(EntityId()));
    
    -- -- Register to enable the Script Event
    scriptEventDefinition:Register();
    
    -- -- Connect
    scriptEventHandler = Script_Broadcast.Connect(luaScriptEventBroadcast, self.entityId);
    -- scriptEventHandler = Script_Broadcast.Connect(luaScriptEventBroadcast);
end

-- -- Optional. Disconnect when this Lua Script Deactivates
function OnDeactivate()
	  -- scriptEventHandler:Disconnect();
end

```


```lua
-- -- This is a second Lua file. This is one of the files that can invoke/call/consume/get-return-value
-- -- NOTE: In some UI cases (uicanvas), you should avoid calling the Script Event inside the OnActivate function.

-- -- NO ADDRESS needed
function FunctionInAnotherLuaFile2()
    local returnValue = Script_Broadcast.Broadcast.BroadcastMethod0(2, EntityId(23456))
end

```

```lua
-- -- This is the third Lua file. This is another one of the files that can invoke/call/consume/get-return-value
-- -- NOTE: In some UI cases (uicanvas), you should avoid calling the Script Event inside the OnActivate function.

-- -- NO ADDRESS needed
function FunctionInAnotherLuaFile3()
    local returnValue = Script_Broadcast.Broadcast.BroadcastMethod0(3, EntityId(234567))
end

```

