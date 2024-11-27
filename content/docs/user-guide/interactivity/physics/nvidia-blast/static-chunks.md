---
description: ' 使用 NVIDIA Blast 在 Open 3D Engine 中创建逼真的部分破坏模拟。 '
title: 使用 NVIDIA Blast 进行部分销毁
weight: 500
draft: true
---

在某些情况下，您可能希望部分销毁实体。例如，您创建了一堵可破坏的墙，但希望在墙的顶部受到射弹的伤害并被摧毁后，墙的底部作为带有碰撞体的静态网格保持原位。您可以通过在 Houdini 中将 **static** 添加到非根网格块的 `name` 基元属性来实现这一点。

## 在 NVIDIA Blast 中将非根数据块设为静态

使用 Houdini 将非根数据块设为静态数据块，这会导致资产部分销毁。

**使非根块成为静态的**

1. 在 Houdini 中，将 **Attribute String Edit** SOP 添加到网络。

1. 将 **Attribute String Edit** SOP 连接到网络。

   1. 将 **Merge** SOP 的输出连接到 **Attribute String Edit** SOP 的输入。

   1. 将 **Attribute String Edit** SOP 的输出连接到 **Blast Export** SOP 的输入。

1. 在 **Attribute String Edit** SOP 的 **Attributes** 选项卡中，启用 **Primitives** 参数，然后从其列表中选择 **name** 属性。

1. 在 **Attribute String Edit** SOP 的 **Filter** 选项卡中，将 **From** 参数设置为要设为静态的数据块的路径名称，例如 **root/chunk5**。

1. 在 **Attribute String Edit** SOP 的 **Filter** 选项卡中，将 **To** 参数设置为与上述相同的路径。将 **static** 附加到路径;例如，**root/chunk5static**。

{{< important >}}
如果指定的块已断开，则其后代在导出到 O3DE 时也是静态的。
{{< /important >}}

1. 您可以向 **Attribute String Edit** SOP 添加其他数据块。选择 **Number of filters** 参数旁边的 **+** 图标以添加筛选条件。重复步骤 **4** 和 **5** 以使另一个数据块静态。

   在下图中，构成兔子后半部分的块已变为静态。请注意，12 个过滤器已添加到 **Attribute String Edit** SOP 的过滤器列表中。您可以在透视视区的 groups and attributes 叠加中看到重命名的数据块。

   ![Create static chunks in Houdini for NVIDIA Blast.](/images/user-guide/physx/blast/ui-blast-houdini-static-chunks.png)

1. 在导出资产之前，在 **Blast Export** SOP 中启用 **Static root** 参数。

请参阅下面的 O3DE 中的结果模拟。一个大型的、不可见的 PhysX Dynamic Rigid Body 碰撞器被放置在兔子上。兔子的前半部分被毁坏了。这些块被模拟为动态刚体，而兔子的背部保持在原位。

![Create static chunks in Houdini for NVIDIA Blast.](/images/user-guide/physx/blast/anim-nvidia-blast-static-simulation.gif)
