---
description: ' 在 Open 3D Engine的 <guilabel>Track View</guilabel> 编辑器中为轨迹视图序列使用电影照明。 '
title: 添加光源
---

您可以在序列中使用仅在序列中触发的灯光组件和/或时间设置来设置不同的灯光场景。您可以将灯光组件添加到序列中，然后添加想要动画的轨道。然后，您可以设置 [控制台变量节点](/docs/user-guide/visualization/cinematics/track-view/nodes-cvar/) 来指定一天中的时间设置。

## 电影照明最佳实践

请参阅以下推荐的电影照明指南和最佳实践。
+ 灯光应在轨迹视图中打开或关闭。如果灯光默认为关闭，它们就不会在游戏中意外渲染或干扰场景拍摄。您可以对每个灯光的**Visible**轨迹进行动画处理，以确定何时应打开或关闭灯光。
+ 根据拍摄需要禁用游戏和立方体贴图灯光，以避免干扰。
+ 对于预渲染的电影场景，使用控制台变量 `e_timeofday` 来触发正确的时间。


更多信息，请参阅 [使用控制台窗口](/docs/user-guide/editor/console/)。
+ 对于实时电影，可使用轨迹事件节点来触发正确的时间。更多信息，请参阅[Event Node](/docs/user-guide/visualization/cinematics/track-view/nodes-event/)。
+ 对于预渲染的电影场景，使用[Shadows Setup Node](/docs/user-guide/visualization/cinematics/track-view/nodes-shadows/)启用高质量阴影模式。
+ 对于预渲染的电影场景，由于性能不是问题，因此应始终启用阴影投射，并根据需要使用尽可能多的聚光灯。聚光灯应尽量使用投影仪纹理。**SpecularMultiplier** 值应始终为 `1`。 
+ 当**ProjectorFOV**值尽可能低时，点光源的阴影贴图质量会大大提高。要柔化阴影，可以稍微提高**ProjectorFOV**值，但这样也会降低阴影贴图的精确度。
+ 不要使用环境光，因为它们会削弱对比度并照亮不需要的区域。相反，使用立方体贴图使最深的阴影尽可能暗，然后添加灯光以增加整体照明。

## Light 组件和暴露到轨道 

有关电影光照组件的信息，请阅读 [Atom Light 组件](/docs/user-guide/components/reference/atom/light/) 文档。
