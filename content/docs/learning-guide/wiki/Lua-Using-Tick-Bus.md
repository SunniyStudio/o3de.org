---
title: "[Lua]-Using-Tick-Bus"
description: ""
toc: false
---

### Polling

When you need to do something every frame, the `TickBus` is what you will need to use. The `TickBus` will notify every component that is subscribed to it when a new frame has occured. The component will also receive information such as `deltaTime`: the time in seconds since the last frame.

```lua
local Ticking = {}

function Ticking:OnActivate()
  self.tickHandler = TickBus.Connect(self)
end

function Ticking:OnDeactivate()
  self.tickHandler:Disconnect()
  self.tickHandler = nil
end

function Ticking:OnTick(deltaTime, timePoint)
  Debug.Log("I am printing every frame! Time since last frame in seconds: " .. deltaTime)
end

return Ticking
```
![printing to the console each frame](https://i.ibb.co/k9Mxn5x/ticking.png)

***

## Breakdown

### Connecting to `TickBus`

When the component activates, we use `Connect` to subscribe to `TickBus` events. We need a variable `self.tickHandler` to store a reference so that we can unsubscribe from the `TickBus`. You can name this variable whatever you like.

```lua
function Ticking:OnActivate()
  self.tickHandler = TickBus.Connect(self)
end
```

### Receiving Tick Events

To receive `TickBus` events you simply need to have correctly named `TickBus` event functions, which will be called automatically as long as your component is subscribed to the `TickBus`. Any parameters you include will also be populated when the function gets called. Use the `Classes Reference` in `LuaIDE` to find the available events and parameters.

![Using the Classes Reference in LuaIDE](https://i.ibb.co/mbjS6ML/classes-Reference.png)

```lua
function Ticking:OnTick(deltaTime, timePoint)
  Debug.Log("I am printing every frame! Time since last frame in seconds: " .. deltaTime)
end
```

### Disconnecting from `TickBus`

To unsubscribe from `TickBus` events, use `Disconnect` on the reference stored during the connection process. It is not strictly necessary to `Disconnect` or set variables to `nil` when a component deactivates, but it is considered best practice. It is safe to use `Disconnect` on a handler that is already disconnected but not when it is `nil`.

```lua
function Ticking:OnDeactivate()
  self.tickHandler:Disconnect()
  self.tickHandler = nil
end
```

***

### Additional Resources

* `TickBus.h` source code [link](https://sourcegraph.com/github.com/o3de/o3de/-/blob/Code/Framework/AzCore/AzCore/Component/TickBus.h)
