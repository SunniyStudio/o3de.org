---
description: ' Open 3D Engine 中 NVIDIA Cloth 的运动约束。 '
title: 布料模拟约束
weight: 400
---

约束限制布料粒子的移动，以防止网格穿透，并从布料模拟中创建更可预测的结果。Open 3D Engine 有两种类型的布料约束：**Motion constraints** 和 **Backstop**。

**Motion constraints** - 将模拟的布料粒子约束在由球体定义的区域内。球体的位置是相对于相应的未模拟顶点位置的。

**Backstop** - 防止模拟的布料粒子进入由球体定义的区域。球体的位置是相对于相应的未模拟顶点位置的。

Motion constraints 和 Backstop 属性是通过在内容创建应用程序（如 Maya）中为网格创建顶点颜色流来设置的。通过 **FBX Settings** 导出网格时，您可以指定哪些顶点颜色流和颜色通道定义 **Cloth ** 修改器中的属性。

运动约束和 Backstop 属性是按顶点分配的，可以与 **Actor** 和 **Mesh** 组件一起使用。

## Motion constraints 

运动约束将模拟布料粒子的移动限制到由球体定义的区域。球体以未模拟网格的相应顶点为中心。球体的半径是使用顶点颜色流中的“运动约束”值以及 **Cloth** 组件的 **Motion constraints** 属性组中的 **Max Distance**、**Scale** 和 **Bias** 属性计算的。

如果不存在顶点颜色流，则每个顶点的 Motion 约束使用默认值 **1.0**。Motion Constraints 顶点颜色流值的范围从 **0.0** 到 **1.0**。如果顶点颜色流的值为 **0.0**，则模拟的布料粒子将完全约束到未模拟的网格顶点。

下图可视化了 Motion 约束。

![Motion constraint diagram for cloth simulation](/images/user-guide/physx/cloth/cloth-motion-constraints-diagram.png)

## Backstop 

由于布料碰撞器是简单的基元，因此您可能会遇到布料碰撞器不足以防止模拟布料粒子穿透其他网格的情况。您可以使用 **Backstop** 来微调模拟布料粒子的行为，以解决这些情况。

Backstop 可防止模拟布料粒子进入由球体定义的区域。球体通过 offset 属性沿相应的未模拟网格顶点的法线定位。Backstop 需要来自顶点颜色流的颜色通道来定义球体的半径和球体的偏移量。顶点颜色流中的 **Backstop Radius** 和 **Backstop Offset** 值由 **Cloth** 组件 **Backstop** 属性中的 **Radius** 和 **Offset** 值进行缩放。

由于顶点颜色通道只能包含介于 **0.0** 和 **1.0** 之间的值，因此颜色通道中表示 **Backstop Offset** 属性的值将被重新映射到 **-1.0** 和 **1.0** 之间的范围。

Backstop Offset 顶点颜色值 **1.0** 由 **Cloth** 组件的 **Back Offset** 属性进行缩放。逆止球体放置在未模拟的网格顶点 后面。

Backstop Offset 顶点颜色值 **0.0** 被重新映射到 **-1.0**，并由 **Cloth** 组件的 **Front Offset** 属性进行缩放。逆止球体放置在未模拟的网格顶点的前面。

Backstop Offset 顶点颜色值 **0.5** 被重新映射到 **0.0**，并通过 **Cloth** 组件的 **Back Offset** 属性进行缩放。逆止球体放置在相应的未模拟顶点上。

Backstop Radius 顶点颜色通道的值范围为 **0.0** 到 **1.0**。Backstop Radius 顶点颜色通道中的值 **0.0** 将禁用相应顶点的 Backstop。

下图可视化了 Backstop。

![Backstop diagram for cloth simulation](/images/user-guide/physx/cloth/cloth-backstop-diagram.png)
