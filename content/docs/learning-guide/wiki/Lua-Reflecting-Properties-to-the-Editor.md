---
title: "[Lua]-Reflecting-Properties-to-the-Editor"
description: ""
toc: false
---

本教程是 _Using Transform Bus_ [此处](https://github.com/o3de/o3de/wiki/%5BLua%5D-Using-Transform-Bus) 的延续. 

### 当前问题

提醒一下，这是我们目前拥有的`ConstantRotate`脚本的完整 Lua 实现：

```lua
local ConstantRotate = {}

function ConstantRotate:OnActivate()
  self.tickHandler = TickBus.Connect(self)
end

function ConstantRotate:OnDeactivate()
  self.tickHandler:Disconnect()
  self.tickHandler = nil
end

function ConstantRotate:OnTick(deltaTime, timePoint)
  TransformBus.Event.RotateAroundLocalZ(self.entityId,  deltaTime)
end

return ConstantRotate
```

这里有两个主要的可用性问题：
1. 每当我们想更改旋转速度时，我们都必须打开 Lua 脚本并更改值
2. 使用此 Lua 组件的每个实体都将具有相同的值

这两者都可以通过使用 Lua 中的 `Properties` 表来解决

***

### Properties Table 简单示例

`Properties` 表允许我们在 Lua 脚本中使用每个组件实例的值。这是一个简单的示例，它允许我们打印我们选择的字符串，而无需更改 Lua 脚本，而只需停留在 O3DE 编辑器中即可。

```lua
local SimpleReflect = 
{
  Properties =
  {
    MyString = {default = "Default message!", description = "The message to print on activation."}
  }
}

function SimpleReflect:OnActivate()
  Debug.Log(self.Properties.MyString)
end

function SimpleReflect:OnDeactivate()
end

return SimpleReflect
```

#### 默认设置

![default component](https://i.ibb.co/DLcG73t/defaultreflect.png)

![default message](https://i.ibb.co/5r1rYzR/defaultlog.png)

#### 编辑的设置

现在，我们只需在 Editor 中更改该值，Lua 脚本就会相应地更新。

![edited component](https://i.ibb.co/hRyPG2s/edited-Reflect.png)

![edited message](https://i.ibb.co/DGvSVnf/edited-Log.png)

***

### 使用属性表

要开始将数据反映到编辑器，您需要做的就是在声明（伪）Lua 组件时包含一个 `Properties` 表，如下所示：

```lua
local MyLuaComponent =
{
  Properties =
  {

  }
}
```

之后，您可以在 O3DE 中反映任何基本的 Lua 类型或反射类。我将在名为 _C++脚本绑定_ 的高级主题中详细讨论反射类。您还可以根据需要自由嵌套数据。反射的数据可以使用以下关键字：

* `default`
* `description`
* `suffix` 用于数字
* `prefix` 用于数字
* `min` 用于数字
* `max` 用于数字
* `step` 用于数字

数据列表将转换为数组。如果不需要，也可以省略任何关键字和封闭大括号。以下是一些示例，显示了将数据反映到编辑器的不同方法：

#### String, Number, Boolean, Array

```lua
local SimpleReflect = 
{
  Properties =
  {
    MyString = "Hello",
    MyNumber = 19.0,
    MyBool = true,
    MyArrayString = {"One", "Two", "Three"},
    MyArrayNumber = {1, 2, 3},
    MyArrayBool = {true, false, true},
    --[[
    Cannot mix types in arrays!
    MyArrayInvalid = {"One", 2.0, false}
    --]]
  }
}
```

![SimpleReflectExample](https://i.ibb.co/0QJW0Nk/simple.png)

#### 使用关键字和分组属性

您公开的属性将自动按名称进行组织。如果要对属性进行分组，则可以通过嵌套它们来实现。

```lua
local SimpleReflect = 
{
  Properties =
  {
    Character =
    {
      CharacterSpeed = {default = 5.0, min = 0.0, max = 20.0, description = "The movement speed in meters per second", suffix = " m/s"},
      CharacterPosition = {default = Vector3(0, 0, 0), description = "The starting position of the character"},
    },
    Camera =
    {
      CameraSpeed ={default = 2.0, min = 0.0, max = 5.0, description = "The scale modifier on camera speed"},
      CameraOffset ={default = Vector3(0, -10, 5), description = "The camera offset from the character"},
    }
  }
}
```

![keywords and grouping properties](https://i.ibb.co/MhCJ7th/simple2.png)

***

### 更新 `ConstantRotate` Lua 脚本

以下是我们将要执行的作的概述，以更新`RotateConstant` Lua 脚本：我们希望最终得到一个通用的 `ConstantRotate` Lua 组件。这意味着我们应该能够指定绕 X、Y 或 Z 轴旋转的转速，甚至是它们的组合。我们需要在属性表中做的就是允许用户为每个轴指定旋转速度：

```lua
local ConstantRotate = 
{
  Properties =
  {
    X = {default = 0, description = "The X axis rotation speed in degrees per second", suffix = " deg/s"},
    Y = {default = 0, description = "The Y axis rotation speed in degrees per second", suffix = " deg/s"},
    Z = {default = 0, description = "The Z axis rotation speed in degrees per second", suffix = " deg/s"},
  }
}
```

![constant rotate reflected](https://i.ibb.co/QCBzLb3/constantrotate.png)

接下来，我们需要 Lua 脚本在旋转实体时使用这些值。进行一些数学运算以转换为以度为单位的旋转。

```lua
function ConstantRotate:OnTick(deltaTime, timePoint)
  TransformBus.Event.RotateAroundLocalX(self.entityId, deltaTime * self.Properties.X * math.pi / 180)
  TransformBus.Event.RotateAroundLocalY(self.entityId, deltaTime * self.Properties.Y * math.pi / 180)
  TransformBus.Event.RotateAroundLocalZ(self.entityId, deltaTime * self.Properties.Z * math.pi / 180)
end
```

***

### 完整的 `ConstantRotate` Lua 脚本

```lua
local ConstantRotate = 
{
  Properties =
  {
    X = {default = 0, description = "The X axis rotation speed in degrees per second", suffix = " deg/s"},
    Y = {default = 0, description = "The Y axis rotation speed in degrees per second", suffix = " deg/s"},
    Z = {default = 0, description = "The Z axis rotation speed in degrees per second", suffix = " deg/s"},
  }
}

function ConstantRotate:OnActivate()
  self.tickHandler = TickBus.Connect(self)
end

function ConstantRotate:OnDeactivate()
  self.tickHandler:Disconnect()
  self.tickHandler = nil
end

function ConstantRotate:OnTick(deltaTime, timePoint)
  TransformBus.Event.RotateAroundLocalX(self.entityId,  deltaTime * self.Properties.X * math.pi / 180)
  TransformBus.Event.RotateAroundLocalY(self.entityId,  deltaTime * self.Properties.Y * math.pi / 180)
  TransformBus.Event.RotateAroundLocalZ(self.entityId,  deltaTime * self.Properties.Z * math.pi / 180)
end

return ConstantRotate
```
