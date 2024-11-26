---
linkTitle: Human Worker
title: Human Worker Gem
description: Human Worker Gem 提供了一组动画人类工人资产，这些资产可用于 Open 3D Engine （O3DE） 中的机器人操作系统 （ROS） 2 的机器人模拟。
toc: true
---

# Human Worker Gem

![O3DE showing a level with HumanWorkerNavigation prefab](/static/images/user-guide/gems/humanworker-demo.png)

此 Gem 提供了一个模拟的人类工作人员，他可以在场景中的航点之间导航，并且对在同一区域内机动的任何机器人都可见。非玩家角色 （NPC） 的导航路径在开始时使用 [重铸框架](../ai/recast/recast-navigation.md).

人类工作人员资产作为 O3DE 预制件交付，包含视觉模型和物理特性以及所需的 O3DE 组件。这包括：
* 具有纹理的人类工人网格
* 人体站立的网格动画
* 人类行走的网格动画
* O3DE 组件，用于在场景中对人类工作人员进行基本导航

您可以直接将多个预制件导入到场景中。最简单的一个`HumanWorkerStatic.prefab`是一个结合了网格和纹理的预制件。您只能将其用作装饰。`HumanWorker.prefab`还被空闲动画扩展。它不会在集合中移动，因为它不包含任何与导航相关的组件。将`HumanWorkerNavigation.prefab`添加到场景中，以实现功能齐全的 NPC。默认情况下，它带有两个导航点的占位符。worker 在这些点之间行走，并在每个点中停止空闲动画。您可以扩展航路点的数量，并使用预制件覆盖修改其位置。有关更多详细信息，请查看 [教程](/content/docs/learning-guide/tutorials/entities-and-prefabs/override-a-prefab.md)。

您可以在 [ROSCon2023Demo](https://github.com/RobotecAI/ROSCon2023Demo)中找到此 Gem 使用的示例。此 Gem 项目有一个 [GitHub 存储库](https://github.com/RobotecAI/o3de-humanworker-gem)。有关更多信息，请参阅 Gem 的 _README_ 文件。

## 许可证

Gem 根据 [Apache 许可证 2.0 版](https://opensource.org/licenses/Apache-2.0) 获得许可。您可以选择使用 [MIT 许可证](https://opensource.org/licenses/MIT)。必须在两个许可证下做出贡献。
