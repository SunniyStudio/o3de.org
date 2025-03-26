---
title: "[Lua]-Reflecting-Properties-to-the-Editor"
description: ""
toc: false
---

This tutorial is a continuation of _Using Transform Bus_ [here](https://github.com/o3de/o3de/wiki/%5BLua%5D-Using-Transform-Bus). 

### Current Problems

As a reminder, here is the full Lua implementation of the `ConstantRotate` script we have so far:

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

There are two main usability issues here:
1. Whenever we want to change the rotation speed, we will have to open the Lua script and change the values
2. Every entity that is using this Lua component will all have the same values

Both of these can be fixed by using the `Properties` table in Lua

***

### Properties Table Simple Example

The `Properties` table allows us to use per-component-instance values in our Lua scripts. Here is a simple example which allows us print a string of our choosing without changing the Lua script, but just by staying in the O3DE Editor.

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

#### Default settings

![default component](https://i.ibb.co/DLcG73t/defaultreflect.png)

![default message](https://i.ibb.co/5r1rYzR/defaultlog.png)

#### Edited settings

We can now simply change the value in the Editor, and the Lua script will update accordingly.

![edited component](https://i.ibb.co/hRyPG2s/edited-Reflect.png)

![edited message](https://i.ibb.co/DGvSVnf/edited-Log.png)

***

### Using the Properties Table

All you need to do to start reflecting data to the editor is to include a `Properties` table when declaring your (pseudo) Lua component like this:

```lua
local MyLuaComponent =
{
  Properties =
  {

  }
}
```

Afterwards, you can reflect any of the basic Lua types or reflected classes in O3DE. I will talk more about reflected classes in the advanced topic called _C++ Script Bindings_. You are also free to nest the data however you like. The reflected data can use the following keywords:

* `default`
* `description`
* `suffix` for numbers
* `prefix` for numbers
* `min` for numbers
* `max` for numbers
* `step` for numbers

Lists of data are converted to arrays. It is also possible to omit any keywords and the enclosing braces if they are not required. Here are some examples showing different ways of reflecting data to the editor:

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

#### Using Keywords and Grouping Properties

Your exposed properties will be automatically organized by name. If you would like to group your properties then you can do so by nesting them.

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

### Updating `ConstantRotate` Lua Script

Here is an overview of what we are going to do to update the `RotateConstant` Lua script: We want to end up with a general purpose `ConstantRotate` Lua component. This means that we should be able to specify a rotation speed for rotating around the X, Y, or Z axis, or even a combination of them. All we need to do in the properties table is allow the user to specify a rotation speed for each axis:

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

Next we need our Lua script to use those values when rotating the entity. Some maths is done to convert to rotations in degrees.

```lua
function ConstantRotate:OnTick(deltaTime, timePoint)
  TransformBus.Event.RotateAroundLocalX(self.entityId, deltaTime * self.Properties.X * math.pi / 180)
  TransformBus.Event.RotateAroundLocalY(self.entityId, deltaTime * self.Properties.Y * math.pi / 180)
  TransformBus.Event.RotateAroundLocalZ(self.entityId, deltaTime * self.Properties.Z * math.pi / 180)
end
```

***

### Complete `ConstantRotate` Lua Script

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
