---
description: ' 使用 NVIDIA Cloth 组件将 Cloth 模拟添加到 Open 3D Engine 中的网格组件。 '
title: Mesh 组件的 Cloth
weight: 100
---

要使用 **Cloth**，您必须启用 **NVIDIA Cloth** Gem。有关更多信息，请参阅[NVIDIA Cloth Gem](/docs/user-guide/gems/reference/physics/nvidia/nvidia-cloth/) 文档。

您可以在所选内容创建应用程序中为具有 **Mesh** 组件的实体创建布料资产，并将它们从“.fbx”文件导入到 O3DE 中。网格资产应包含以下内容：
+ 将在 O3DE 中模拟和渲染的布料网格。
  + 可以使用内容创建应用程序中的顶点颜色工具添加布料数据以定义每个顶点的质量和约束属性。有关详细信息，请参阅 [布料的逐顶点属性](/docs/user-guide/interactivity/physics/nvidia-cloth/vertex-data/).
+ **可选** - 任何其他静态网格。例如，如果创建要模拟为 Cloth 的旗帜，则可以包含旗杆的网格。

{{< note >}}
示例 **网格** 组件布料资源位于 **NVIDIA Cloth** gem 目录中，该目录位于`/dev/Gems/NvCloth/Assets/Objects/cloth/Environment/`.
{{< /note >}}

有关导出网格资源的信息，请参阅 [FBX 设置网格导出](/docs/user-guide/assets/scene-settings/meshes-tab/).

## Add Cloth to Mesh 组件

通过将 **Cloth** 组件添加到具有 **Mesh** 组件的实体，然后设置 **Cloth** 组件的属性来创建布料。

1. 在 O3DE 编辑器中，向关卡添加新实体。

1. 向实体添加 **Mesh** 组件，并引用网格资产和材料。

1. 向实体添加 **Cloth** 组件。

1. 设置网格资源的 cloth 数据。

   1. 单击 **Mesh node** 属性旁边的按钮以打开 **FBX Settings** 窗口。

   ![Open 3D Engine cloth component mesh node select.](/images/user-guide/physx/cloth/ui-cloth-mesh-node-select.png)

   1. 在 **FBX Settings** 窗口的 **Meshes** 选项卡上，依次选择 **Add Modifier**、**Cloth**。

   1. 在 **Cloth** 修改器区域中：

      1. 从下拉列表中选择布料网格。

      1. 如果适用，请选择包含 **Inverse Mass** 数据的顶点颜色流和通道。如果未提供数据，则 cloth 默认为所有顶点的反向质量值 1.0。

      1. 如果适用，请选择包含 **Motion Constraints** 数据的顶点颜色流和通道。如果未提供 data，则 cloth 默认为所有顶点的运动约束值 1.0。

      1. 如果适用，请选择包含 **Backstop Offset** 和 **Backstop Radius** 数据的顶点颜色流和通道。如果未提供数据，则模拟中将不应用任何 backstop 约束。

      ![Open 3D Engine cloth modifier setup.](/images/user-guide/physx/cloth/ui-cloth-modifier-mesh-setup.png)

   1. 选择 **Update** （更新） 按钮。然后，Asset Processor 会更新资源并包含布料数据。

1. 配置 cloth 组件。

   1. 从下拉列表中选择 cloth mesh 节点。

   ![Open 3D Engine cloth component.](/images/user-guide/physx/cloth/ui-cloth-component-select-mesh.png)

   1. 调整 Cloth 属性以获得所需的 Cloth 行为。有关详细信息，请参阅 [Cloth 组件](/docs/user-guide/components/reference/physx/cloth/).

## 查看 Cloth 模拟

在 O3DE 编辑器中，按 Ctrl+G 或按 **Play** 按钮来运行您的项目。

![Open 3D Engine cloth simulation with the NVIDIA Cloth gem.](/images/user-guide/physx/cloth/anim-mesh-cloth.gif)
