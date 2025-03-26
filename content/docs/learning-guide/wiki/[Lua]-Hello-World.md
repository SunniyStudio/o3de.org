---
title: "[Lua]-Hello-World"
description: ""
toc: false
---

### Simple Print

As per tradition, here is a simple Lua component that prints "Hello, world!" to the console when it activates.

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
### Color Print

A neat tip you can use with `Debug.Log` is you can prepend `"$[0-9]"` to print to the console in various colors.
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
