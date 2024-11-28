---
linkTitle: Tweener 参数
description: ' 了解如何在 Open 3D Engine 的脚本化实体 Tweener 系统中修改 Lua 脚本参数。 '
title: Tweener 参数
weight: 400
---

脚本化实体补间参数提供了灵活性，并允许您自定义补间动画。将参数与 [timeline](./tweener-timeline) 一起使用，以获得各种可能性。

使用以下补间参数自定义动画。

**duration**
指定动画从起始值到其最终值的时间（以秒为单位）。

**\["x"\] and \["y"\]**
`UiTransform2dComponent`的`x` 和 `y`值的快捷方式，它会自动附加到每个 UI 实体。

**timeIntoTween**
指定从指定点开始的补间（以秒为单位）。例如，如果`duration`设置为`6`，而`timeIntoTween`设置为 `3`，则补间从中间点立即开始，并在三秒后结束。

**easeMethod**
指定要应用于补间的 [缓动类型](concepts/tweener-understanding-types)。
+ `ScriptedEntityTweenerEasingMethod_Linear`
+ `ScriptedEntityTweenerEasingMethod_Quad`
+ `ScriptedEntityTweenerEasingMethod_Cubic`
+ `ScriptedEntityTweenerEasingMethod_Quart`
+ `ScriptedEntityTweenerEasingMethod_Quint`
+ `ScriptedEntityTweenerEasingMethod_Sine`
+ `ScriptedEntityTweenerEasingMethod_Expo`
+ `ScriptedEntityTweenerEasingMethod_Circ`
+ `ScriptedEntityTweenerEasingMethod_Elastic`
+ `ScriptedEntityTweenerEasingMethod_Back`
+ `ScriptedEntityTweenerEasingMethod_Bounce`

**easeType**
修改 `easeMethod`.
+ `ScriptedEntityTweenerEasingType_In`
+ `ScriptedEntityTweenerEasingType_Out`
+ `ScriptedEntityTweenerEasingType_InOut`

**delay**
指定延迟动画开始的时间量（以秒为单位）。

**timesToPlay**
播放动画的次数。指定 `-1`  可无限期播放动画。

**isFrom**
如果为 `false`，动画从组件中指定的值开始，到脚本中指定的值结束。
如果为 `true`，动画从脚本中指定的值开始，到组件中指定的值结束。

**isPlayingBackward**
如果为`true`，则动画向后播放。要向后播放动画，必须为`timeIntoTween` 指定一个值。如果没有为 `timeIntoTween` 指定值，则忽略此参数。

**uuid**
由 Scripted Entity Tweener 系统生成并用于调试目的的 ID。您无需修改或指定值。

**onComplete, onUpdate, 和 onLoop**
使用这些参数可以指定动画完成、更新或循环时的回调函数。`onUpdate` 还会传回动画的当前完成状态百分比和 animated 参数的当前值。
