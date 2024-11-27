---
linkTitle: PhysX 配置
title: 配置 PhysX 系统
description: '在 Open 3D Engine 中配置 PhysX 系统的设置。 '
weight: 100
---

可以为每个项目配置 PhysX 系统。使用 **PhysX Configuration** （PhysX 配置） 工具，您可以执行以下操作：

* 设置 PhysX 模拟的全局频率。
* 设置全局 PhysX 重力常数。
* 启用连续碰撞检测 （CCD） 以改善碰撞结果。
* 设置冲突的最小弹跳阈值。
* 配置调试绘制可视化属性。
* 为全局和局部风力设置标签。
* 创建碰撞图层和组。
* 配置可视化调试器。

在 O3DE Editor 中，从 **Tools** 菜单中选择 **PhysX Configuration** 以打开 PhysX Configuration 工具。对 PhysX 配置设置所做的更改会自动保存在项目的`<project_name>\Registry\physxsystemconfiguration.setreg`文件中。如果您使用的是源代码管理，请确保包含此文件。

本节中的主题提供有关 PhysX Configuration （PhysX 配置） 工具中可用设置的信息。

| 主题 | 说明 |
| - | - |
| [全局配置](configuration-global) | 了解全局 PhysX 设置，包括模拟频率、碰撞设置、调试可视化选项和风标签。 |
| [碰撞图层](configuration-collision-layers) | 创建碰撞层以将 PhysX 实体组织到类别中。 |
| [碰撞组](configuration-collision-groups) | 创建碰撞组以定义哪些碰撞层彼此交互。 |
| [在代码中创建碰撞图层和碰撞组](configuration-collision-layer-and-group-programming) | 以编程方式创建和访问碰撞组和层。 |
| [调试器配置](configuration-debugger) | 配置 PhysX Visual Debugger （PVD）。 |
| [PhysX World 编程注意事项](configuration-physx-world-programming-notes) | 使用 *PhysX World* 进行同步离散模拟，从而创建单个大型模拟的错觉。 |
