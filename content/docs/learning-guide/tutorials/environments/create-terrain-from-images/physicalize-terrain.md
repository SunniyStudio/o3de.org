---
linkTitle: 物理化地形
title: 物理化地形
description: 通过添加物理碰撞器来物理化地形。
weight: 500
toc: true
---

在本教程部分中，您将通过添加物理碰撞器来物理化地形。

## 添加地形物理特性

您可以向地形添加碰撞以进行 PhysX 模拟，并应用 [物理材质](/docs/user-guide/interactivity/physics/nvidia-physx/materials/)  来定义地形的物理属性，例如动态和静态摩擦。要向地形添加物理特性，请执行以下操作：

1. 添加 **PhysX Heightfield Collider** 组件到 **Terrain Spawner** 实体。

2. **PhysX Heightfield Collider** 组件显示有关缺少必需组件的警告。选择 **Add Required Component** 并从列表中选择 **Terrain Physics Heightfield Collider**。

3. **PhysX Heightfield Collider** 组件将碰撞器添加到地形，以便动态 PhysX 对象与其碰撞。如果 heightfield 在运行时没有更改，例如从图像渐变（高度贴图）生成地形时，您可以启用 **Use Baked Heightfield** 属性以获得更好的性能。

4. **Terrain Physics Heightfield Collider** 组件的作用类似于 **Terrain Surface Materials List** 组件。为地形添加默认物理材质以定义其物理属性。您还可以添加与上一节中应用的细节材质匹配的特定物理材质。例如，如果您有泥土、草地和岩石的细节材质，则可以应用适合这些不同细节材质的物理材质。
