---
description: ' 利用 NVIDIA Blast 在Open 3D Engine 中创建逼真的破坏模拟。 '
title: 使用NVIDIA Blast模拟破坏
weight: 400
draft: true
---



要在 O3DE 中使用NVIDIA Blast资产，需要创建一个实体，添加一个**Blast Family**组件，添加一个**Blast Family Mesh Data**组件，然后将blast资产分配给这些组件。

{{< note >}}
为了快速测试 NVIDIA Blast 仿真，以下步骤假定资产是从 Houdini 导出的，并在**Blast Export** SOP 中禁用了**Static root**。在禁用**Static root**的情况下，NVIDIA Blast 资产是动态的，将实体投掷到 PhysX 碰撞面（如**PhysX Terrain**）上即可触发销毁。如果启用了**Static root**，根资产就是静态的，销毁必须由外力触发，如弹丸撞击。

更多信息，请参阅 [NVIDIA Blast创建资产](/docs/user-guide/interactivity/physics/nvidia-blast/create-blast-asset)。
{{< /note >}}

**内容**
+ [为NVIDIA Blast创建资产](#nvidia-blast-create-entity)
+ [将自动处理过的网格资产添加到 NVIDIA Blast 实体中](#nvidia-blast-entity-automatic-assets)
+ [将手动创建的网格资产添加到 NVIDIA Blast 实体中](#nvidia-blast-entity-manual-assets)
+ [测试 NVIDIA Blast 破坏模拟](#nvidia-blast-simulate-destruction)

## 为NVIDIA Blast创建资产 {#nvidia-blast-create-entity}

创建实体时，您将添加 NVIDIA Blast 功能并定义资产的销毁方式。

**创建 NVIDIA Blast 实体**

1. 确保地形有一个 **PhysX Terrain** 关卡组件。在**Level inspector**中，选择**Add Component**，然后从组件列表中选择**PhysX Terrain**。

1. 创建一个新实体。在**Perspective**中单击右键，从右键菜单中选择**Create entity**。

1. 在实体中添加一个**Blast Family**组件。在**Entity Inspector**中，选择**Add Component**，然后从组件列表中选择**Blast Family**。**Blast Family** 组件可为实体添加NVIDIA Blast功能。更多信息，请参阅 [Blast Family 组件](/docs/user-guide/components/reference/destruction/blast-family/)。

1. 为**Blast Family**组件设置**Blast asset**。单击**Blast asset**属性右侧的**Folder**按钮，在Blast asset选择窗口中选择`.blast`资产。

![Add the .blast asset to the Blast Family component.](/images/user-guide/physx/blast/ui-blast-add-blast-asset.png)

1. 为**Blast Family**组件设置**Blast Material**。爆破材料定义了各种力对将断裂资产连接在一起的键造成的破坏程度，以及造成破坏所需的破坏程度。更多信息请参阅[使用Blast材料指定破坏属性](/docs/user-guide/interactivity/physics/nvidia-blast/materials/)。

1. 在实体中添加**Blast Family Mesh Data**组件。在**Entity Inspector**中，选择**Add Component**，然后从组件列表中选择**Blast Family Mesh Data**。**Blast Family Mesh Data**组件会将NVIDIA Blast 网格添加到实体中。更多信息，请参阅 [Blast Family Mesh Data 组件](/docs/user-guide/components/reference/destruction/blast-family-mesh-data/)。

如果您已使用**Python Asset Builder**处理了网格资产，请按照本节中的步骤操作： [将自动处理过的网格资产添加到 NVIDIA Blast 实体中](#nvidia-blast-entity-automatic-assets)。

如果您使用 **FBX Settings** 手动编辑了网格资产，请按照本节中的步骤操作： [将手动创建的网格资产添加到 NVIDIA Blast 实体中](#nvidia-blast-entity-manual-assets)。

## 将自动处理过的网格资产添加到 NVIDIA Blast 实体中 {#nvidia-blast-entity-automatic-assets}

在处理NVIDIA Blast 资产时，**Python Asset Builder**会创建一个`blast_slice`资产。爆破片会自动将网格资产和材质添加到**Blast Family Mesh Data **组件中。

**添加自动处理的网格资产**

1. 在**Blast Family Mesh Data**组件中，为Blast网格设置材质。单击**Material**属性右侧的**Folder**按钮，然后从材质选择窗口中选择材质。

1. 在**Blast Family Mesh Data**组件中，设置**Blast Slice**属性。单击 **Blast Slice** 属性右侧的 **Folder** 按钮，并从 Blast Slice 选择窗口中选择资产。

   如果您想查看网格列表，请启用**Show mesh assets**属性。

   ![Add the blast slice asset to the Blast Family Mesh Data component.](/images/user-guide/physx/blast/ui-blast-add-blast-mesh-data.png)

1. 现在实体已设置为模拟销毁。继续浏览本节： [测试 NVIDIA Blast 破坏模拟](#nvidia-blast-simulate-destruction)。

## 将手动创建的网格资产添加到 NVIDIA Blast 实体中 {#nvidia-blast-entity-manual-assets}

如果您的NVIDIA Blast网格资产已在**FBX Settings**中进行了手动编辑，请使用以下步骤将网格资产添加到实体中。

**添加手动处理的资产**

1. 在**Blast Family Mesh Data**组件中，为爆炸网格设置材质。单击**Material**属性右侧的**Folder**按钮，然后从材料选择窗口中选择材料。

1. 在**Blast Family Mesh Data**组件中，启用**Show mesh assets**属性以显示网格资产列表。

1. 在列表中添加一个网格槽。选择**Mesh assets**右侧的**+**按钮，在**Mesh assets**列表中添加网格槽。

1. 在列表中添加网格。单击编号网格槽属性右侧的**Folder**按钮，然后从**Static Mesh**选择窗口中选择一个网格资产。

1. 重复步骤**3**和**4**，直到blast资产的所有网格都添加到**Blast family mesh data**组件中。

![Add mesh assets manually to the Blast Family Mesh Data component.](/images/user-guide/physx/blast/ui-blast-family-mesh-data-add-mesh.png)

1. 现在实体已设置为模拟销毁。继续浏览本节： [测试 NVIDIA Blast 破坏模拟](#nvidia-blast-simulate-destruction)。

## 测试 NVIDIA Blast 破坏模拟 {#nvidia-blast-simulate-destruction}

由于从 Houdini 导出的爆炸资产已禁用**Static root**，并且已在关卡中添加了**PhysX Terrain**关卡组件，因此可通过将物体投掷到地形上测试破坏情况。

**测试破坏模拟**

1. 选择实体后，按**2**键启用移动工具。

1. 点击并拖动移动小工具的**Z**轴，将实体移动到地形上方几个单位的位置。

1.  按**Ctrl+P**查看模拟。实体与地形碰撞后会掉落并破碎。

1.  按**Ctrl+P**结束模拟。

![Add the blast slice asset to the Blast Family Mesh Data component.](/images/user-guide/physx/blast/anim-nvidia-blast-view-simulation.gif)
