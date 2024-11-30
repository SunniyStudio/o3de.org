---
linkTitle: 调试 Lua 脚本
title: 调试 Lua 脚本
description: 了解如何在 Open 3D Engine 中调试 Lua 脚本。
toc: true
weight: 850
---

O3DE 为 Lua 脚本提供了多种功能，使调试更加容易。

## 记录到控制台

要将文本打印到 **O3DE 编辑器** 和控制台，请使用`Debug.Log()` 函数。

以下示例显示了 `Debug.Log()` 函数的用法。

```lua
local LoggingTest = { }
function LoggingTest:OnActivate()
    componentName = "MyComponent"
    Debug.Log(componentName .. " has been activated.")
end

return LoggingTest
```

## 使用 assert 检测潜在问题

当检测到可能导致执行错误的条件时，您可以使用 `assert` 函数在控制台中显示错误消息。`assert`函数有两个参数：一个计算结果为 true 或 false 的条件，以及如果条件为 false，则显示一条消息。

以下示例显示了`assert` 函数的用法。

```lua
function SampleScript:DoStuff()
    -- This value should never be negative
    assert( self.positiveValue >= 0, "Expected a positive value! Got: " .. self.positiveValue )
end

-- Console output when the value of self.positiveValue is -5:
-- [Error] Lua error (2 - [string "q:/lyengine/branches/systems/dev/samplespro..."]:61: Expected a positive value! Got: -5) during call samplescript:DoStuff
```

## 传达错误

您可以使用 `Debug.Error()` 函数在控制台中显示错误并停止执行当前脚本函数。这不会停止脚本的所有执行。如果您有活动的处理程序，则仍可在引擎发布通知时调用它们。`Debug.Error()`函数采用类似于 `Debug.Assert`函数的参数：条件和消息。该消息以亮红色显示，并且仅当条件为 false 时，执行才会停止。

以下示例显示了`Debug.Error()`函数的用法。

```lua
function SampleScript:CheckAndError()
    -- This value should never be negative
    Debug.Error( self.positiveValue >= 0, "Detected a negative value: " .. self.positiveValue )
end

-- Console output when the value of self.positiveValue is -5:
-- [Error] Error on argument 0: Detected a negative value: -5
```

## 在需要用户注意时显示警告

可能会出现不会对脚本执行产生负面影响但可能有助于用户了解的脚本条件。`Debug.Warning()`函数使用的参数类似于 `Error` 和 `Assert`函数的参数，但只在控制台中显示橙色警告消息。它不会停止执行。

以下示例显示了`Debug.Warning()`函数的用法。

```lua
function SampleScript:CheckValue()
    -- This value should probably never be negative
    Debug.Warning( self.positiveValue >= 0, "Detected a negative value: " .. self.positiveValue )
end

-- Console output when the value of self.positiveValue is -5:
-- [Warning] Warning on argument 0: Detected a negative value: -5
```
