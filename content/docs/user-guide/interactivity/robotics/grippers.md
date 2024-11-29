---
linkTitle: 抓手
title: 抓手
description: 使用 ROS 2 Gem Open 3D Engine （O3DE） 模拟机器人抓手。
toc: true
weight: 520
---

## 概述

夹持器是机器人操作中使用最广泛的效应器之一。
模拟机器人夹持器由 ROS 2 动作服务器控制，允许用户跟踪 gipping 操作的当前状态。
通过动作服务器控制夹持器的 API 在 [control_msgs](https://github.com/ros-controls/control_msgs/blob/master/control_msgs/action/GripperCommand.action).

### 支持的功能

有两种夹具可供选择：
 - 真空抓手
 - 手指夹持器

#### 真空抓手
真空抓手模拟经过简化，相当于向纵对象添加一个固定关节，该对象需要具有“Grippable”标签。
真正的真空抓手的有效性范围有限，因为需要通过精确定位来形成对产生真空至关重要的密封。
为了模拟此类行为，Vacuum Gripper 组件具有以下参数：
 - Effector Trigger Collider （效果器触发器碰撞器）
 - Effector Articulation Link（执行器关节链接）

效应器触发器碰撞器是具有 PhysX 碰撞器组件的实体。 \
PhysX collider （PhysX 碰撞器） 组件必须设置为 **trigger**。要了解有关触发器的更多信息，请参阅 [PhysX Collider （PhysX 碰撞器） 组件文档](/docs/user-guide/components/reference/physx/collider/). \
PhysX collider （PhysX 碰撞器） 组件需要调整到模拟抓手的预期范围。\
触发器碰撞器的示例配置如下所示：

![Gripper Screenshot](/images/user-guide/interactivity/robotics/gripper_screen.svg)\
Effector Articulation Link （效应器关节链接） 是具有 PhysX Articulation Link （PhysX 关节链接） 的实体。
此实体将是与纵对象形成关节的父实体。

您需要在 [标签组件](/docs/user-guide/components/reference/gameplay/tag/) 中为实体添加“可抓取”标签，以便在模拟中抓取该实体。这可以防止抓手附着在地面和其他意外物体上：\
![Gripper tag](/images/user-guide/interactivity/robotics/tag.png)

总而言之，以下是 ROS2 操作模板中使用的真空抓手的示例配置：\
![Gripper Config](/images/user-guide/interactivity/robotics/vacuumGripperConfig.png)


#### 手指夹持器

手指夹持器广泛用于机器人技术，由至少两个数字组成，这两个数字可以彼此靠近以捕获物体。ROS 2 Gem 中的手指抓手是通过碰撞和接触来模拟的。
它是一个组件，用于控制附加到同一实体树的 PhysX Articulation Link 顶部的转动电机。

Finger Gripper 组件具有以下参数，这些参数是微调夹持器行为所必需的：
- 速度 epsilon，它确定夹持器仍可被视为静止的最大速度。
- 目标容差，即夹持器与目标位置之间的最大距离，仍算作达到目标。
- Stall time（失速时间），用于确定夹持器的手指是否移动的参数。这是考虑夹持器静止需要的时间。换句话说，较短的停顿时间可能会导致过早报告成功。


#### 夹爪动作服务器
Gripper Action Server 组件与 Vacuum 抓手或 Finger 抓手通信，并将其 API 作为动作服务器公开给 ROS。它允许您配置 action server 名称。

### 局限性

- 目前，这两个夹持器都仅适用于 PhysX 关节。
- 它们可以与刚体和关节交互，但只能附加到关节链接。
- 真空抓手很简单，只能抓取带有“Grippable”标签的物体。

## Running with MoveIt2

您可以将 [MoveIt2](https://github.com/ros-planning/moveit2) 框架配置为具有一个可以规划夹持器移动的移动组。
这样的移动组是在 [Ros2RoboticManipulationTemplate](https://github.com/o3de/o3de-extras/blob/development/Templates/Ros2RoboticManipulationTemplate/README.md)提供的 [ROS 2 launch](https://github.com/o3de/o3de-extras/blob/development/Templates/Ros2RoboticManipulationTemplate/Template/Examples/panda_moveit_config_demo.launch.py) 文件中实现的。
在提供的示例中，将 Rviz2 的“MotionPlanning”插件中的“规划组”更改为“hand”。
您现在可以使用“Joints”标签移动机械手的关节。

![Panda Gripper](/images/user-guide/interactivity/robotics/panda_gripper.png)

请参阅 [关节操纵](joints-manipulation.md)了解有关使用 MoveIt2 进行集成和控制的更多信息。
