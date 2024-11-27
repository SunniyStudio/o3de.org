---
description: ' 使用 NVIDIA Cloth 在 Open 3D Engine 中创建逼真的模拟布料和织物。 '
linktitle: NVIDIA Cloth
title: 使用 NVIDIA Cloth 模拟布料
weight: 300
---

 借助 Open 3D Engine 中的 **NVIDIA Cloth**，您可以为包含 **Actor** 或 **Mesh** 组件的实体创建逼真的布料模拟。**NVIDIA Cloth ** Gem 提供了一个组件，可用于在已使用**FBX Settings**中应用的 **Cloth** 修改器处理的任何网格上模拟布料。

要使用 **NVIDIA Cloth**，您必须启用 [NVIDIA Cloth gem](/docs/user-guide/gems/reference/physics/nvidia/nvidia-cloth/).

## NVIDIA Cloth 功能 

****
+ 将布料数据应用于从 `.fbx` 文件导入的网格。
+ 将布料模拟添加到包含 **Mesh** 和 **Actor** 组件的实体。
+ 布料网格简化和静态三角形移除允许您使用复杂的布料网格并更快地生成布料模拟。
+ 使用您创建的顶点颜色流为每个布料粒子定义 **Inverse mass**, **Motion constraints**, 和 **Backstop**。
+ 在布料模拟和具有运动约束的角色关键帧动画之间混合。
+ 使用 **Animation Editor** 向角色添加布料碰撞器。
+ 将局部风力添加到您的布料模拟或使用力区域来模拟风。
+ 在可用的 CPU 内核上并行模拟布料。
+ **NVIDIA Cloth** Gem 的公共 API 允许其他系统和 Gem 访问布料模拟功能。
+ 使用实时布料调试可视化效果调试布料模拟和约束。

## 使用 NVIDIA Cloth 

[Cloth 组件](/docs/user-guide/components/reference/physx/cloth/) - Cloth 组件参考。

[用于Mesh组件的布料](/docs/user-guide/interactivity/physics/nvidia-cloth/meshes/) - 为包含 **Mesh** 组件的实体创建布料。

[用于Actor组件的布料](/docs/user-guide/interactivity/physics/nvidia-cloth/actors/) - 为包含 **Actor** 组件的实体创建布料。

[布料的逐顶点属性](/docs/user-guide/interactivity/physics/nvidia-cloth/vertex-data/) - 使用逐顶点属性来定义 **Inverse mass**, **Motion constraints**, 和 **Backstop**，以创建更高质量和更可预测的布料模拟。

[布料模拟约束](/docs/user-guide/interactivity/physics/nvidia-cloth/constraints/) - 概述了 **Motion constraints** 和 **Backstop**  如何改进布料模拟的结果。

[Cloth 可视化调试器](/docs/user-guide/interactivity/physics/nvidia-cloth/debugging/) - 为 Cloth 模拟启用可视化调试器。

## NVIDIA Cloth 参考 

[NVIDIA Cloth 文档](https://gameworksdocs.nvidia.com/NvCloth/1.1/index.html) 在 NVIDIA GAMEWORKS 开发者门户。
