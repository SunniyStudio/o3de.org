---
title: ROS 2 Lidar Sensor 组件
linktitle: ROS 2 Lidar Sensor
description: ROS 2 激光雷达传感器组件，用于开放式 3D 引擎 （O3DE） 中的机器人操作系统 （ROS 2）。
---

**ROS 2 Lidar Sensor** 组件封装了激光雷达的仿真，包括数据采集和发布。
它使用抽象来实现易于替换的实现，但在如何获取点方面有所不同。
激光雷达可用于障碍物检测、定位和导航等任务。


## 属性

[ROS 2 Gem](/docs/user-guide/gems/reference/robotics/ros2)

## 依赖

[ROS 2 Frame component](/user-guide/components/reference/ros2/core/ros2-frame)

## 属性

![ROS 2 Lidar Sensor component properties](/images/user-guide/components/reference/robotics/ros2/ros2-lidar-sensor-component.png)

| 属性                          | 说明                                                            | 值           | 默认值             |
|-----------------------------|---------------------------------------------------------------|-------------|-----------------|
| **Sensor Configuration**    | 请参阅 [Sensor Configuration 属性](common/sensor-configuration.md) |             |                 |
| **Lidar Model**             | 这是什么类型的激光雷达。这可以是自定义的，也可以是对应于真实设备的。                            | Enumeration | `Custom3DLidar` |
| **Lidar Implementation**    | 用于光线投射（或其他获取数据的方法）的机制。实现可以由外部 Gem 注册。                         | Enumeration | `SceneQueries`  |
| **Ignore Collision Layers** | 采集数据时要忽略的碰撞层。这对于避免传感器网格本身的阻碍非常有用。                             | List        | empty           |
| **Points At Max**           | 是否为超出最大范围（无穷大值）的值返回点。                                         | Boolean     | false           |

请注意，根据激光雷达的实现，可能存在一些其他属性。
您可以修改自定义激光雷达的激光雷达参数，但不能修改特定激光雷达模型的激光雷达参数。
它们具有更复杂的射击模式，这些模式仅大致反映在这些属性中。

| 属性                       | 说明                                                            | 值       | 默认值           |
|--------------------------|---------------------------------------------------------------|---------|---------------|
| **Name**                 | 激光雷达的名称，反映模型的选择                                               | String  | `CustomLidar` |
| **Layers**               | 激光雷达具有多少个垂直图层。                                                | Integer | 24            |
| **Points per layer**     | 激光雷达返回的每个图层的点数。                                               | Integer | 924           |
| **Layers**               | 激光雷达具有多少个垂直图层。                                                | Integer | 24            |
| **Min Horizontal Angle** | 激光雷达发射模式的最小水平角 （度）。                                           | Float   | -180.0        |
| **Max Horizontal Angle** | 激光雷达发射模式的最大水平角度（度）。                                           | Float   | 180.0         |
| **Min Vertical Angle**   | 激光雷达发射模式的最小垂直角 （度）。                                           | Float   | -35.0         |
| **Max Vertical Angle**   | 激光雷达发射模式的最大垂直角（度）。                                            | Float   | 35.0          |
| **Min range**            | 激光雷达返回点的最小距离。                                                 | Float   | 0.0           |
| **Max range**            | 激光雷达返回实际点的最大距离。请注意，如果设置了 **Points At Max** ，则数据将包含最大范围的无穷大值点。 | Float   | 100.0         |

## 用法

将 **ROS 2 Lidar Sensor** 添加到您的机器人中，以模拟激光雷达数据。选择激光雷达实现和要模拟的模型。

如果您有 NVIDIA 显卡，请考虑[RGL Gem](https://github.com/RobotecAI/o3de-rgl-gem)提供的高效 GPU 实现。
它比基于 Physics scene 查询的默认实现快几个数量级。RGL Gem 还支持多种流行的激光雷达模型。
