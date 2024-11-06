---
title: ROS 2 Georeference Level 组件
linktitle: ROS 2 Georeference Level 组件
description: ROS 2 Georeference Level组件使您能够在Open 3D Engine (O3DE)中的机器人操作系统 （ROS 2） 中指定模拟的地理位置。
---

**ROS 2 Georeference Level** 组件允许您选择模拟的地理位置。此组件是 level 组件，应添加到 level 实体中。例如，如果您使用的是 [GNSS 传感器组件](../sensors/ros2-gnss-sensor.md)，它补充了其功能。

## 提供方

[ROS 2 Gem](/docs/user-guide/gems/reference/robotics/ros2)

## 依赖

None

## 属性

![ROS 2 Georeference Level Component Properties](/images/user-guide/components/reference/robotics/ros2/ros2-georeference-component.png)

| 属性                    | 说明                                                                                                                      | 值      | 默认值         |
|-----------------------------|----------------------------------------------------------------------------------------------------------------------------------|-------------|-----------------|
| **Altitude**                | ENU 起始点实体在地球的 WGS84 椭球体上方的高程                                                           | meters      |  0              |
| **Latitude**                | WGS84 中的南北地理坐标，其中北为正                                                      | degrees     |  0              |
| **Longitude**               | WGS84 中的东西地理坐标，其中 east 为正                                                          | degrees     |  0              |
| **ENU Origin Transform**    | 已分配地理位置的实体，其本地坐标系遵循 ENU（东-北-上）方向       | Entity      |                 |
 
## 用法

1. 在您的关卡中确定具有已知地理位置的位置。这可能是建筑物的一角或道路的交叉口。
2. 在已知位置创建一个空实体。
3. 旋转实体，使其局部坐标系与地图对齐：
   - X 应该指向东方。
   - Y 应该指向北方。
   - Z 应该指向上方。
4. 在 **ROS 2 Georeference Level 组件** 中，输入实体的地理位置，并设置 **ENU Origin Transform** 以引用上述实体。
