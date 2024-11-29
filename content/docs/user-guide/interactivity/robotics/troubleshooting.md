---
linkTitle: 故障排除
title: 故障排除
description: 排查机器人操作系统 （ROS） 和 Open 3D Engine （O3DE） 中的 ROS 2 Gem 的常见问题。
weight: 600
---

在本节中，您将学习有用的提示，以及当 Open 3D Engine （O3DE） 中的机器人仿真未按预期工作时应检查的内容。
您还将找到一些最常见问题的解决方案。

如果你没有找到你的问题，请尝试在[`o3de-extras`](https://github.com/o3de/o3de-extras)仓库中搜索问题和讨论，或在[O3DE Discord](https://{{< links/o3de-discord >}})的`#sig-simulation` 频道中提问。

## 这是 ROS 2 的问题吗？

### 查看错误消息和日志

- 检查控制台输出是否有错误。
- 查看 Editor 日志。在 Project 文件夹中，选中`user/log/Editor.log`.

### 检查你的 ROS 配置

#### 正确安装
您的 ROS 2 已安装吗？来源正确吗？当您的项目构建时也是如此吗？检查 `ROS_DISTRO` 和 `AMENT_PREFIX_PATH`。
  - `echo $ROS_DISTRO` 应显示非空值，例如 `humble`.
  - `echo $AMENT_PREFIX_PATH` 应包括您的 ROS 2 发行版安装路径以及您已获得的任何其他工作区（如果有）。
  - 如果您在项目中使用 ROS 服务，请确保两端的`RMW_IMPLEMENTATION`环境变量相同（签入每个端点）。

#### 节点和主题可见性

如果您的模拟正在运行，您应该会看到 ROS 节点和主题。
  - 运行 `ros2 node list`。您至少应该看到 `/o3de_ros2_node`。
  - 运行 `ros2 topic list` 列出主题。你应该始终能看到 `/parameters` 和 `/rosout`。
    - 如果您的模拟正在运行，您应该会看到其他主题，例如`/clock` 和 `/tf`。

#### 消息流量

- ROS 2 主题是否有流量？当您的模拟正在运行时，应发布消息。
  - 检查 `ros2 topic hz` 或 `ros2 topic echo`。如果您没有看到流量，则可能是由防火墙、禁用多播或 Docker 问题（如果从 Docker 运行）引起的。
  - 请参考 [故障排除指南](#ros2-2-troubleshooting-guide)。
  
#### ROS 2 故障排除指南

有关其他解决方案，请参阅 ROS 2 的 [安装故障排除](https://docs.ros.org/en/rolling/How-To-Guides/Installation-Troubleshooting.html) 页面。

## 这是 Gem 还是 O3DE 问题？

如果您的调试确认问题与 ROS 2 Gem 或 O3DE 有关，请通过在 [`o3de-extras`](https://github.com/o3de/o3de-extras/issues) 或 [`o3de`](https://github.com/o3de/o3de/issues)存储库中提出问题来帮助社区。首先，请检查报告的问题列表以避免重复。

更好的是，帮助我们修复它，让机器人的开源仿真更好地为每个人服务。
按照 [贡献指南](/docs/contributing/) 了解如何提交修复和改进。
