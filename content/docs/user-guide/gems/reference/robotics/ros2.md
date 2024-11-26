---
linkTitle: ROS 2
title: ROS 2 Gem
description: ROS 2 Gem 使用 Open 3D Engine （O3DE） 中的机器人操作系统 （ROS） 2 帮助构建机器人模拟。
toc: true
---

<!-- # O3DE ROS 2 Gem -->

**ROS 2 Gem** 使用 [机器人操作系统 （ROS）](https://docs.ros.org/en/rolling/index.html) 在 **Open 3D Engine （O3DE）** 中启用机器人模拟。ROS 2 Gem 具有以下功能：

* 对 ROS 2 生态系统的直接和自然支持：
    * 不使用任何网桥在 ROS 和 O3DE 之间进行通信。模拟节点将像任何其他 ROS 2 节点一样运行。
        * 使您能够直接包含 ROS 2 标头并在 O3DE 中编写 ROS 2 代码。
        * 没有网桥可以提高通信性能。
        * 自定义消息、服务和操作就可以了！
    * 提供一种包含 ROS 2 依赖项的简单方法。
* 传感器:
    * 通过 Sensor Component Base 进行抽象化，该 Base 负责发布传感器数据和频率等常见设置。
    * 具有多种类型的可配置、可扩展传感器，例如激光雷达（3D 和 2D）、摄像头（包括深度通道）、IMU、里程计、GNSS 和接触式传感器。
* 用于自动处理的实用程序:
    * 模拟时间： - 发布`/clock`，支持非实时。
    * 转换帧的计算和发布 (`/tf`, `/tf_static`).
    * 命名空间 - 默认允许在 O3DE 中进行多机器人仿真！
    * 验证主题和命名空间名称。
    * 通过 ROS 2 服务动态生成机器人。
* 机器人控制组件：
    * 提供一种快速易用的控制机器人的方法。
    * 包括对 Twist 和 AckermannDrive 消息接口的支持。
* 操作和夹持器：
    * 支持机器人手臂和其他关节系统。
    * 可配置的组件，易于与 MoveIt2 集成。
    * 手指和真空夹持器。
* 车辆动力学：
    * Ackermann Steering 订阅 [AckermannDrive](http://docs.ros.org/en/api/ackermann_msgs/html/msg/AckermannDrive.html).
    * 差速驱动器订阅 [Twist](http://docs.ros.org/en/noetic/api/geometry_msgs/html/msg/Twist.html).
* 机器人导入器
    * 允许从 URDF、SDFormat 和 XACRO 导入机器人。
    * 支持传感器插件，导入时在 O3DE 中创建传感器组件。

## 相关主题

| 主题                                                                                             | 说明                                   |
|------------------------------------------------------------------------------------------------|--------------------------------------|
| [O3DE 中的机器人技术](/docs/user-guide/interactivity/robotics)                                        | 了解 O3DE 中的机器人技术，包括 Gem、模板和演示，以帮助您入门。 |
| [ROS 2 Gem 概念和结构](/docs/user-guide/interactivity/robotics/concepts-and-components-overview.md) | ROS 2 Gem 的概念和结构概述，包括其组件概述           |
| [ROS 2 Gem API 参考](/docs/api/gems/ros2)                                                        | 为 ROS 2 Gem 的 API 参考生成的文档。           |
