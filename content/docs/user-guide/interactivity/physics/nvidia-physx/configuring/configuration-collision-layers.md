---
linkTitle: 碰撞层
title: PhysX 碰撞层
description: ' 在 Open 3D Engine （O3DE） 中为 PhysX 系统创建碰撞层。 '
weight: 300
toc: true
---

使用碰撞层，您可以将相关的 PhysX 实体放入类别中。以下列表演示了一些示例 PhysX 碰撞层：

* **Terrain** - 地形、地板以及玩家实体可以遍历的任何实体。
* **Static objects** - 具有碰撞体和静态刚体组件但没有动画的实体，例如大型岩石、树干和墙壁。
* **Players** - 所有玩家控制的实体。
* **Enemies** - 通过脚本或 AI 移动的实体，可以对玩家控制的实体造成伤害，也可以从玩家控制的实体中受到伤害。
* **Projectiles** - 可以造成伤害的实体。

一个项目最多可以有 64 个 PhysX 碰撞层。您定义的图层特定于您的项目。将 **PhysX Collider** 组件添加到实体时，会为其分配一个索引为 **\[0\]** 的碰撞层，称为`Default`。您可以将每个碰撞器组件分配给一个层。一个实体可以有多个 Collider 组件分配给不同的层。

## 创建碰撞层

1. 在 O3DE 编辑器中，从 **Tools** 菜单中选择 **PhysX Configuration**。

1. 选择 **Collision Filtering**（冲突筛选）**选项卡。

1. 单击 **Layers** 按钮查看图层列表。

1. 在可用文本字段中键入图层的名称。图层名称不得超过 32 个字符。

    ![Creating Layers in the PhysX Configuration tool.](/images/user-guide/interactivity/physics/nvidia-physx/configuring/physx-configuration-2.png)

## 碰撞层分配

1. 在 **O3DE Editor** 中，选择要分配给碰撞层的具有 **PhysX Collider** 组件的实体。

1. 在 **PhysX Collider** 组件中，从 **Collision Layer** 属性中，从下拉列表中选择一个可用的碰撞层。

    ![The PhysX Collider component in the Entity Inspector.](/images/user-guide/interactivity/physics/nvidia-physx/configuring/physx-configuration-3.png)

{{< note >}} 
* 如果重命名图层，则其引用会自动更新。
* 图层无法重新排序。
* 如果在选择为该层分配了碰撞器的实体时创建、重命名或删除碰撞层，则更改不会显示在 **Entity Inspector**中。要查看更改，请取消选择并重新选择实体以刷新组件界面。
{{< /note >}}
