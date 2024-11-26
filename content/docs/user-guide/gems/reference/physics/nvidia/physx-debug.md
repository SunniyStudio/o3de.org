---
linkTitle: PhysX Debug
title: PhysX Debug Gem
description: PhysX Debug Gem 为 Open 3D Engine （O3DE） 项目中的 PhysX 提供调试功能和可视化效果。
toc: true
---

PhysX Debug Gem 提供了调试 PhysX 场景几何体的可视化效果的功能，例如 **PhysX Primitive Collider** （PhysX 基元碰撞器） 和 **PhysX Dynamic Rigid Body** （PhysX 动态刚体） 组件等。

当您输入控制台变量或使用 **ImGui** 工具时，您可以在编辑器和游戏模式下查看 PhysX 调试行。此 Gem 使用直接来自 PhysX 的数据来实时显示模拟世界的剔除视图（受与摄像机的接近程度的限制）。

在编辑器模式下，此 Gem 在视区摄像机的给定距离内显示 PhysX 形状。在游戏模式下，此 Gem 使用当前活动的摄像机来可视化 PhysX 场景的剔除视图。

此 Gem 包括以下功能：
+ 可视化物理几何体的调试渲染，例如碰撞基元、地形、形状和力。
+ 使用控制台变量、PhysX 设置菜单和 **ImGui** 工具控制机制。
+ 可视化视锥体剔除。
+ 使用第三方工具 Visual Debgger 的 PhysX Visual Debugger 挂钩和控件。
+ 碰撞网格的基于接近度的调试可视化。

{{< note >}}
要启用 PhysX 调试 Gem，您必须首先启用 [PhysX](/docs/user-guide/gems/reference/physics/nvidia/physx) 和 [ImGui](/docs/user-guide/gems/reference/debug/imgui/)。
{{< /note >}}

有关详细信息，请参阅 [调试PhysX](/docs/user-guide/interactivity/physics/debugging/).
