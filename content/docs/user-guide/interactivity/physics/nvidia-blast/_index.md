---
description: ' 使用 NVIDIA Blast 在 Open 3D Engine 中创建逼真的破坏模拟。 '
linktitle: NVIDIA Blast
title: 使用 NVIDIA Blast 进行模拟破坏
weight: 200
draft: true
---

借助 Open 3D Engine 中的 **NVIDIA Blast**，您可以通过在 SideFX Houdini 中编写爆炸资产并使用 **Blast Family** 和 **Blast Family Mesh Data** 组件创建实体来模拟破坏。

要使用 **NVIDIA Blast**，您必须启用 [NVIDIA Blast gem](/docs/user-guide/gems/reference/physics/nvidia/nvidia-blast/)。

{{< note >}}
适用于 O3DE 的 NVIDIA Blast 需要 SideFX Houdini 商业或独立许可证才能创建资产。学徒执照是不够的。有关 Houdini 的更多信息，请参阅 [SideFX 的主页](https://www.sidefx.com/)。

随 **NVIDIA Blast** Gem 提供的预编译 Houdini 插件需要 Houdini 18.0。
{{< /note >}}

## NVIDIA Blast特征

以下是 NVIDIA Blast 提供的功能：
+ 使用提供的 Houdini 插件和 Houdini 数字资产 （HDA） 在 SideFX Houdini 中断开几何体并创作爆炸资产。
+ 创建多个级别的破坏以进行模拟。
+ 使用提供的 Python Asset Builder for NVIDIA Blast 自动处理和快速设置资产。
+ 创建爆炸材料以定义在爆炸资产上触发破坏模拟所需的力。
+ 将 NVIDIA Blast 资产添加到具有**Blast Family** 和 **Blast Family Mesh Data**组件的实体。
+ 使用为 NVIDIA Blast 提供的 **Script Canvas** 节点进行脚本销毁。
+ 使用实时调试可视化来调试 NVIDIA Blast 模拟。

## 使用 NVIDIA Blast

有关 NVIDIA Blast 的详细信息，请参阅以下主题。

[Blast Family 组件](/docs/user-guide/components/reference/destruction/blast-family/) - Blast Family 组件参考

[Blast Family Mesh Data 组件](/docs/user-guide/components/reference/destruction/blast-family-mesh-data/) - Blast Family Mesh Data组件参考

[安装 NVIDIA Blast 插件](/docs/user-guide/interactivity/physics/nvidia-blast/install-houdini-plugin/#install-nvidia-blast-plug-ins) - 安装 NVIDIA Blast 插件和 Houdini 数字资产。

[创建NVIDIA Blast资产](/docs/user-guide/interactivity/physics/nvidia-blast/create-blast-asset/) - 在 Houdini 中断开网格并导出 NVIDIA Blast 的资产。

[处理NVIDIA Blast资产](/docs/user-guide/interactivity/physics/nvidia-blast/process-blast-asset/) - 处理 O3DE 的 NVIDIA Blast 资产。

[使用NVIDIA Blast模拟破坏](/docs/user-guide/interactivity/physics/nvidia-blast/) - 使用 NVIDIA Blast 资产创建实体并模拟破坏。

[使用NVIDIA Blast模拟部分破坏](/docs/user-guide/interactivity/physics/nvidia-blast/static-chunks/) - 使用 attributes 创建 partial destruction。

[使用 Blast 材质指定破坏属性](/docs/user-guide/interactivity/physics/nvidia-blast/materials/) - 使用Blast材质定义触发破坏所需的力。

[NVIDIA Blast 可视化调试器](/docs/user-guide/interactivity/physics/nvidia-blast/debug/) - 使用 NVIDIA Blast 的可视化调试器。

[NVIDIA Blast的Script Canvas节点](/docs/user-guide/interactivity/physics/nvidia-blast/script-canvas/) - Script Canvas 中的脚本销毁模拟。

## NVIDIA Blast 参考 

 [NVIDIA Blast 文档](https://developer.nvidia.com/blast) 位于NVIDIA GAMEWORKS开发者中心。
