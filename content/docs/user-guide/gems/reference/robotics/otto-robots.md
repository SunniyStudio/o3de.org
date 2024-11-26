---
linkTitle: OTTO Robots
title: OTTO Autonomous Mobile Robots (AMRs) Gem
description: OTTO Robots Gem 提供一组自主移动机器人资产，这些资产可用于 Open 3D Engine （O3DE） 中的机器人操作系统 （ROS） 2 的机器人模拟。
toc: true
---

<!-- # OTTO Robots Gem -->

![O3DE showing a level with OTTO 1500 and OTTO 600 robots](/images/user-guide/gems/otto-robots-gem-demo.png)

**OTTO Robots Gem** 提供以下自主移动机器人 （AMR） 的简化模型，这些机器人由 [OTTO Motors by Rockwell Automation 开发](https://ottomotors.com):
- [OTTO 1500 v2](https://ottomotors.com/1500)
- [OTTO 600](https://ottomotors.com/600)

OTTO 1500 v2 机器人提供基本和高升降平台，而 OTTO 600 仅提供低升降平台。此外，它还包含展台的资产：
- OTTO 1500 v2 的高架和低架
- OTTO 600 支架

升降平台可以使用 PhysX 调试 Gem 手动转向，也可以使用 [ROS&nbsp;2 Gem](./ros2.md)的 Pid 电机控制器组件通过代码进行控制。同样，可以通过 ROS 2 接口修改 OTTO 600 灯，以直观地了解状态。您可以使用 [ROS&nbsp;2 Gem](./ros2.md) 中提供的前向和后向 3D 感知摄像头和 IMU 传感器轻松扩展模型，以制作功能齐全的 AMR。

此 Gem 用于 [ROSCon2023Demo](https://github.com/RobotecAI/ROSCon2023Demo)。您可以从 [GitHub 存储库](https://github.com/RobotecAI/o3de-otto-robots-gem/)下载它。有关更多信息，请参阅 Gem 的 _README_ 文件。

## 许可证

Gem 根据 [Apache 许可证 2.0 版](https://opensource.org/licenses/Apache-2.0) 获得许可。您可以选择使用 [MIT 许可证](https://opensource.org/licenses/MIT)。必须在两个许可证下做出贡献。

模型是根据 [OTTO Motors by Rockwell Automation](https://ottomotors.com) 友情共享的 STL 文件创建的。
