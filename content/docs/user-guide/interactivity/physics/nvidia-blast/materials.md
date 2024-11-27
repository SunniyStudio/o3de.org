---
description: ' 使用 Blast 材质在 NVIDIA Blast 的 Open 3D 引擎中设置破坏属性。 '
title: 使用 Blast 材质指定破坏属性
weight: 600
draft: true
---


爆炸资产块由结合力固定在一起。喷砂材料决定了哪些类型的力会损坏键、可能损坏键的最小力以及键在断裂前可以承受多少损坏。

{{< important >}}
Blast 材质是创建逼真的可破坏实体的关键组成部分。重力等被动力可能会对爆炸资产产生累积伤害效果，当没有明显的外力或碰撞器作用在资产上时，会导致触发破坏。

大型可破坏资产（如建筑物）具有大块，需要更强的键和更高的伤害阈值，以防止过早触发破坏。
{{< /important >}}

**Contents**
+ [Create Blast materials](#create-blast-materials)
+ [NVIDIA Blast configuration](#nvidia-blast-configuration)
+ [Blast material properties](#blast-material-properties)

## 创建 Blast 材质

爆炸材料存储在爆炸材料库中。项目的 Blast 材质库在 **Blast Configuration** 编辑器中设置。您可以在实体的 **Blast Family** 组件中分配爆炸材料。

**创建爆炸材质**

1. 在 **Tools** 菜单中，选择 **Asset Editor**。

1. 在 **Asset Editor **中，从 **File** 菜单中选择 **New**，然后选择 **Blast Material**。新的 **Blast Material** 将显示在 **Asset Editor** 中。

1. 设置 **Material name** 属性。此属性是用于将爆炸材料分配给 **Blast Family** 组件的标识符。

1. 设置爆炸材料属性。有关信息，请参阅下面的 [Blast 材料属性](#blast-material-properties) 。

1. 可以通过选择 Blast 材质列表右上角的 **+** 按钮来创建其他 Blast 材质。

1. 保存 Blast 材质库。在 **Asset Editor **的 **File** 菜单中，选择 **Save as**。

1. 在 **Save as...** 窗口中，导航到项目中的相应目录，为爆炸材质库命名，然后选择 **Save**。

## NVIDIA Blast 配置 

项目的 Blast 材质库在 **Blast Configuration **中设置。

**使用 Blast 配置**

1. 从 **Tools** 菜单中，选择 **Blast Configuration**。

![Blast configuration.](/images/user-guide/physx/blast/ui-blast-configuration.png)

1. 为您的项目设置 **Blast material library**。选择 **Blast material library** 右侧的 **Folder** 按钮，然后选择您创建的 Blast 材料库。

1. 设置 **Stress solver iterations** 属性。应力损伤是由冲击力产生的。此属性设置应力求解器为每个 **Blast Family** 组件计算的每时钟周期的迭代次数。有效值范围为 **0** 到 **50000**。

现在，您可以将爆炸材料库中的爆炸材料分配给 **Blast Family** 组件。

## Blast 材质属性

爆炸材料的作用取决于 *生命值* 和 *伤害* 的概念。块通过键固定在一起。每个羁绊都有一定的生命值。对键的伤害是由作用于块的 *脉冲力* 造成的。脉冲力以两种方式产生伤害：
+ **Vector damage向量伤害** 是由施加到 Blast 实体的冲击波、风或重力等冲击力产生的。
+ **Stress damage压力损伤** 是由撞击产生的冲击力产生的，例如弹丸与 Blast 实体碰撞。

![Blast material properties.](/images/user-guide/physx/blast/ui-blast-material.png)

**Material name**
材质的名称。使用此标识符可从 **Blast 系列** 组件的库中选择材质。

**Health**
每个 bond 的默认运行状况为 **1.0**。**Health** 属性乘以默认绑定运行状况，以设置 Blast 资产中所有绑定的运行状况。
向量伤害从 **Health** 中减去。当 **Health** 达到 0.0 时，该键将被销毁。
压力损伤将 **Health** 视为阈值。除非在给定的刻中超过阈值，否则不会施加伤害。

**Force Divider**
Force Divider 是一个 *硬度* 除数，它将脉冲力的尺度保持在合理的范围内，以限制应力损伤。
矢量伤害以标量形式给出，不受 **Force Divider** 的影响。
应力损伤计算考虑了连接键的数量，使资产更难销毁。应力损伤的计算公式为 `StressDamage=ImpulseForce/(ForceDivider*ConnectedBonds)`.

**Minimum damage threshold**
向量伤害的 floor 值。如果矢量伤害值小于最小阈值，则忽略伤害。如果矢量损伤值大于最小阈值，则根据 **Maximum damage threshold** 检查矢量损伤值。

**Maximum damage threshold**
向量伤害的上限值。如果矢量伤害值小于最大阈值，则从 **Health** 中减去伤害。如果向量伤害值大于最大阈值，则仅从 **Health** 中减去 **Maximum damage threshold**。

**Stress linear factor**
在应力损伤期间应用于线性力的系数。默认值为 **1.0**。

**Stress angular factor**
在应力损伤期间应用于角力的系数。默认值为 **1.0**。
