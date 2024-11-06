---
linkTitle: PhysX Ragdoll
description: 使用 PhysX Ragdoll （人偶） 组件在 Open 3D Engine （O3DE） 动画编辑器中创建角色的物理表示。
title: PhysX Ragdoll 组件
---



您可以使用 **PhysX Ragdoll** 组件在动画系统中创建角色的物理表示，并模拟某些行为，例如命中反应和角色死亡。物理表示由具有简单形状的刚体层次结构组成，这些刚体通过关节连接。您可以根据需要调整人偶设置，以获得物理真实性和性能。

**PhysX Ragdoll**组件需要 [PhysX](/docs/user-guide/gems/reference/physics/nvidia/physx/) gem。

有关 PhysX 系统的更多信息，请参阅[使用 PhysX 系统模拟物理行为](/docs/user-guide/interactivity/physics/nvidia-physx/)。

## 使用PhysX Ragdoll组件 

您可以使用 PhysX 系统和 **Animation Editor** 创建人偶。

**使用 PhysX Ragdoll 组件**

1. 在 O3DE Editor 中，将 **PhysX Ragdoll** 组件添加到表示角色的实体中。有关详细信息，请参阅 [向实体添加组件](/docs/user-guide/components/reference/#adding-components-to-an-entity)。

1. 选择 **Tools**, **Animation Editor**。

1. 使用 **Animation Editor** 创建和控制人偶的物理表示形式。有关详细信息，请参阅 [创建和模拟 PhysX 人偶](/docs/user-guide/visualization/animation/animation-editor/creating-and-simulating-physx-ragdoll/)。

## PhysX Ragdoll 组件属性 

![PhysX Ragdoll component properties.](/images/user-guide/component/physx/ui-physx-ragdoll-component-properties.png)


****

| 属性 | 说明 |
| --- | --- |
| Position Iteration Count |  指定用于关节稳定性和准确性的迭代次数。 迭代计数越高，行为越真实，但性能越低。 较低的迭代计数可能会导致不切实际的行为，例如关节分离以及人偶的某些部分与地形相交。默认值：`16` 有效值：`1`到`255`  |
| Velocity Iteration Count |  指定用于解决碰撞的迭代次数，例如恢复 （弹性） 和刚体相交。 迭代次数越多，根据材质设置解决碰撞，但会降低性能。 使用较低的迭代计数来减少人偶的恢复。 默认值：`16` 有效值：`1`到`255`  |
| Enable Joint Projection |  如果设置，则关节投影将在要求苛刻的情况下保留关节约束，例如人偶的某些部分以高能量移动。此设置可能会提高物理合理性。默认启用。  |
| Joint Projection Linear Tolerance |  PhysX 系统在每个关节中允许的最大线性偏差。由于抖动，不建议使用小于`0.001`米的值。 要编辑此属性，必须设置 **Enable Joint Projection** 属性。默认值： `0.001`  |
| Joint Projection Angular Tolerance |  PhysX 系统在每个关节中允许的最大角度偏差。由于抖动，不建议使用小于 `1` 度的值。 要编辑此属性，必须设置 **Enable Joint Projection** 属性。默认值： `1`  |
