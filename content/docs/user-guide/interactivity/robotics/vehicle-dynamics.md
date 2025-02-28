---
linkTitle: 车辆动力学
title: ROS 2 车辆动力学
description: 了解 ROS 2 Gem 在 Open 3D Engine （O3DE） 中的车辆动力学的基本概念和结构。
toc: true
weight: 500
---

## Vehicle Model

VehicleModelComponent 用于将目标速度、转向或加速度等输入转换为车辆（机器人）部件上的物理力。**VehicleModel** 有一个 **VehicleConfiguration**，用于定义车轴、参数化和分配车轮。该模型需要每个轮子实体中都存在 **WheelControllerComponent**。它还使用 **DriveModel** 的实现，将车辆输入转换为作用在转向元件和车轮上的力。

### Wheel Controller

车轮控制器是应连接到车辆车轮的控制器。轮子实体应附加 PhysX Hinge Joint （PhysX 铰链关节）。联合控制器应具有：
 - **Motor Configuration / Use Motor** 已启用。
 - **Motor Configuration / Force Limit Value** 设置为 可取的 值。

![PhysX Joint](/images/user-guide/gems/ros2/physx_joint.png)

车轮控制器具有以下参数，如下所示。

![Wheel Controller](/images/user-guide/gems/ros2/wheelController.png)  

| 参数名              | 说明                                                                      |
|------------------------------|----------------------------------------------------------------------------------|
| **Steering Entity**          | 具有 PhysX Hinge Joint （PhysX 铰链关节） 的实体，该关节可更改轮子的方向。 |
| **Scale of steering axis**   | 允许用户更改车轮转向的比率或/和方向。       |

### Ackermann Drive Model

**AckermannDriveModel** 的实现使用 [PID 控制器](https://en.wikipedia.org/wiki/PID_controller)。该模型计算车辆接头中的速度或力，并相应地将其应用于命令速度。

![AckermannModel](/images/user-guide/gems/ros2/ackermanModel.png)

模型的参数通过 **AckermannVehicleModelComponent**公开给用户：

| 参数名              | 说明                                                                      |
|---------------------------------------------------|--------------------------------------------------------------------------|
| **DriveModel / Axles**                            | 车辆的车轴列表。                                          |
| **DriveModel / Axles / Axle Wheels**              | 轴中的轮子列表。                                              |
| **DriveModel / Axles / Is it a steering**         | 如果启用，Ackermann Drive Model 将应用转向角。  |
| **DriveModel / Axles / Is it a drive**            | 如果启用，Ackermann 驱动模型将施加驱动力。      |
| **DriveModel / Axles / Track**                    | 前后轴之间的距离。                               |
| **DriveModel / Axles / Wheelbase**                | 左右轮之间的距离。                              |
| **DriveModel / Steering PID / P**                 | 用于转向伺服的 PID 控制器的比例增益。                |
| **DriveModel / Steering PID / I**                 | 用于转向伺服的 PID 控制器的积分增益。                    |
| **DriveModel / Steering PID / D**                 | 转向伺服的 PID 控制器的导数增益。                   |
| **DriveModel / Steering PID / IMin**              | PID 的集成影响最小。                                       |
| **DriveModel / Steering PID / IMax**              | PID 的集成影响最大。                                       |
| **DriveModel / Steering PID / AntiWindUp**        | 防止 PID 中的积分缠绕。                                     |
| **DriveModel / Steering PID / OutputLimit**       | 将输出钳制到最大值。                                       |
| **DriveModel / Vehicles Limits / Speed limit**    | 可实现的最大线速度（以米/秒为单位）。                   |
| **DriveModel / Vehicles Limits / Steering limit** | 可实现的最大转向角。                                      |

### Skid Steering Drive Model
该模型计算车辆接头中的速度，并将其相应地应用于命令的速度和配置。

![SkidSteeringModel](/images/user-guide/gems/ros2/skidSteeringModel.png)  
模型的参数通过 **AckermannVehicleModelComponent**公开给用户：

| 参数名              | 说明                                                                      |
|--------------------------------------------------------|--------------------------------------------------------------------|
| **DriveModel / Axles**                                 | 车辆的车轴列表。                                      |
| **DriveModel / Axles / Axle Wheels**                   | 轴中的轮子列表。                                     |
| **DriveModel / Axles / Is it a steering**              | 在此模型中，它将被忽略。                                    |
| **DriveModel / Axles / Is it a drive**                 | 如果启用，Skid Steering Drive Model 会将速度应用于轴的车轮。|
| **DriveModel / Axles / Track**                         | 前后轴之间的距离。                           |
| **DriveModel / Axles / Wheelbase**                     | 左右轮之间的距离。                         |
| **DriveModel / Vehicles Limits / Linear speed limit**  | 可实现的最大线速度（以米/秒为单位）。             |
| **DriveModel / Vehicles Limits / Angular speed limit** | 可达到的最大角速度（以弧度/秒为单位）。          |

### 手动控制

**VehicleModel** 将处理名称为“steering”和“accelerate”的输入事件。这意味着您可以将 [InputComponent](/docs/user-guide/components/reference/gameplay/input/) 添加到同一实体，并为输入设备（例如键盘或游戏手柄）定义输入映射以手动控制车辆。

您可以使用 [rqt_robot_steering](https://index.ros.org/p/rqt_robot_steering/) 等工具通过 Twist 消息移动您的机器人。**RobotControl** 适合与 [ROS 2 导航堆栈](https://navigation.ros.org/) 一起使用。

可以使用此组件实现自己的控制机制。
