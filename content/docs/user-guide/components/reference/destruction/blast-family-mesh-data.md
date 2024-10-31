---
linkTitle: Blast Family Mesh Data
description: ' 了解 Open 3D Engine中的Blast Family Mesh Data 组件。 '
title: Blast Family Mesh Data 组件
draft: true
---




通过**Blast Family Mesh Data** 组件，您可以为 NVIDIA Blast 实体设置网格和材质资产。**Blast Family Mesh Data**组件与**Blast Family**组件一起使用。本主题将介绍**Blast Family Mesh Data**组件的属性。

**Blast Family Mesh Data**组件由[NVIDIA Blast gem](/docs/user-guide/gems/reference/physics/nvidia/nvidia-blast/)提供。

有关使用**Blast Family Mesh Data**组件的信息，请参阅[使用NVIDIA Blast模拟破坏](/docs/user-guide/interactivity/physics/nvidia-blast/)。

## Blast Family Mesh Data 组件属性

![Properties of the Blast Family Mesh Data component](/images/user-guide/physx/blast/ui-blast-family-mesh-data-component.png)

**Show mesh assets**
启用后，您可以手动将网格资产添加到此**Blast Family Mesh Data**组件中。
**Show mesh assets**属性会显示**Mesh assets** 列表。选择**Mesh assets**右侧的 **+** 按钮可添加一个列表条目。选择**Folder**按钮，为列表条目选择网格。

**Material**
在此**Blast Family Mesh Data**组件中，为网格提供视觉外观的材质。

**Blast Slice**
该**Blast Family Mesh Data**组件的爆炸切片。NVIDIA Blast 的 Python 资产生成器在处理爆炸资产时会生成爆炸切片。爆破片会自动将网格资产和材质添加到**Blast Family Mesh Data**组件中。
