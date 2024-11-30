---
linkTitle: Lua 环境
title: Lua 环境 (高级)
description: 了解如何在 Open 3D Engine Lua 环境中添加 ScriptContext 实例和使用通用代码。
toc: true
weight: 700
---

默认情况下，O3DE 组件实体 Lua 环境是单个 Lua 环境\(或 `lua_State`\)。此环境绑定到 `ComponentApplication` 拥有的`BehaviorContext`。因此，它有权访问启动时反映的所有 API 操作。

## 添加其他 VMs 

你可以使用 `ScriptSystemBus` \(调用`AddContextWithId`，或者创建自己的实例并调用`AddContext`\) 添加更多的 `ScriptContext` 实例。如果希望新上下文可用于调试，则必须使用 `ScriptDebugAgentBus::RegisterContext`注册它。

## 重用代码

Lua 提供了使用内置的 Lua [require](https://www.lua.org/pil/8.1.html)函数从其他 Lua 文件加载和执行脚本的功能。请务必注意，此函数需要特殊的 path 格式。文件路径由句点而不是斜杠分隔，没有`.lua`文件扩展名，并且相对于 O3DE 资产目录。例如，如果您想使用  `require`函数为脚本提供项目 `Scripts` 目录中的一些常用功能，则可以使用类似于以下示例的代码。

```lua
local library = require("Scripts.MyLibraryFile")
```
