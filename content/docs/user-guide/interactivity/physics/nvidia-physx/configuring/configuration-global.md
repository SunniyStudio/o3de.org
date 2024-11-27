---
linkTitle: 全局配置
title: PhysX系统全局配置
description: ' 在 Open 3D Engine （O3DE） 中配置 PhysX 系统的全局设置。 '
weight: 200
toc: true
---

在 **全局配置** 选项卡上，您可以配置全局 PhysX 设置和调试可视化设置。

![PhysX Global Configuration tab](/images/user-guide/interactivity/physics/nvidia-physx/configuring/physx-configuration-1.png)

## System 系统配置 

下表描述了 **系统配置** 设置。

| 属性 | 说明 |
| - | - |
| **Max Time Step** `m_maxTimeStep` | 指定模拟可以处理的最大时间步长。此设置为仿真步骤的长度设置限制，并防止在未设置 `m_fixedTimeStep` 时仿真不稳定。如果帧之间的时间大于`m_maxTimeStep` ，则模拟步骤的时间将限制为此值。该值应为一个小增量。默认值为 '0.05' （1/20 秒）。 |
| **Fixed Time Step** `m_fixedTimeStep` | 设置 PhysX 模拟的频率。默认值为`0.017`  （1/60 秒）。值越低，模拟越准确，但运行时成本越高。较高的值可能会导致结果不太稳定。如果此值设置为 `0`，则模拟使用帧之间的时间，该时间可能会有所不同。如果帧时间大于此值，则 O3DE 会将时间拆分为以下计算得出的步数: <pre>frame_time/m_fixedTimeStep</pre> |
| **Raycast Buffer Size** | 可从光线投射查询返回的最大命中数。 默认值为 `32`. |
| **Shapecast Buffer Size** | 可从 shapecast 查询返回的最大命中数。 默认值为 `32`. |
| **Overlap Query Buffer Size** | 可从重叠查询返回的最大命中数。 默认值为`32`. |

## Scene 场景配置

下表描述了 **场景配置** 的设置。

| 属性 | 说明 |
| - | - |
| **Gravity** |  世界空间重力矢量（以米/平方秒为单位）。默认 **X**、**Y** 和 **Z** 值为`0.0`, `0.0`, 和 `-9.81`. |
| **Continuous Collision Detection (CCD)** |  启用连续碰撞检测 （CCD），这可以以牺牲性能为代价来改善仿真结果。默认情况下处于禁用状态。 |
| **Persistent Contact Manifold** |  如果启用，则在帧之间保留碰撞曲面的数据。默认情况下，此选项处于启用状态，建议保持启用此设置。持久流形存储在一个时间步中创建的触点数据，以便在后续时间步中潜在使用。这需要更多的内存进行模拟，但可以提高碰撞计算的速度和准确性。如果发生碰撞，数据将存储在持续接触流形中，以便在下一个时间步中使用。如果曲面在下一个时间步中不再发生碰撞，则数据将被丢弃。否则，数据将用于加快计算速度、提高准确性并减少抖动和其他不需要的仿真伪影。 |
| **Bounce Threshold Velocity** |  碰撞对象不会反弹的相对速度。默认值为 `2.0`.  |

## Editor 编辑器配置 

以下选项控制 **O3DE 编辑器** 中 PhysX 调试可视化的外观，包括 [PhysX Dynamic Rigid Body](/docs/user-guide/components/reference/physx/rigid-body/)组件 的 **Debug Draw COM**（质心）选项。

{{< note >}}
这些选项是 [PhysX](/docs/user-guide/gems/reference/physics/nvidia/physx/) Gem 的一部分，与 [Debug Draw](/docs/user-guide/gems/reference/debug/debug-draw/) Gem 或[PhysX Debug](/docs/user-guide/gems/reference/physics/nvidia/physx-debug/) Gem 无关。

关节层次结构选项仅适用于 PhysX 关节。它们不适用于角色骨架或模拟对象关节。
{{< /note >}}

| 属性 | 说明 |
| - | - |
| **Debug Draw Center of Mass Size** | 表示质心的调试绘制圆的大小。可能的值是 从 `0.1` 到 `5.0` 米。默认值为 `0.1`.  |
| **Debug Draw Center of Mass Color** | 表示质心的调试绘制圆的颜色。要指定颜色，请在文本框中输入其 RGB 值。该图标显示所选颜色。默认值为 `255`, `0`, `0` (red).  |
| **Global Collision Debug** | 设置全局碰撞调试绘制可见性选项。<br />**Enable all colliders** 显示所有 PhysX 碰撞器，包括设置为隐藏的碰撞器。<br />**Disable all colliders** 隐藏所有 PhysX 碰撞器，包括设置为可见的碰撞器。<br />**Set manually** 您可以在每个碰撞器组件上单独设置 PhysX 碰撞器可见性。这是默认设置。 |
| **Global Collision Debug Color Mode** | 设置调试颜色模式。<br />**Material Color Mode** 使用物理材质的调试颜色。<br />**Error Mode** 对于错误情况（例如三角形过多的网格），显示发光的红色。 |
| **Display Joints Hierarchy** | 启用后，PhysX 关节导联-跟随器连接在视区中显示为一条具有两种颜色的线。一种颜色表示潜在客户，一种颜色表示关注者。默认启用。 |
| **Joints Hierarchy Lead Color** | 引线-从动轮连接线的引线半部分的颜色。 |
| **Joints Hierarchy Follower Color** | 导联-从动轮接头连接线的从动轮半部分的颜色。|
| **Joints Hierarchy Distance Threshold** | 为从动节点绘制线所需的最小距离。短于此阈值的距离仅绘制引线连接的线。 默认值是 `1.0`. |

## Wind 风力配置 

下表描述了 **Wind Configuration** 的设置。有关更多信息，请参考 [创建风力](/docs/learning-guide/tutorials/physx/wind-provider).

| 属性 | 说明                                          |
| --- |---------------------------------------------|
| **Global wind tag** | PhysX 风系统使用此标签来指定提供 *全局* 风力的实体。             |
| **Local wind tag** | PhysX 风系统使用此标签来指定提供风力的实体，这些实体 *局部化* 到碰撞器体积。 |
