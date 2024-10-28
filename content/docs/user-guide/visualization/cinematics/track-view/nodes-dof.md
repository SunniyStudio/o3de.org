---
description: ' 在 Open 3D Engine的 <guilabel>Track View</guilabel> 编辑器中使用Depth of Field节点，通过限制对焦区域来增加场景的真实感。 '
title: 添加 Depth of Field 节点
---

您可以使用**Depth of Field** (DOF)节点来增加场景的真实感，模拟真实世界中摄像机的工作方式。您可以使用宽景深对整个场景进行对焦，也可以使用浅景深只对距离摄像机一定距离的物体进行锐利对焦。

**在轨迹视图中添加景深节点**

1. 在轨迹视图中，视情况右键单击序列（顶部节点）或树中的**Director**节点，然后选择**Add Depth of Field Node**。

1. 对于下面列出的每个键，单击 **DepthOfField** 节点下列出的适用键。

1. 双击该键，将其定位在时间轴中突出显示的行上。

1.  双击绿色标记，并在**Key Properties**下输入**Value**。


**Depth Of Field节点关键属性**

| 属性 | 说明 |
| --- | --- |
| Enable | 启用或禁用景深效果。 |
| FocusDistance | 焦点与摄像机的距离。正值在摄像机前方，负值在摄像机后方。 |
| FocusRange | 与摄像机之间的距离，直至达到最大模糊程度。默认情况下，该值是 FocusDistance 值的两倍。 |
| BlurAmount | 最大模糊度值。 |
