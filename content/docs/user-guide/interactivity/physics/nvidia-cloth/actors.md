---
description: ' 使用 NVIDIA Cloth 组件将布料模拟添加到 Open 3D Engine 中的 Actor 组件。 '
title: Actor 组件的布料
weight: 200
---

要使用 **Cloth**，您必须启用 **NVIDIA Cloth **Gem。有关更多信息，请参阅[NVIDIA Cloth gem](/docs/user-guide/gems/reference/physics/nvidia/nvidia-cloth/) 文档。

您可以在所选内容创建应用程序中为具有 **Actor** 组件的实体创建布料资产，并将其从`.fbx`文件导入 O3DE。角色资产应具有以下内容：

**Actor mesh**
一个或多个直观地表示角色的网格，蒙皮到骨架上，它们将 **不会** 模拟为布料。

**Cloth mesh**
一个或多个将被模拟并渲染为 Cloth 的网格。
+ 必须将布网蒙皮到骨骼。骨骼不必是布料网格的独占对象。骨骼必须是 actor 的骨架层次结构的一部分。由于模拟将驱动布料网格，因此我们建议您为布料网格使用少量其他骨骼。
+ 可以使用内容创建应用程序中的顶点颜色工具添加布料数据以定义每个顶点的质量和约束属性。有关详细信息，请参阅 [布料的逐顶点属性](/docs/user-guide/interactivity/physics/nvidia-cloth/vertex-data/)。

**Skeleton**
用于驱动 actor 和 cloth 网格的骨架。布料网格可以蒙皮到它们自己的骨骼或层次结构中的任何骨骼。驱动布料网格的骨骼必须是骨架层次结构的一部分。

**Animation**
基于角色骨架的 **Motion set** 和 **Anim graph** 。关键帧动画布料可以使用 **Motion constraints** 与模拟布料混合。

{{< note >}}
示例 **Actor** 组件布料资源位于 **NVIDIA Cloth** gem 目录中，该目录位于`/dev/Gems/NvCloth/Assets/Objects/cloth/Chicken/`.
{{< /note >}}

有关导出角色资源的信息，请参阅 [FBX 设置角色导出](/docs/user-guide/assets/scene-settings/actors-tab/).

## 添加 Cloth 组件到具有 Actor 组件的实体

通过将 **Cloth** 组件添加到具有 **Actor** 组件的实体，然后设置 **Cloth ** 组件的属性来创建布料。

1. 在 O3DE 编辑器中，向关卡添加新实体。

1. 将 **Actor** 组件添加到实体，并引用 Actor 资产和材质。

1. 添加 **Anim Graph** 组件并引用角色动画图形资产和运动集。

1. 向实体添加 **Cloth** 组件。

1. 设置 actor 资源的 cloth 数据。

   1. 单击 **Mesh node** 属性旁边的按钮以打开 **FBX Settings** 窗口。

   ![Open 3D Engine cloth component mesh node select.](/images/user-guide/physx/cloth/ui-cloth-mesh-node-select.png)

   1. 在 **FBX Settings** 窗口的 **Meshes** 选项卡上，依次选择 **Add Modifier**、**Cloth**。

   1. 在 **Cloth** 修改器区域中：

      1. 从下拉列表中选择布料网格。

      1. 如果适用，请选择包含 **Inverse Mass （反质量） 数据的顶点颜色流和通道。如果未提供数据，则 cloth 默认为所有顶点的反向质量值 1.0。

      1. 如果适用，请选择包含 **Motion Constraints** 数据的顶点颜色流和通道。如果未提供 data，则 cloth 默认为所有顶点的运动约束值 1.0。

      1. 如果适用，请选择包含 **Backstop Offset** 和 **Backstop Radius** 数据的顶点颜色流和通道。如果未提供数据，则模拟中将不应用任何 backstop 约束。

      ![Open 3D Engine cloth modifier setup.](/images/user-guide/physx/cloth/ui-cloth-modifier-actor-setup.png)

   1. 选择 **Update** （更新） 按钮。然后，Asset Processor 会更新资源并包含布料数据。

1. 配置 cloth 组件。

   1. 从下拉列表中选择 cloth mesh 节点。

   ![Open 3D Engine cloth component.](/images/user-guide/physx/cloth/ui-cloth-component-select-actor.png)

   1. 调整 Cloth 属性以获得所需的 Cloth 行为。有关详细信息，请参阅 [Cloth 组件](/docs/user-guide/components/reference/physx/cloth/).

   1. 您可以使用 **Motion constraints** 属性 **Max Distance** 和 **Scale** 在布料模拟和关键帧动画之间进行混合。

## 向角色添加布料碰撞器

您可以向角色添加布料碰撞器，以防止布料形状在模拟期间穿透角色的网格。布料碰撞器已添加到 **Animation Editor** 中的角色中。有关向角色添加布料碰撞器的信息，请参阅 [向角色添加布料碰撞器](/docs/user-guide/visualization/animation/character-editor/cloth-colliders/).

## 查看 Cloth Simulation

在 O3DE 编辑器中，按 **Ctrl+G** 或按 **Play** 按钮来运行您的项目。

![Open 3D Engine cloth simulation with the NVIDIA Cloth gem.](/images/user-guide/physx/cloth/anim-actor-cloth.gif)
