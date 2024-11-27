---
description: ' 添加逐顶点属性，以在 Open 3D Engine 中微调 NVIDIA Cloth 模拟。 '
title: 布料的逐顶点属性
weight: 300
---

可以使用内容创建应用程序中的顶点颜色工具为每个布料粒子设置布料属性。在 **Cloth** 修改器的 **FBX Settings** 中，您可以选择哪个顶点颜色流以及流中的哪个颜色通道表示每个属性。您可以为每个属性使用不同的流，也可以通过将属性存储在不同的颜色通道中来将多个属性合并到单个顶点颜色流中。

**Inverse Mass**
**Inverse Mass** 计算 Per Cloth 的 Particle Mass 值。如果未提供顶点颜色流，则默认情况下，所有顶点的 **Inverse Mass** 值将为 **1.0**。**Inverse Mass** 的值范围是 **0.0** 到 **1.0**。
**Inverse Mass** 值 **0.0** 将从布料模拟中排除顶点。具有 **0.0** **Inverse Mass** 值的顶点将是静态的。
每个布料粒子质量的计算公式为`VertexMass = 1.0/InverseMass`。例如，如果颜色通道中的 **反转质量** 值为 **0.3**，则生成的布料粒子质量值为 `3.33`。**Inverse Mass** 值越小，布料粒子质量就越大。

**Motion Constraints**
**Motion constraints** 将模拟布料粒子的移动限制到由球体定义的区域。球体的位置是相对于相应的未模拟顶点位置的。有关 **Motion Constraints**的详细说明，请参阅 [Cloth模拟约束](/docs/user-guide/interactivity/physics/nvidia-cloth/constraints/).
**Motion Constraints** 逐顶点 属性计算球体的半径。**运动约束** 的值范围为 **0.0** 到 **1.0**。
如果 **Motion Constraints** 值为 **0.0**，则 Cloth 粒子将约束到相应的未模拟顶点。

**Backstop**
**Backstop** 可防止模拟的布料粒子进入由球体定义的区域。球体的位置是相对于相应的未模拟顶点位置的。您可以为每个顶点定义两个 **Backstop**属性，即 **Backstop Offset** 和 **Backstop Radius**。有关 **Backstop** 的详细说明，请参阅 [Cloth 模拟约束](/docs/user-guide/interactivity/physics/nvidia-cloth/constraints/).

**Backstop Offset**
**Backstop Offset** 定义 Backstop 球体沿相应未模拟顶点的法线的偏移。**Backstop Offset** 值范围 **0.0** 到 **1.0** 被重新映射到 **-1.0** 到 **1.0** 之间的范围。
**0.0** 的 **Backstop Offset** 值将重新映射到 **-1.0**，并将 Backstop 球体放置在未模拟顶点的前面。
如果 **Backstop Offset** 值为 **1.0**，则 Backstop 球体将放置在未模拟的顶点后面。
**0.5** 的 **Backstop Offset** 值将重新映射到 **0.0**，并将 Backstop 球体放置在未模拟的顶点上。

**Backstop Radius**
**Backstop Radius**计算 Backstop 球体的半径。**Backstop Radius**的值范围为 **0.0** 到 **1.0**。
如果 **Backstop Radius** 值为 **0.0** 将禁用相应顶点的 Backstop。
