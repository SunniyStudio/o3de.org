---
link-title: Cloth 调试
description: '使用控制台变量 （CVAR） 在 Open 3D Engine （O3DE） 中调试 NVIDIA Cloth。'
title: NVIDIA Cloth 调试
weight: 500
---

![Debug visualization of the NVIDIA Cloth displaying wireframe, normals, tangents, and bitangents.](/images/user-guide/interactivity/physics/nvidia-cloth/cloth-debug-visualization.png)

您可以在编辑器控制台中使用以下控制台变量 （CVAR） 来可视化和调试 NVIDIA Cloth：

| CVAR | 说明 | 值 |
| --- | --- | --- |
| `cloth_DebugDraw <value>` | 绘制布料网格线框。 | **0** - 禁用线框显示。<br>**1** - 启用线框显示。 |
| `cloth_DebugDrawNormals <value>` | 绘制 Cloth 网格法线。 | **0** - 禁用法线显示。<br>**1** - 启用法线显示。<br>**2** - 启用法线、切线和双切线显示。 |
| `cloth_DebugDrawColliders <value>` | 绘制布料碰撞器。 | **0** - 禁用碰撞器显示。<br>**1** - 启用碰撞器显示。 |
| `cloth_DebugDrawMotionConstraints <value>` | 绘制布料运动约束。 | **0** - 禁用运动约束显示.<br>**1** - 启用运动约束显示。 |
| `cloth_DebugDrawBackstop <value>` | 绘制布料 backstop。 | **0** - 禁用 backstop 显示。<br>**1** - 启用 backstop 显示。 |
| `cloth_DistanceToTeleport <meters>` | 实体在帧中移动才能将其视为布料传送的米数。 | 表示以米（世界单位）为单位的距离的浮点值。 |
| `cloth_SecondsToDelaySimulationOnActorSpawned <seconds>` | 布料模拟将延迟的时间（以秒为单位），以避免在生成 Actor 时突然产生冲动。 | 表示延迟（以秒为单位）的浮点值。 |
