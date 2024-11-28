---
linkTitle: UI Particle Emitter
description: 使用 Open 3D Engine 中的 Particle Emitter （粒子发射器） 组件从 UI 元素发射二维粒子。
title: UI Particle Emitter 组件
weight: 200
---

您可以使用 **Particle Emitter** 组件从元素发射二维粒子。使用 [UI Editor](/docs/user-guide/interactivity/user-interface/editor) 的 **Properties** 窗格为 **Particle Emitter** 组件配置以下设置。


**Emitter 设置**

| 名称 | 说明 |
| --- | --- |
| Emit on activate | 激活组件时开始发射。 |
| Hit particle count on activate | 当发射器开始发射时，Emit and process the average number of particles。 |
| Infinite life time | 使发射器的生命周期无限大。 |
| Emitter life time | 输入发射器处于活动状态的秒数。当达到生命周期的终点时，发射器将停止发射。此选项在未设置 Infinite life time （无限生存期） 时可用。 |
| Emit rate | 输入每秒要发射的粒子数。|
| Emitter shape |  选择以下选项之一：   |
| Particle count limit | 使用 Active particle limit 值来限制活动粒子的数量。 |
| Active particles limit | 当设定了 Particle Count Limit 时，此选项可用。键入活动粒子的最大数量。当达到最大数量时，只有在移除现有粒子后才会发射其他粒子。最大值为 9999。|
| Fixed random seed | 为发射器指定固定的随机种子。如果未选中，则每次发射器开始发射时都会生成一个随机种子。 |
| Random seed | 当 Fixed random seed （固定随机种子） 设置为 时，此选项可用。输入在选择 Fixed random seed （固定随机种子） 时用于粒子发射器的数字种子。该字段最多接受 9 位负整数或正整数。|
| Emit on edge | 当 Emitter shape （发射器形状） 为 Circle （圆形） 或 Quad （四边形） 时，此选项可用。在指定形状的边缘上发射粒子。 |
| Emit inside distance | 当设置了 Emit on Edge 时，此选项可用。输入从中发射粒子的边内的距离。 |
| Emit outside distance | 键入从中发射粒子的边缘外的距离。|
| Initial direction type |  当**Emitter shape** 是 **Circle** 或 **Quad** 时，此选项可用。选择以下选项之一以指定初始方向的计算方式：  |
| Emit angle | 输入发射粒子的垂直度数。 |
| Emit angle variation | 输入一个数字或使用滑块指定发射角度的变化（以度为单位）。有效值为 0 到 180。值 10 指定正负 10 度的变化范围。 |


**Particle 设置**

| 名称 | 说明 |
| --- | --- |
| Infinite life time | 使粒子生命周期无限大。 |
| Life time | 输入发射的粒子最初处于活动状态的秒数。 |
| Life time variation | 输入发射粒子的生命周期可以变化的秒数。 |
| Sprite pathname | 单击省略号 （...） 打开 Pick Texture 对话框，然后选择一个 Sprite 图像文件。 |
| Animated sprite sheet | 当所选 sprite 是 sprite 表（具有多个单元格）时，此选项可用。选择以随时间更改每个粒子的 sprite 表单元格索引。 |
| Loop sprite sheet animation |当设置 Animated sprite sheet 时，此选项可用。选择以循环 Sprite 表单元格动画。 |
| Random sprite sheet index | 当所选 sprite 是 sprite 表（具有多个单元格）时，此选项可用。选择以随机选择初始 Sprite 表单元格索引。 |
| Sprite sheet index | 当所选 sprite 是 sprite 表（具有多个单元格）且未设置 Random sprite sheet index （随机 sprite 表索引） 时，此选项可用。选择用于发射粒子的 sprite 表索引。 |
| Sprite sheet start frame | 当所选 sprite 是 sprite 表（具有多个单元格）并且设置了 Random sprite sheet index （随机 sprite 表索引） 时，此选项可用。为 Sprite 表动画选择 Sprite 表范围的起始帧，或随机选择索引。 |
| Sprite sheet end frame | 当所选 sprite 是 sprite 表（具有多个单元格）并且设置了 Random sprite sheet index （随机 sprite 表索引） 时，此选项可用。设置 Sprite 表动画或随机选择索引的 Sprite 表范围的结束帧。 |
| Sprite sheet frame delay |当设置 Animated sprite sheet 时，此选项可用。键入每个 sprite sheet 帧之间的延迟秒数。 |
| Blend mode |  选择以下选项之一：  |


**Particle Movement**

