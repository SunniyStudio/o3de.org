---
description: ' 使用 NVIDIA Blast 在 Open 3D Engine 中创建逼真的破坏模拟。 '
title: 适用于 NVIDIA Blast 的 Script Canvas 节点
weight: 800
draft: true
---

NVIDIA Blast Gem 包含多个 **Script Canvas** 节点，用于编写可破坏资产的脚本。这些节点可以在 **Script Canvas** 的 **Blast** 组中找到。

**内容**
+ [BlastFamilyComponentNotificationBus 节点](#blastfamilycomponentnotificationbus-nodes)
+ [BlastFamilyComponentNotificationBus 节点](#blastfamilycomponentnotificationbus-nodes-1)
+ [BlastFamilyDamageRequestBus 节点](#blastfamilydamagerequestbus-nodes)

## BlastFamilyComponentNotificationBus nodes 

**On Actor Created**
每当从给定实体中使用的已销毁对象创建角色时调用的事件通知。

**On Actor Destroyed**
每当从给定实体中使用的已销毁对象销毁 actor 时调用的通知。

## BlastFamilyComponentNotificationBus nodes 

**Get Actors Data**
从给定实体中使用的可破坏对象获取角色数据，例如实体 ID 以及角色是否为静态对象。

## BlastFamilyDamageRequestBus nodes 

**Capsule Damage**
对距离 **position0** 或 **position1** 不远于 **minRadius** 的所有区块和键造成完全伤害。如果距离小于 **maxRadius**，则应用线性递减的伤害。

**Destroy Actor**
从给定实体中使用的已销毁对象中销毁 actor。

**Get Family Id**
获取给定实体中 **Blast Family ** 的实体 ID。

**Impact Spread Damage**
对距离 **position** 不远于 **minRadius** 的所有区块和键造成全额伤害。如果距离小于 **maxRadius**，则应用减少伤害。减少伤害是使用支撑图上的 BFS 而不是欧几里得距离计算的。

**Radial Damage**
对距离 **position** 不远于 **minRadius** 的所有区块和键造成全额伤害。如果距离小于 **maxRadius**，则应用线性递减的伤害。

**Shear Damage**
对与 **normal** 正交的键造成全额伤害。不会对平行的键造成任何伤害。对区块的伤害取决于到 **position** 的距离。**minRadius** 和 **maxRadius** 之间的伤害衰减是线性的。

**Stress Damage**
应力损伤是使用 **force** 向量而不是标量值施加的。损伤根据支撑图在键之间传播。

**Triangle Damage**
与由 **position0**、**position1** 和 **position2** 定义的给定顶点描述的三角形相交的所有块和键都会受到完全伤害。
