---
title: "[Lua]-Hello-World"
description: ""
toc: false
---

### 简单打印

按照传统，这里有一个简单的 Lua 组件，当它激活时，它会将 “Hello， world！” 打印到控制台。

```lua
local HelloWorld = {}

function HelloWorld:OnActivate()
  Debug.Log("Hello, world!")
end

function HelloWorld:OnDeactivate()
end

return HelloWorld
```
![simple print](https://i.ibb.co/pW4TkkD/simple-print.png)

***
### 彩色打印

你可以用在`Debug.Log`中的一个巧妙的提示是，你可以在`"$[0-9]"`前面加上各种颜色打印到控制台。

```lua
local HelloWorld = {}

function HelloWorld:OnActivate()
  for i = 0, 9 do
    Debug.Log("$" .. i .. "Hello, world! " .. i)
  end
end

function HelloWorld:OnDeactivate()
end

return HelloWorld
```
![color print](https://i.ibb.co/WfG0SVX/color-print.png)
