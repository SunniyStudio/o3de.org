---
description: '在 Houdini 中为 Open 3D Engine 创建 NVIDIA Blast 资产。 '
title: 为 NVIDIA Blast 创建资产
weight: 200
draft: true
---

NVIDIA Blast 的资产是使用提供的工具和插件在 Houdini 中创建的。按照以下步骤为 O3DE 创建和导出 Blast 资产。

{{< note >}}
这些步骤可应用于任何网格。为获得最佳效果，网格应完全封闭。自定义法线、UV 集和顶点颜色流可以应用于根网格和生成的块。
{{< /note >}}

**内容**
+ [为NVIDIA Blast分割网格](#fracture-a-mesh-for-nvidia-blast)
+ [为NVIDIA Blast导出资产](#export-an-asset-for-nvidia-blast)

## 为 NVIDIA Blast 打破网格

**使网格破裂**

1. 在 Houdini 中创建一个包含网格的对象。为避免在生成运行时资源时出现潜在警告和错误，请确保将 UV 和 Normal 属性添加到网格。

1. 在 **SOP** 级别，附加一个 **Name** SOP 到要打破的几何体。

1. 在 **Name** SOP 中，为 **Name** 参数输入 **root**。这会将 `name` 基元属性应用于值为 **root** 的几何体，以便可以识别根网格。

![Add a Name SOP in Houdini.](/images/user-guide/physx/blast/ui-blast-houdini-name-node.png)

1. 您可以使用 **Fracture Selection（破裂选择）** 工具自动设置破裂网络。在继续之前，请务必了解 NVIDIA Blast Gem 提供的两个 Houdini 数字资产，即 **Fracture Single** 和 **Fracture Hierarchy**：
   + **Fracture Single** 将提供的几何体碎裂为指定数量的块。通过将 **Fracture Single** SOP 附加到已经破裂的几何体，可以创建多级断裂。
   + **Fracture Hierarchy** 将断开提供的几何体，并对 **Fracture levels（破裂级别）** 参数指定的每个附加级别的结果数据块进行破裂。如果指定 **2** **Fracture levels** 和 **10** **Fractures per level**，则根几何体将碎裂成 10 个块，每个块将碎裂成 10 个块。

{{< caution >}}
**Fracture Hierarchy** 生成的数据块数量呈指数级增长。将 **Fracture levels** 和 **Fractures per level** 设置得太高可能会生成太大而无法加载和使用 NVIDIA Blast 进行模拟的资源。要对多个骨折级别进行精细控制，请使用多个 **Fracture Single** SOP。
{{< /caution >}}

{{< note >}}
**Fracture Single** 和 **Fracture Hierarchy** Houdini 数字资产 （HDA） 都是使用标准 Houdini SOP 创建的。您可以打开这些 HDA 以查看其网络内容，并将其用作项目自定义分离 SOP 的基础。
{{< /note >}}

   提供的 **Fracture Selection（破裂选择）** 工具添加了一个 **Fracture Single** SOP，并将根网格和破裂操作的结果块馈送到一个 **Merge** SOP 中。

   选择 **Name** SOP 后，从 **Fracture tools for Blast** 搁架中选择 **Fracture Selection**。将 **Fracture Single** SOP 附加到网络中，并与根网格一起输入到 **Merge** SOP 中。请注意，**Fracture Single** SOP 指定 **root** 作为 **Group to fracture**，将 **chunk** 指定为 **Chunk name prefix**。

   ![Fracture Selection in Houdini.](/images/user-guide/physx/blast/ui-blast-houdini-fracture-selection.png)

1. 要可视化断裂：

   1. 确保在 **Merge** SOP 上启用了 **Display** 标志。

   1. 从 **Perspective** 视区右侧的 **Display options** 工具栏中选择 **Display groups and attribute list** 切换。这将在 **Perspective** 视区中启用组和属性叠加。

   1. 在 groups and attributes overlay 中，选择 **Gear** 按钮以打开列表选项。

   1. 在列表选项中，选择 **Attributes**，然后选择 **name** 以按其 name 属性可视化几何体。您可以在列表中看到几何体的层次结构，并且网格块在透视视口中按其名称基元属性着色。

    {{< note >}}
将 Houdini 的几何体选择模式设置为基元后，您可以按 **S** 键进入选择模式，然后单击组和属性叠加层中任何级别的任何数据块，以选择该数据块及其后代。
{{< /note >}}

![Fracture Selection in Houdini.](/images/user-guide/physx/blast/ui-blast-houdini-fracture-visualize.png)

1. 如果根几何体很复杂，则可能很难看到网格的破裂位置。您可以使用 **Exploded View** 来更好地检查破裂结果。要查看破裂网格的爆炸视图：

   1. 将 **Partition** SOP 附加到 **Merge** SOP。

   1. 将 **Exploded View** SOP 附加到 **Partition** SOP。

   1. 在 **Exploded View** SOP 上启用 **Display** 标志。

   ![Fracture Selection in Houdini.](/images/user-guide/physx/blast/ui-blast-houdini-exploded-view.png)

1. Fractures 可以是分层的，也就是说，Fracture chunk 可以进一步断开。要添加其他破裂级别：

   1. 将 **Fracture Single** SOP 添加到网络。

   1. 将 **Fracture\_root** SOP 的输出连接到新添加的 **Fracture Single** SOP 的输入。

   1. 将新添加的 **Fracture Single** SOP 的输出连接到 **Merge** SOP 的输入。

   1. 在 **Fracture Single** SOP 中，指定要断开的数据块。在下面的示例中，使用路径 **root/chunk1** 在 **Group to fracture** 参数中指定 **chunk1**（兔子的脸）。

   ![Fracture layer in Houdini.](/images/user-guide/physx/blast/ui-blast-houdini-fracture-layer.png)

## 导出用于 NVIDIA Blast 的资源

网格破裂后，您必须导出 `.fbx` 和 `.blast` 资产，以便由 **Asset Processor** 处理，以便在 Open 3D Engine 中使用。

**导出资产**

1. 将 **Blast Export** SOP 添加到网络。

1. 将 **Merge** SOP 的输出连接到 **Blast Export** SOP 的输入。

1. 确保 **Root Name** 和 **Chunk name prefix** 参数设置正确。

1. 将 **Object name** 参数设置为所需的名称。导出的 `.fbx` 和 `.blast` 文件由此参数的值命名。

1. 设置 **Output Directory** 参数。默认情况下，`.fbx` 和 `.blast` 文件将导出到保存 Houdini 场景文件的目录中。

1. 禁用 **Static root** 参数。
   + 如果启用了 **Static root** 参数，则根网格是静态刚体，不受重力影响。足够的力会使根网格破裂，并且块会动态模拟。
   + 如果禁用 **Static root** 参数，则根网格是动态的，被视为刚体，并受重力影响。足够的力会使根网格破裂，并且块会动态模拟。

   {{< note >}}
所有数据块都模拟为动态刚体。您可以通过将单词 **static** 添加到非根块的 name primitive 属性中，将非根块创建为静态刚体。这对于您想要销毁实体的一部分同时保留实体的一部分的情况非常有用。

有关更多信息，请参阅 [使用 NVIDIA Blast 进行部分销毁](/docs/user-guide/interactivity/physics/nvidia-blast/static-chunks)。
{{< /note >}}

1. 选择 **Export Blast Asset** 生成一个 `.blast` 资产。

1. 选择 **Export FBX** 生成一个 `.fbx` 资产。

![Export blast assets from Houdini.](/images/user-guide/physx/blast/ui-blast-houdini-export.png)
