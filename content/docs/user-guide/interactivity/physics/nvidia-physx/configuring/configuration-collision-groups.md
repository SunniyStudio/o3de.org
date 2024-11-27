---
linkTitle: 碰撞组
title: PhysX 碰撞组
description: ' 在 Open 3D Engine 中为 PhysX 系统创建碰撞组。 '
weight: 400
toc: true
---

碰撞组充当碰撞图层的遮罩。您可以指定哪些碰撞图层是碰撞组的一部分。一个碰撞层可以包含在多个碰撞组中。每个 PhysX 碰撞器组件都可以与一个碰撞组交互。碰撞器可以与分配给所选碰撞组中包含的碰撞层的任何碰撞器进行交互。

{{< important >}}
如果碰撞体的碰撞层位于彼此的碰撞组中，则碰撞体会进行交互。如果一个碰撞层不存在于另一个层的碰撞组中，则碰撞体不会交互。
{{< /important >}}

## 创建碰撞组

1. 在 O3DE 编辑器中，从 **Tools** 菜单中选择 **PhysX Configuration**。

1. 选择 **Collision Filtering**（碰撞筛选）选项卡。

1. 单击 **Groups** 按钮查看碰撞组列表。可用的碰撞层显示为列。

1. 单击 **Add**（添加），然后在文本框中输入组的名称。

    ![Adding Collision Groups in the PhysX Configuration tool.](/images/user-guide/interactivity/physics/nvidia-physx/configuring/physx-configuration-4.png)

1. 选中 collision layer （碰撞层） 列中的复选框，以将碰撞层包含在碰撞组中。清除该复选框以从碰撞组中排除碰撞层。

## 碰撞组分配

1. 在 **O3DE Editor** 中，选择要分配碰撞组的具有 **PhysX Collider** 组件的实体。

1. 在 **PhysX Collider** 组件中，从 **Collides With** 属性中，从下拉列表中选择一个可用的碰撞组。

    ![Choosing a collision group for the PhysX Collider component in the Entity Inspector.](/images/user-guide/interactivity/physics/nvidia-physx/configuring/physx-configuration-5.png)

## 碰撞组配置示例

以下示例定义 **Player**、**Enemy**、**Bullet** 和 **Terrain** 图层。这些碰撞层分为以下碰撞组：

+ **PlayerBullet** - 与 **Enemy** 和 **Terrain** 碰撞
+ **EnemyBullet** - 与 **Player** 和 **Terrain**碰撞
+ **TerrainCollision** - 与 **Player**, **Enemy**, **Bullet**, 和 **Terrain**碰撞
+ **PlayerCollision** - 与 **Enemy**, **Bullet**, 和 **Terrain**碰撞

![An example collision group configuration.](/images/user-guide/interactivity/physics/nvidia-physx/configuring/physx-configuration-6.png)

玩家发射的子弹具有以下图层和组：
+ Layer: **Bullet**
+ Group: **PlayerBullet**

敌人发射的子弹具有以下图层和组：
+ Layer: **Bullet**
+ Group: **EnemyBullet**

{{< note >}}
您不必定义“敌方 bullet”或“player bullet”层。相反，请定义单个 **Bullet** 图层并创建单独的碰撞组，以指定与之碰撞的对象。
{{< /note >}}
