---
linkTitle: Create Layers and Groups in Code
title: Creating Collision Layers and Groups Programmatically
description: ' Create layers and groups programmatically in Open 3D Engine. '
weight: 500
toc: true
---

## 检索图层和组

您可以检索 [您在 O3DE 编辑器中创建的](configuration-collision-groups) 的碰撞层和组的实例，如下所示。这些方法将执行在 **PhysX Configuration** （PhysX 配置） 工具中定义的图层的查找。如果未找到与名称匹配的碰撞层，则返回默认层 \(`0`\)。

```
CollisionLayer layer("MyLayer");
CollisionGroup group("MyGroup");
```

您还可以使用请求总线来查找层和组，如以下代码所示：

```
CollisionLayer layer;
CollisionRequestBus::BroadcastResult(layer, &Physics::CollisionRequests::GetCollisionLayerByName, layerName);
CollisionGroup group;
CollisionRequestBus::BroadcastResult(group, &Physics::CollisionRequests::GetCollisionGroupByName, groupName);
```

## 创建图层和组

您可以在运行时在代码中创建碰撞层和组。这在您可能无法预定义碰撞图层和组的情况下非常有用，例如在运行时按程序生成资产的项目。

以下示例代码在运行时创建一个碰撞组，其中包含一个 `Enemy` 层和一个 `Tree` 层。

```
CollisionLayer layer1("Enemy"), layer2("Tree");
CollisionGroup group = AzFramework::Physics::CollisionGroup::None;
group.SetLayer(layer1, true);
group.SetLayer(layer2, true);
```

如果构造碰撞组所需的所有层都是已知的，则可以使用重载运算符，如以下示例所示：

```
CollisionGroup group = CollisionLayer("Layer1") | CollisionLayer("Layer2") | CollisionLayer("Layer3");
```
