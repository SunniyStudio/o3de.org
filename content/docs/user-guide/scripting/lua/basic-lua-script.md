---
linkTitle: Lua 脚本结构
description: Open 3D Engine 中 Lua 脚本的基本结构。
title: Lua 脚本的基本结构
toc: true
weight: 200
---

## 脚本表

用作组件的脚本包含一个 **Script Table**。Script Table 为脚本提供功能，并被视为一个类。Script Table 通常由以下部分组成：

+ 脚本表中的可选 **属性表**。Properties Table （属性表） 提供了一个界面，您可以使用该界面从编辑器中自定义脚本行为。

+ `OnActivate()` 引擎在激活具有脚本的实体时调用的函数。

+ `OnDeactivate()` 当具有脚本的实体被停用时由引擎调用的函数。

## 框架脚本

以下示例显示了一个框架脚本。

```lua
-- ScriptName.lua 

local ScriptName = 
{
    Properties =
    {
        -- Property definitions
    }
}

function ScriptName:OnActivate()
     -- Activation Code
end

function ScriptName:OnDeactivate()
     -- Deactivation Code
end

return ScriptName
```

## 实体表

对于每个 **Lua 脚本** 组件，O3DE 会创建一个名为 **实体表** 的表。引用脚本中的 Script Table 是 Entity Table 的元表。由于这种关系，当调用脚本中的任何方法时，`self` 参数（在大多数情况下是隐式的）引用 Entity Table。

然后，实体表具有以下可用的属性和方法：

+ 一个 **Properties** 表，从脚本表的 Properties Table 复制。在适当的情况下提供默认值。

+ 一个 `entityId` 属性，其中包含一个引用当前实体的 **EntityId** 类型的对象。
