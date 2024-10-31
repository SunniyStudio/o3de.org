---
linkTitle: Blast Family
title: Blast Family 组件
description: ' 了解 Open 3D Engine 中的 Blast Family 组件。 '
draft: true
---



通过**Blast Family** 组件，您可以使用NVIDIA Blast 库启用破坏模拟，并为模拟设置属性。**Blast Family**组件与**Blast Family Mesh Data**组件一起使用。本主题将介绍**Blast Family**组件的属性。

**Blast Family**组件由[NVIDIA Blast Gem](/docs/user-guide/gems/reference/physics/nvidia/nvidia-blast/)提供。

有关使用**Blast Family**组件的信息，请参阅 [使用 NVIDIA Blast 模拟破坏](/docs/user-guide/interactivity/physics/nvidia-blast/).

## Blast Family 组件属性

![Properties of the Blast Family component](/images/user-guide/physx/blast/ui-blast-family-component.png)

**Blast Asset**
用于模拟破坏的blast资产。

**Blast Material**
blast材料库中的爆炸材料。blast材料定义了各种力对固定断裂资产的键造成的破坏程度，以及触发破坏所需的破坏程度。更多信息，请参阅[使用Blast材料指定破坏属性](/docs/user-guide/interactivity/physics/nvidia-blast/materials/)。

**Physics Material**
Blast资产的物理材料。物理材料定义了摩擦等物理属性。

**Collision Layer**
此**Blast Family**的碰撞层。

**Collides With**
碰撞组，包含与该**Blast Family**碰撞的层。

**Simulated**
启用后，该**Blast Family**的碰撞将成为 PhysX 模拟的一部分。

**In Scene Queries**
启用后，该**Blast Family**的碰撞器将可用于场景查询。

**CCD Enabled**
启用后，该**Blast Family**将使用连续碰撞检测。**CCD**可确保对高速物体进行精确的碰撞检测。

  **Tag**
  为该**Blast Family**设置标签。标签可用于快速识别脚本或代码中的组件。
