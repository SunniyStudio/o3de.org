---
title: "[Lua]-Using-Transform-Bus"
description: ""
toc: false
---

### The Transform Component

When you create a new entity, a `Transform` component is automatically added to it, which also be cannot removed. A `Transform` component represents the entity's position in 3D space and also the parent entity it is attached to. In order for your components to communicate with the `Transform` component, you will need to use the `TransformBus`.

***

### Example Overview

In this example, we will be rotating an entity every frame using the `TickBus` event `OnTick` discussed in the previous chapter. Every frame, our component will communicate with the `Transform` component that exists on the same entity that the Lua script is attached to, by sending a `TransformBus` event called `RotateAroundLocalZ`. In order to better visualize what is happening, a `Mesh` component is also added to the entity.

![example overview](https://s9.gifyu.com/images/example1.gif)

1. Connect to `TickBus` notification events
2. Implement the `OnTick` event to receive notifications about when a new frame has occured
3. Every frame, we use the `TransformBus` to send an event called `RotateAroundLocalZ` addressed to the same entity this Lua script is on
4. To get the address of the entity this Lua script is on, we use `self.entityId` which is a special value provided to us

***

### Lua Script Implementation

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
  TransformBus.Event.RotateAroundLocalZ(self.entityId, deltaTime)
end

return ConstantRotate
```

***

### Explanation

Notice that instead of subscribing to an event and implementing what happens when we receive that event (which was the case in the `TickBus` examples); In this example, we are sending an event to a specific entity. The implementation of what happens is left to the other component (in this case the `Transform` component). If you are curious about the exact implementation, you can find it [here](https://sourcegraph.com/github.com/o3de/o3de/-/blob/Code/Framework/AzToolsFramework/AzToolsFramework/ToolsComponents/TransformComponent.cpp?L592:34). For now, it is enough for us to simply know that components are able to send events to all or specific addresses, and that other (possibly including the sender of the event) components that are subscribed to such events will be notified about it.

There are two ways to send events generally; we use the `TransformBus` for our examples. We can use `Event` which sends the event message to a specific address or `Broadcast` which sends the message to everything that is subscribed to its events. We omit the first parameter which is the address to send the event message to when using a `Broadcast` as it is not needed.

* `TransformBus.Event.RotateAroundLocalZ(self.entityId, deltaTime)`
* `TransformBus.Broadcast.RotateAroundLocalZ(deltaTime)`

When you use this specific example with `Broadcast`, every activated entity in your game will be rotating! I will be covering in more detail about the event messaging system in a later chapter called _EBus Overview_.

***

### Additional Resources
* `TransformBus.h` source code [link](https://sourcegraph.com/github.com/o3de/o3de/-/blob/Code/Framework/AzCore/AzCore/Component/TransformBus.h)

