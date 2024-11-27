---
description: ' 使用 NVIDIA Blast 在 Open 3D Engine 中创建逼真的破坏模拟。 '
title: 使用 NVIDIA Blast 模拟破坏
weight: 400
draft: true
---


要在 O3DE 中使用 NVIDIA Blast 资产，请创建一个实体，添加一个 **Blast Family** 组件，添加一个 **Blast Family Mesh Data** 组件，然后将 Blast 资产分配给组件。

{{< note >}}
为了快速测试 NIVIDIA Blast 模拟，以下步骤假定资产已从 Houdini 导出，并在 **Blast Export** SOP 中禁用了 **Static root**。禁用 **Static root** 后，NVIDIA Blast 资产是动态的，可以通过将实体放置在 PhysX 碰撞表面（如 **PhysX 地形）上来触发破坏。如果启用了 **Static root**，则根资源是静态的，并且必须由外力（例如弹丸撞击）触发破坏。

有关更多信息，请参阅 [为 NVIDIA Blast 创建资产](/docs/user-guide/interactivity/physics/nvidia-blast/create-blast-asset).
{{< /note >}}

**内容**
- [为 NVIDIA Blast 创建实体](#create-an-entity-for-nvidia-blast)
- [将自动处理的网格资源添加到 NVIDIA Blast 实体](#add-automatically-processed-mesh-assets-to-a-nvidia-blast-entity)
- [将手动创建的网格资产添加到 NVIDIA Blast 实体](#add-manually-created-mesh-assets-to-a-nvidia-blast-entity)
- [测试 NVIDIA Blast 破坏模拟](#test-nvidia-blast-destruction-simulation)

## 为 NVIDIA Blast 创建实体

创建实体时，您可以添加 NVIDIA Blast 功能并定义资产的销毁方式。

**为 NVIDIA Blast 创建实体**

1. 确保地形具有 **PhysX Terrain** 关卡组件。在 **Level inspector**中，选择 **Add Component**，然后从组件列表中选择 **PhysX Terrain**。

1. 创建新实体。右键单击 **Perspective**，然后从上下文菜单中选择 **Create entity**。

1. 将 **Blast Family** 组件添加到实体中。在 **Entity Inspector** 中，选择 **Add Component**，然后从组件列表中选择 **Blast Family**。**Blast Family** 组件将 NVIDIA Blast 功能添加到实体中。有关详细信息，请参阅 [Blast Family 组件](/docs/user-guide/components/reference/destruction/blast-family/)。

1. 为 **Blast Family** 组件设置 **Blast Asset**。单击 **Blast asset** 属性右侧的 **Folder** 按钮，然后在 Blast Asset 选择窗口中选择`.blast`资源。

![Add the .blast asset to the Blast Family component.](/images/user-guide/physx/blast/ui-blast-add-blast-asset.png)

1. 为 **Blast Family** 组件设置 **Blast Material**（**Blast 材质**）。爆炸材料定义了各种力对将断裂资产固定在一起的键造成的损害程度，以及造成破坏所需的损害程度。有关详细信息，请参阅 [使用 Blast 材质指定破坏属性](/docs/user-guide/interactivity/physics/nvidia-blast/materials/).

1. 将 **Blast Family Mesh Data** 组件添加到实体。在 **Entity Inspector** 中，选择 **Add Component** 并从组件列表中选择 **Blast Family Mesh Data**。**Blast Family Mesh Data** 组件将 NVIDIA Blast 网格添加到实体中。有关详细信息，请参阅 [Blast Family Mesh Data 组件](/docs/user-guide/components/reference/destruction/blast-family-mesh-data/).

如果您已使用 Python Asset Builder 处理网格资产，请按照以下部分中的步骤操作：[将自动处理的网格资产添加到 NVIDIA Blast 实体](#add-automatically-processed-mesh-assets-to-a-nvidia-blast-entity).

如果您已使用 **FBX Settings** 手动编辑了网格资产，请按照以下部分中的步骤操作：[将手动创建的网格资产添加到 NVIDIA Blast 实体](#add-manually-created-mesh-assets-to-a-nvidia-blast-entity).

## 将自动处理的网格资产添加到 NVIDIA Blast 实体

**Python Asset Builder** 在处理 NVIDIA Blast 资产时创建 `blast_slice`资产。爆炸切片会自动将网格资产和材质添加到 **Blast Family Mesh Data**组件。

**添加自动处理的网格资源**

1. 在 **Blast Family Mesh Data** 组件中，为 Blast 网格设置材质。单击 **Material** 属性右侧的 **Folder** 按钮，然后从材质选择窗口中选择一种材质。

1. 在 **Blast Family Mesh Data** 组件中，设置 **Blast Slice** 属性。单击 **Blast Slice** 属性右侧的 **Folder** 按钮，然后从 Blast Slice 选择窗口中选择资产。

   如果要查看网格列表，请启用 **Show mesh assets** 属性。

   ![Add the blast slice asset to the Blast Family Mesh Data component.](/images/user-guide/physx/blast/ui-blast-add-blast-mesh-data.png)

1. 现在，该实体已设置为模拟破坏。继续阅读该部分：[测试 NVIDIA Blast 破坏模拟](#test-nvidia-blast-destruction-simulation).

## 将手动创建的网格资产添加到 NVIDIA Blast 实体

如果您的 NVIDIA Blast 网格资产已在 **FBX Settings** 中手动编辑，请使用以下步骤将网格资产添加到实体。

**添加手动处理的资产**

1. 在 **Blast Family Mesh Data** 组件中，为 Blast 网格设置材质。单击 **Material** 属性右侧的 **Folder** 按钮，然后从材质选择窗口中选择一种材质。

1. 在 **Blast family mesh data** 组件中，启用 **Show mesh assets** 属性以显示网格资源列表。

1. 向列表中添加网格槽。选择 **Mesh assets** 右侧的 **+** 按钮，将网格槽添加到 **Mesh assets** 列表中。

1. 将网格添加到列表中。单击编号网格插槽属性右侧的 **Folder** 按钮，然后从 **Static Mesh** 选择窗口中选择一个网格资源。

1. 重复步骤 **3** 和 **4**，直到 Blast 资产的所有网格都已添加到 **Blast family mesh data** 组件中。

![Add mesh assets manually to the Blast Family Mesh Data component.](/images/user-guide/physx/blast/ui-blast-family-mesh-data-add-mesh.png)

1. 现在，该实体已设置为模拟破坏。继续阅读该部分：[测试 NVIDIA Blast 破坏模拟](#test-nvidia-blast-destruction-simulation).

## 测试 NVIDIA Blast 破坏模拟

由于爆炸资产是从 Houdini 导出的，并且禁用了 **Static root**，并且已将 **PhysX Terrain** 级别组件添加到关卡中，因此可以通过将对象拖放到地形上来测试破坏。

**测试破坏模拟**

1. 选择实体后，按 **2** 键以启用移动工具。

1. 单击并拖动移动 Gizmo 的 **Z** 轴，将实体移动到地形上方几个单位。

1. 按 **Ctrl+P** 查看模拟。实体在与地形碰撞时掉落并碎裂。

1. 按 **Ctrl+P** 结束模拟。

![Add the blast slice asset to the Blast Family Mesh Data component.](/images/user-guide/physx/blast/anim-nvidia-blast-view-simulation.gif)
