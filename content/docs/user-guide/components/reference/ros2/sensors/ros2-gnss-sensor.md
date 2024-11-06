---
title: ROS 2 GNSS Sensor 组件
linktitle: ROS 2 GNSS Sensor
description: Open 3D Engine(O3DE）中机器人操作系统 （ROS 2） 的 ROS 3 GNSS 传感器组件模拟 GNSS （GPS） 接收器并发布相应的消息。
---

**ROS 2 GNSS Sensor** 组件封装了全球导航卫星系统 （GNSS） 接收器的模拟，提供数据，就像它是真实的传感器一样。GNSS 包括全球定位系统 （GPS） 和 Galileo 等系统。GNSS 组件发布的消息包含以 [NavSatFix](https://docs.ros2.org/latest/api/sensor_msgs/msg/NavSatFix.html) 消息格式指定的当前地理位置。

## 提供方

[ROS 2 Gem](/docs/user-guide/gems/reference/robotics/ros2)

## 依赖

[ROS 2 Frame Component](/user-guide/components/reference/ros2/core/ros2-frame)

## 属性

![ROS 2 GNSS Sensor Component Properties](/images/user-guide/components/reference/robotics/ros2/ros2-gnss-sensor-component.png)

| 属性                       | 说明                                                            | 值 | 默认值 |
|--------------------------|---------------------------------------------------------------|---|-----|
| **Sensor Configuration** | 请参阅 [Sensor Configuration 属性](common/sensor-configuration.md) |   |     |

## 用法

1. 利用 **ROS 2 Georeference 组件** 确定您的关卡的地理位置。
2. 将 **ROS 2 GNSS Sensor** 添加到您的机器人中，以模拟从 GNSS 接收器发出的数据。
