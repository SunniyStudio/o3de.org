---
linkTitle: PhysX Worlds
title: 对 PhysX Worlds 进行编程
description: ' Open 3D Engine （O3DE） 中 PhysX 世界的编程说明。 '
weight: 700
toc: true
---

对于要模拟的 PhysX 对象，它们必须存在于世界内。多个世界可以有如下用途：
+ 模拟第一个世界中的动作结果。例如，第二个世界可能会显示如果在第一个世界中撞倒了一座积木塔，那么它在 5 秒后会是什么样子。
+ 模拟您不想与世界其他部分交互的对象子集。例如，您可以模拟附加到玩家腰带上的对象的移动。
+ 克服单个大世界的硬件或软件限制。通过将单个世界平铺为多个较小的世界并在它们之间移动对象，您可以创建单个大世界的错觉。

## World ID

在 PhysX Gem 中创建的每个世界都可通过类型为`AZ::Crc32`的 ID 进行寻址。使用此 ID 对 `WorldRequestBus` 进行寻址。

如果您的游戏中只有一个世界，则可以使用 `BroadcastResult` 来调用`WorldRequestBus`，如以下示例所示：

```
// Single world setup
RayCastHit choose;
WorldRequestBus::BroadcastResult(choose, &WorldRequests::RayCast, request);
```

如果您有一个多世界游戏，请使用 `EventResult` 并传入世界 ID，如以下示例所示：

```
// Multiple world setup
RayCastHit choose;
WorldRequestBus::EventResult(choose, AZ_CRC("AZPhysicalWorld"), &WorldRequests::RayCast, request);
```

{{< note >}}
如果您的游戏创建多个世界，则必须管理添加到这些世界中的对象。
{{< /note >}}

## 时间步长常数

您可以在创建 `PhysXWorld` 时为 `Physics::WorldSettings` 配置步长常量。有关更多信息，请参阅 [PhysX 系统全局配置](./configuration-global/#system-configuration).
