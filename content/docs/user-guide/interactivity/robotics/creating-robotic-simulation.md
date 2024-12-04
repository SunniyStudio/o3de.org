---
linkTitle: 创建机器人模拟
title: 创建机器人模拟
description: 有关如何在 Open 3D Engine （O3DE） 中使用 ROS 2 Gem 创建机器人模拟的分步过程。
weight: 400
toc: true
---

## 如何创建自己的机器人模拟

<!-- ### 教程

>此部分将填写

### 高级步骤

>This section is to be detailed. -->

设置并熟悉 [示例项目](/docs/user-guide/interactivity/robotics/overview/#demos) 后，请考虑以下步骤：
1. [创建新的 O3DE 项目](/docs/welcome-guide/create/)
   1. 最好使用 [项目模板](/docs/user-guide/interactivity/robotics/overview/#templates) 之一，以便机器人快速启动。
2. [为您的项目注册 ROS2 Gem](/docs/user-guide/project-config/register-gems/)  指南。
3. 为您的机器人和环境创建或导入资产。
   1. 您可以使用 O3DE 支持的格式。
   2. 您可以 [从 URDF/XACRO 导入您的机器人](/docs/user-guide/interactivity/robotics/importing-robot).
   3. 导入的模型可能需要进行一些调整才能为仿真做好准备。
   4. 使用 Vehicle Dynamics 控制器移动机器人。
4. 确定需要模拟的传感器。
   1. 此 Gem 中已实现一些传感器。
      1. 它们可能需要专用化 （特定于特定模型的实现）。
      2. 在每种情况下，您可能希望考虑在性能和真实感之间进行权衡。
   2. 如果要实现新传感器，请使用 `ROS2SensorComponent` 作为基类。
5. 开发必要的传感器及其Prefab。
7. 开发场景和模拟场景，放置 Assets 并配置组件。
8. 使用 ROS 2 机器人堆栈运行模拟。您可以使用许多 [ROS 2 软件包](https://index.ros.org/packages/#humble) 和 [ROS 2 生态系统](https://project-awesome.org/fkromer/awesome-ros2) 中的一些项目快速构建一个。