| 名称 | 说明 |
| --- | --- |
| Relative to emitter | 相对于 Particle Emitter （粒子发射器） 组件附加到的元素移动粒子。如果未选择此选项，则当发射器在画布中移动时，元素会留下粒子轨迹。 |
| Movement co-ordinate type |  选择粒子移动的坐标空间类型：   |
| Speed | 当“移动”坐标类型设置为“笛卡尔”时，此选项可用。输入一个数字，该数字指定在计算发射方向时发射粒子的初始速度。|
| Speed variation | 当“移动”坐标类型设置为“笛卡尔”时，此选项可用。输入一个数字，该数字指定在计算发射方向时发射粒子的初始速度变化。|
| Initial velocity | 当 Movement co-ordinate type 设置为 Polar 时，此选项可用。输入 X 和 Y 值，用于指定发射粒子的初始速度。 |
| Initial velocity variation | 当 Movement co-ordinate type 设置为 Polar 时，此选项可用。输入 X 和 Y 值，这些值指定发射粒子的初始速度的变化。 |
| Acceleration co-ordinate type |  选择用于粒子加速的坐标空间类型：   |
| Acceleration | 输入 X 和 Y 值，用于指定每个发射粒子的加速度。 |
| Orientation velocity based | 将每个粒子的顶部指向当前速度矢量。 |
| Initial orientation velocity based | 将每个粒子的顶部指向初始速度矢量。 |
| Initial rotation | 输入初始从垂直方向顺时针旋转的度数。 |
| Initial rotation variation | 输入初始旋转变化的度数。值 10 指定围绕指定初始旋转正负 10 度的变化范围。 |
| Rotation speed | 输入旋转速度（以每秒顺时针方向的度数为单位）。 |
| Rotation speed variation | 键入转速的变化（以每秒顺时针方向的度数为单位）。值 10 指定转速中正负 10 度的变化范围。 |


**Particle Size**

| 名称 | 说明 |
| --- | --- |
| Lock aspect ratio | 将发射粒子的宽度和高度锁定到当前纵横比中。 |
| Pivot | 输入 X 和 Y 值，指定粒子的轴心，从左上角的 （0,0） 到右下角的 （1,1）。 |
| Size | 输入 X 和 Y 值，用于指定每个发射粒子的大小。 |
| Size variation | 输入 X 和 Y 值，以指定每个发射粒子的大小变化。 |


**Particle Color**

| 名称 | 说明 |
| --- | --- |
| Color | 输入指定每个发射粒子颜色的 RGB 值，或单击白色方块以使用 Select Color 对话框。 |
| Color brightness variation | 输入一个介于 0 和 1 之间的十进制数，该数字指定每个发射粒子的亮度变化。 |
| Color tint variation | 输入一个介于 0 和 1 之间的十进制数，该数字指定每个发射粒子的色调变化。 |
| Alpha | 输入一个介于 0 和 1 之间的十进制数，用于指定用于发射粒子的 Alpha。 |


**Timelines**

| 名称 | 说明 |
| --- | --- |
| Speed multiplier | 单击加号 （+） 可添加关键帧，这些关键帧控制曲线，以在其生命周期内乘以粒子速度。 |
| Width multiplier |  当未设置 **Lock aspect ratio**（在 **Particle Size**中） 时，此选项可用。 单击加号 （**+**） 可添加关键帧，这些关键帧控制曲线在其生命周期内乘以粒子宽度。 |
| Height multiplier |  当未设置 **Lock aspect ratio**（在 **Particle Size**中） 时，此选项可用。 单击加号 （**+**） 可添加关键帧，这些关键帧控制曲线在其生命周期内乘以粒子高度。  |
| Size multiplier | 单击加号 （+） 可添加关键帧，这些关键帧控制曲线在其生命周期内乘以粒子大小。 |
| Color multiplier | 单击加号 （+） 可添加关键帧，这些关键帧控制曲线在其生命周期内乘以粒子颜色。 |
| Alpha multiplier | 单击加号 （+） 可添加关键帧，这些关键帧控制曲线在其生命周期内乘以粒子 Alpha。 |
| Time |  输入一个介于 '`0`' 和 '`1`' 之间的值，该值指定关键帧在粒子生命周期内出现的时间。 值 '`0`' 是粒子生命周期的开始，'`1`' 是粒子生命周期的结束。 |
| Multiplier |  指定一个介于 '`-100`' 到 '`100`' 之间的值，以乘以此时间轴控制的值。 例如，如果速度值为 '`50.0`'，特定关键帧的速度乘数为 '`2.0`'，则指定关键帧的速度值为 '`100`'。 |
| In tangent |  控制当前关键帧的 in tangent。选择以下选项之一：    |
| Out tangent |  控制当前关键帧的 out tangent。选择以下选项之一：    |
| Ease In |  S指定曲线逐渐接近平坦的切线。 例如，指定 **Ease In** 给 **In tangent**，指定 **Ease Out** 给 **Out tangent**看起来像 x3 （x 立方） 曲线在其原点处的展平切线。 |
| Ease Out |  指定曲线从平坦切线逐渐退缩。 例如，指定**Ease In** 给 **In tangent**，指定 **Ease Out** 给 **Out tangent**看起来像 x3 （x 立方） 曲线在其原点处的展平切线。 |
| Linear | 指定曲线从关键帧线性移动到下一个或上一个关键帧。|
| Step | 指定曲线从当前关键帧值跳转到下一个或上一个关键帧值。|
