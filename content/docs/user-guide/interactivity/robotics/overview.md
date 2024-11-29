---
linkTitle: 概念
title: 机器人概念
description: Open 3D Engine （O3DE） 中机器人技术的概念、Gem、模板和演示概述。
weight: 100
---

ROS 2 Gem 有助于使用 [ROS 2 / 机器人操作系统](https://docs.ros.org/en/jazzy/index.html).

ROS 2 Gem 包含许多用于构建机器人模拟的组件，例如传感器、
不同类型驱动器的控制器、机械臂和机器人的动态生成。它还配备了许多实用程序。

## Gems

有几种 Gem 可以通过 **Open 3D Engine （O3DE）** 为机器人模拟提供支持。
- [ROS 2 Gem](/docs/user-guide/gems/reference/robotics/ros2)，最中心也是最重要的一个。它提供了大多数功能，而 Gems 的其他机器人技术则依赖于它。
- 提供示例 ROS 2 预制件（机器人）和场景装饰的资源 Gem，包括`ProteusRobot`, `RosRobotSample`, `WarehouseAssets`, `WarehouseAutomation`, 和 `WarehouseSample`.
- 3rd party Gems: 
  - [Robotec GPU Lidar (RGL) Gem](https://github.com/RobotecAI/o3de-rgl-gem) - 使用 CUDA 在 O3DE 中加速 LIDAR 仿真。
  - [Robotec Vehicle Dynamics Gem](https://github.com/RobotecAI/robotec-vehicle-dynamics-gem) - 简单的车辆控制器。

## 模板

机器人有三个模板：
- [ROS 2 项目模板](https://github.com/o3de/o3de-extras/tree/development/Templates/Ros2ProjectTemplate):
  - 使用 ROSBot XL 机器人（差速驱动（滑移转向）机器人）的简单内部场景。
  - 它是最轻量级和最基本的机器人项目模板。
- [Warehouse 项目模板](https://github.com/o3de/o3de-extras/tree/development/Templates/Ros2FleetRobotTemplate):
  - 带有 Proteus 机器人的照片级逼真仓库。
  - 使用包含的生成组件可以轻松添加更多机器人。
- [Manipulation 项目模板](https://github.com/o3de/o3de-extras/tree/development/Templates/Ros2RoboticManipulationTemplate):
  - 机械臂用例的两个级别：操作研发和码垛化。
  - 适用于机械臂的用例，提供两种夹爪。包括用于操作的项目。

## 演示

有一些开源项目演示了 ROS 2 Gem 可以做什么：
- [Robot Vacuum Sample](https://github.com/o3de/RobotVacuumSample): 在美丽公寓中导航的机器人吸尘器：
- [Robot Harvesting Sample](https://github.com/o3de/ROSConDemo): 通过 ROS 2 编排的农业机器人在风景秀丽的果园中采摘苹果。
- [Automated Fulfillment Center](https://github.com/RobotecAI/ROSCon2023Demo): 机械臂和自主移动机器人负责码垛和内部物流。
