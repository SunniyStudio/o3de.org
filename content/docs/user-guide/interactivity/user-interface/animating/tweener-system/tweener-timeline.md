---
linkTitle: Tweener Timeline
description: ' 使用 Scripted Entity Tweener 的时间轴功能将动画链接在一起，并在 Open 3D Engine 中对它们进行精细控制。 '
title: Tweener Timeline
weight: 500
---

在 Scripted Entity Tweener 系统中，您可以创建时间轴以对动画进行精细控制。您可以链接和分组动画。您可以暂停、恢复、搜索、向后播放、标记，甚至动态控制动画的播放速度。

要创建时间轴，您可以自定义以下 Lua 脚本。

在此示例中，脚本首先对 `ToAnimate` 指定的实体进行动画处理，将其大小从其原始值增加到宽度 `["w"]` 和高度`["h"]`为`600`。然后，该脚本通过将实体缩小到宽度`["w"]`和高度 `["h"]`的 `200`来为实体添加动画。

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
	self.timeline = self.ScriptedEntityTweener:TimelineCreate()
	self.timeline:Add(
		{
			id = self.Properties.ToAnimate,
			easeMethod = ScriptedEntityTweenerEasingMethod_Bounce,
			easeType = ScriptedEntityTweenerEasingType_In,
			duration = 3.0,
			["w"] = 600, ["h"] = 600,
		})
	self.timeline:Add(
		{
			id = self.Properties.ToAnimate,
			easeMethod = ScriptedEntityTweenerEasingMethod_Sine,
			easeType = ScriptedEntityTweenerEasingType_in-out,
			duration = 3.0,
			["w"] = 200, ["h"] = 200,
		})
	self.timeline:Play()
end

function AnimateUiEntity:OnDeactivate()
	self.ScriptedEntityTweener:OnDeactivate()
end

return AnimateUiEntity
```

以下示例显示了所有受支持的时间线操作。

```
-- Chained animations
self.timeline = self.ScriptedEntityTweener:TimelineCreate()
self.timeline:Add(…)
self.timeline:Add(…)
self.timeline:Play()

--Labels
self.timeline:AddLabel("Label", 0.5) -Add label that specifies 0.5 seconds  into animation
self.timeline:Play("Label") -Play from label

--General timeline operations
local animationParameters = { id = self.Properties.ToAnimate, duration = 3, ["opacity"] = 0.5 }
self.timeline:Add({animationParameters}, {["label"] = "LabelName"}, ["offset"] = 2) --Start animation 2 seconds after LabelName
self.timeline:Add({animationParameters}, {["initialStartTime"] = 2}, ["offset"] = -1) --Start animation at second 1

self.timeline:Play()
self.timeline:Play(2) --Play timeline starting at 2 seconds
self.timeline:Pause() --Pause timeline
self.timeline:Resume() --Resumes timeline, Play() also resumes
self.timeline:Seek(3) --Play timeline starting at 3 seconds
self.timeline:PlayBackwards() --Start playing animation backwards
self.timeline:PlayBackwards(3) --Start playing animation backwards starting from second 3
self.timeline:SetSpeed(2) --Start playing animation at 2x speed
```
