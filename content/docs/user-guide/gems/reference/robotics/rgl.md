---
linkTitle: RGL
title: Robotec GPU Lidar (RGL) Gem
description: Robotec GPU Lidar (RGL) Gem 在 Open 3D Engine （O3DE）中支持为机器人技术提供 GPU 加速的 LiDAR 仿真。
toc: true
---

<!-- # Robotec GPU Lidar (RGL) Gem -->

**Robotec GPU Lidar （RGL） Gem** 是一个与 [ROS&nbsp;2 Gem](./ros2.md)  配合使用的模块，并使用 GPU 加速的 _Lidar Sensor Component_ 实现对其进行扩展。它使用开源的 [Robotec GPU Lidar library](https://github.com/RobotecAI/RobotecGPULidar) 通过基于 GPU 的光线投射和 [CUDA](https://docs.nvidia.com/cuda/) 和 [OptiX](https://raytracing-docs.nvidia.com/optix8/index.html) 来模拟 LiDAR。

Gem 通过支持以下视觉对象来忠实地表示模拟环境：
* Mesh 组件
* 使用 [O3DE Terrain Gem](../environment/terrain) 创建的地形

您可以使用 O3DE Level Editor 完全自定义 LiDAR 的设置。其中包括以下属性：
* 可配置的光线投射模式
* LiDAR 系列
* 从光线投射中排除的实体

您还可以选择 ROS&nbsp;2 Gem 提供的预设之一来创建适合您需求的 LiDAR 模型。

Gem 可从我们的网站 [Github 存储库](https://github.com/RobotecAI/o3de-rgl-gem) 获得。有关更多信息，请参阅 Gem 的 _README_ 文件和/或库的 [README 文件](https://github.com/RobotecAI/RobotecGPULidar)。

## 许可证

[Robotec GPU Lidar (RGL) Gem](https://github.com/RobotecAI/o3de-rgl-gem/blob/development/LICENSE) 和 [Robotec GPU Lidar library](https://github.com/RobotecAI/RobotecGPULidar/blob/develop/LICENSE) 都以 [Apache License, Version 2.0](https://opensource.org/licenses/Apache-2.0)授权。
