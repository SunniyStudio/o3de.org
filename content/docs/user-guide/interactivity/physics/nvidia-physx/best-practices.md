---
description: ' 请参阅以下在 Open 3D Engine 中使用 PhysX 的最佳实践。 '
title: PhysX 最佳实践
weight: 600
---

在使用 PhysX 时，请参阅以下最佳实践。
+ 与地形相交的碰撞体可能会导致意外行为。例如，对象可能会火箭飞入太空、抖动或降低性能。避免将碰撞体与地形相交。如果需要使碰撞体与地形相交，请为碰撞体大小使用较小的值。可以通过清除 **PhysX Configuration** 工具的 **Global Configuration** 选项卡中的 **Persistent Contact Manifold** 复选框来缓解这些情况。
+ **PhysX Character Controller** 组件必须与 **Actor** 组件位于同一实体上，才能使用 **Animation Editor**。
+ 将 **PhysX Static Rigid Body** 组件添加到实体时，请选中 **Transform** 组件中的 **Static** 选项。这允许其他系统对在运行时永远不会移动的静态实体应用优化。
+ 当存在 **PhysX Dynamic Rigid Body** 组件时，请避免选中 **Static** 选项。刚体将以静态方式运行，并且将出现一条警告，指出 **PhysX Dynamic Rigid Body** 组件和 **Static** 变换选项不兼容。
+ 在将 PhysX 碰撞器添加到实体时，首选 **PhysX Primitive Collider** 组件。它提供更好的性能，应尽可能使用。

