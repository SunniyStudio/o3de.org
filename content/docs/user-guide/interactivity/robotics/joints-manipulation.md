---
linkTitle: 关节操作
title: 关节操作
description: 使用 ROS 2 Gem Open 3D Engine （O3DE） 控制关节系统，例如机械臂。
toc: true
weight: 510
---

## 概述

对于涉及机械臂或其他关节系统（如腿部运动）的应用，操纵关节至关重要。
O3DE 支持通过 [ros2_controllers](https://github.com/ros-controls/ros2_controllers)等软件包使用 ROS 控制关节系统，
以及与 [MoveIt2](https://moveit.ros.org/) 的集成。

### 支持的功能

使用操作组件，您可以：
- 使用 __JointsManipulationComponent__ 自动发布 [JointState](https://docs.ros2.org/latest/api/sensor_msgs/msg/JointState.html)  条 ROS 消息。
- 通过 [FollowJointTrajectory](https://github.com/ros-controls/control_msgs/blob/humble/control_msgs/action/FollowJointTrajectory.action) ROS 动作控制关节系统。
- 基于铰接以及铰链和棱柱关节的控制系统。
- 为开发人员扩展接口，以便通过 __JointsManipulationRequests__ 和 __JointsPositionControllerRequests__ 实现您自己的控制组件。
- 在多机器人场景中使用操作组件。

### 限制

操纵组件仅适用于单自由度关节。

### 组件及其交互

关节的移动是通过一组实现它们的接口和组件来处理的。

| 接口                                   | 组件                                                                             | 规则                            |
|--------------------------------------|--------------------------------------------------------------------------------|-------------------------------|
| __JointsPositionControllerRequests__ | __JointsArticulationControllerComponent__<br/>__JointsPIDControllerComponent__ | 将关节移动到所需位置。                   |
| __JointsManipulationRequests__       | __JointsManipulationComponent__                                                | 保持并发布关节状态信息，将命令中继到控制器。        |
| __JointsTrajectoryRequests__         | __JointsTrajectoryComponent__                                                  | Host 动作服务器用于轨迹命令，通过一系列位置控制轨迹。 |

## 模拟关节系统

### 快速开始使用 Manipulation Template

如果你的目标是模拟机械臂，最快的开始方法是从 Manipulation Template [创建项目](/docs/welcome-guide/create/)。

如果您按照 [项目配置](project-configuration.md)的步骤操作，将设置`O3DE_HOME`和 `PROJECT_PATH`的环境变量，并注册模板。
使用以下命令创建新项目：

```shell
${O3DE_HOME}/scripts/o3de.sh create-project --project-path $PROJECT_PATH --template-name Ros2RoboticManipulationTemplate
```

该模板附带了几个示例，您可以按照其 [README](https://github.com/o3de/o3de-extras/tree/development/Templates/Ros2RoboticManipulationTemplate).

### 配置你的机器人

#### 导入机器人
项目完成后，使用 [机器人导入](importing-robot.md) 功能将您选择的机器人加载到 O3DE 中。
确保在导入器的最后一页上选中复选框 ```Use articulation for joints and rigid bodies```。
强烈建议使用关节来稳定模拟机械臂和其他关节系统。

#### 添加必要的组件
如果导入继续没有问题，请打开新创建的Prefab的根实体并添加三个新组件：
- `JointsArticulationControllerComponent` 来控制机器人的运动。
- `JointsTrajectoryComponent`以侦听 MoveIt 轨迹消息。
- `JointsManipulationEditorComponent` 它发布 ```joint_states``` 并设置初始位置。

#### 在关节中启用转动电机

在所有关节上启用转动电机，并设置 ```motor force limit```, ```stiffness``` 和 ```damping``` 值。启用转动电机的关节将保持其设置位置。

#### 设置初始位置和主题名称

- 在`JointsManipulationEditorComponent`中，为所有关节添加初始位置。
关节名称可以在`ROS2FrameComponent`关节名称内的Prefab实体中找到。初始位置以弧度为单位。 
- 在`JointsTrajectoryComponent` 中，设置用于控制轨迹的主题。
通常，此主题名称以 ```joint_trajectory_controller``` 结尾，但应与 MoveIt 配置文件中的名称相同。

#### 开始模拟

开始模拟并查看机器人如何将自身设置到初始位置。如果出现错误，请检查 O3DE 控制台的日志，其中包括所有关节名称。
现在，机器人已准备好使用 MoveIt 进行控制。

### 使用 MoveIt2 运行

![MoveIt2](/images/user-guide/interactivity/robotics/robotic_arm_moveIt.png)

为您的 ROS 发行版安装 moveIt 软件包：
```shell
sudo apt install ros-${ROS_DISTRO}-moveit ros-${ROS_DISTRO}-moveit-resources
```

之后，准备 MoveIt 启动文件。这可以通过使用机器人提供的配置手动创建文件来实现。
或使用 [MoveIt 设置助手](https://moveit.picknik.ai/main/doc/examples/setup_assistant/setup_assistant_tutorial.html).

您可以在 [Manipulation Template （操作模板）](https://github.com/o3de/o3de-extras/tree/development/Templates/Ros2RoboticManipulationTemplate) 中看到两个示例。

最后，运行启动文件并使用 MoveIt 控制模拟的关节系统。

![MoveIt2](/images/user-guide/interactivity/robotics/rviz2_moveit.png)

## 相关主题

| 主题                   | 说明                         |
|-------------------------|-------------------------------------|
| [抓手](grippers.md) | 在 O3DE 中模拟机器人抓手 |


