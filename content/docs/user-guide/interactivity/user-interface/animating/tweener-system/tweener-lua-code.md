---
linkTitle: Tweeners in Lua
description: ' 了解如何使用 Lua 脚本通过 Open 3D Engine 中的 Scripted Entity Tweener 系统为实体制作动画。 '
title: Tweeners in Lua
weight: 300
---

您必须具有最少的代码集才能在 Scripted Entity Tweener 系统中播放动画。将此脚本用作 **Lua script** 组件的一部分。有关 Lua 脚本组件的更多信息，请参阅 [将 Lua 脚本添加到组件实体](/docs/user-guide/scripting/lua/add-lua-script).

在以下示例中，实体的不透明度在 “`5`” 秒内线性补间为 “`0.5`”。

```
local AnimateUiEntity =
{
	Properties =
	{
		ToAnimate = {default = EntityId()},
	},
}
function AnimateUiEntity:OnActivate()
	self.tickBusHandler = TickBus.Connect(self)
	self.ScriptedEntityTweener = require("Scripts.ScriptedEntityTweener.ScriptedEntityTweener")
end

function AnimateUiEntity:OnTick(deltaTime, timePoint)
	self.tickBusHandler:Disconnect()
	self.ScriptedEntityTweener:StartAnimation
	{
		id = self.Properties.ToAnimate,
		duration = 5.0,
		["opacity"] = 0.5,
	}
end
function AnimateUiEntity:OnDeactivate()
	self.ScriptedEntityTweener:OnDeactivate()
end
return AnimateUiEntity
```

以下示例显示了具有所有默认参数的完整调用。在此示例中，实体`duration`为 `5` 秒内移动到代码中指示的 `["x"]` 和 `["y"]`位置。

```
self.ScriptedEntityTweener:StartAnimation
	{
		id = self.Properties.ToAnimate,
		duration = 5.0,
		["x"] = 150, ["y"] = 200,
		timeIntoTween = 0, -- Start tween some seconds in
		easeMethod = ScriptedEntityTweenerEasingMethod_Linear,
		easeType = ScriptedEntityTweenerEasingType_In,
		delay = 0,
		timesToPlay = 1,
		isFrom = false,
		isPlayingBackward = false,
		uuid = Uuid.Create(),
		--onComplete = function() Debug.Log("Called when this animation is done") end
		--onUpdate = function(currentValue, currentProgressPercent) Debug.Log("Called when this animation updates") end
		--onLoop = function() Debug.Log("Looped animation") end
	}
```

有关参数说明，请参阅 [Tweener 支持的组件](./tweener-components).
