---
linkTitle: 概念和结构
title: ROS 2 概念和结构
description: 了解 Open 3D Engine （O3DE） 中 ROS 2 Gem 的基本概念和结构。
weight: 300
toc: true
---

本文介绍了 Open 3D Engine （O3DE） 中 [ROS 2 Gem](/docs/user-guide/gems/reference/robotics/ros2/)  的底层概念和结构。
您将了解 ROS 2 和 O3DE 如何通信，以及 ROS 2 组件如何相互连接以在机器人模拟中执行各种功能。

## ROS 2 概念

有关 ROS 2 概念的快速介绍，请参阅 [ROS 2 概念文档](https://docs.ros.org/en/humble/Concepts.html).

## 结构和通信

Gem 创建了一个 [ROS 2 节点](https://docs.ros.org/en/humble/Tutorials/Understanding-ROS2-Nodes.html)，它直接成为 ROS 2 生态系统的一部分。因此，您的模拟不会使用任何桥接进行通信，并且会通过 Environment Variables（环境变量）等设置进行配置。它确实是生态系统的一部分。

请注意，模拟节点是通过`ROS2SystemComponent` - 一个单例来处理的。但是，如果您需要多个节点，则可以自由创建和使用自己的节点。

通常，您将创建发布者和订阅，以便使用常见主题与 ROS 2 生态系统进行通信。
这是通过 [rclcpp API](https://docs.ros.org/en/humble/p/rclcpp/generated/classrclcpp_1_1Node.html#classrclcpp_1_1Node) 完成的。例：

```
auto ros2Node = ROS2Interface::Get()->GetNode();
AZStd::string fullTopic = ROS2Names::GetNamespacedName(GetNamespace(), m_MyTopic);
m_myPublisher = ros2Node->create_publisher<sensor_msgs::msg::PointCloud2>(fullTopic.data(), QoS());
```

请注意，QoS 类是一个简单的包装器[`rclcpp::QoS`](https://docs.ros.org/en/humble/p/rclcpp/generated/classrclcpp_1_1QoS.html).

## 组件概述

- __Central Singleton__
  - `ROS2SystemComponent`
- __Core abstractions__
  - `ROS2FrameComponent`
  - `ROS2SensorComponent`
- __Sensors__
  - `ROS2CameraSensorComponent`
  - `ROS2GNSSSensorComponent`
  - `ROS2IMUSensorComponent`
  - `ROS2LidarSensorComponent`
  - `ROS2Lidar2DSensorComponent`
  - `ROS2OdometrySensorComponent`
  - `ROS2ContactSensorComponent`
- __Robot control__
  - `AckermannControlComponent`
  - `RigidBodyTwistControlComponent`
  - `SkidSteeringControlComponent`
- __Spawner__
  - `ROS2SpawnerComponent`
  - `ROS2SpawnPointComponent`
- __Vehicle dynamics__
  - `AckermannVehicleModelComponent`
  - `SkidSteeringModelComponent`
  - `WheelControllerComponent`
- __Robot Import (URDF) system component__
  - `ROS2RobotImporterSystemComponent`
- __Joints and Manipulation__
  - `JointsManipulationComponent`
  - `JointsTrajectoryComponent`
  - `JointsArticulationControllerComponent`
  - `JointsPIDControllerComponent`

### Frames

`ROS2FrameComponent` 表示机器人的一个有趣的物理部分。它处理此部分与其他参考系之间的时空关系。它还封装了命名空间，这有助于区分不同的机器人和机器人的不同部分，例如在一个机器人上有多个相同的传感器的情况下。

所有传感器和机器人控制组件都需要`ROS2FrameComponent`.

### 传感器

传感器从模拟环境获取数据并将其发布到 ROS 2 域。传感器组件源自 `ROS2SensorComponentBase`.

- 每个传感器都有一个配置，包括一个或多个发布者。
- 传感器使用以下两个事件源之一以给定的速率（频率）发布：帧更新或物理场景模拟事件。
- 一些传感器可以可视化。

如果提供的传感器组件不支持您的传感器，您很可能需要创建一个派生自`ROS2SensorComponentBase`. 
在开发新传感器时，查看 ROS2 Gem 中已经提供的传感器是如何实现的非常有用的。
考虑将新传感器添加为单独的 Gem。这种传感器 Gem 的一个很好的例子是[RGL Gem](https://github.com/RobotecAI/o3de-rgl-gem).

### 机器人控制

该 Gem 带有 `ROS2RobotControlComponent`，您可以使用它来移动您的机器人：

- [Twist](https://github.com/ros2/common_interfaces/blob/master/geometry_msgs/msg/Twist.msg) 消息
- [AckermannDrive](https://github.com/ros-drivers/ackermann_msgs/blob/master/msg/AckermannDrive.msg)
 
组件在配置的主题上订阅这些命令消息。默认情况下，该主题是 `cmd_vel` 的，位于 __ROS2Frame__ 指定的命名空间中。

要使用收到的命令消息，请使用`AckermannControlComponent`, `RigidBodyTwistControlComponent`, 或 `SkidSteeringControlComponent`，具体取决于转向类型。
您还可以实现自己的控制组件或使用 Lua 脚本来处理这些命令。
除非使用脚本，否则控制组件应将 ROS 2 命令转换为 `VehicleInputControlBus`.
如果存在 [`VehicleModelComponent`](#vehicle-model)，则这些事件将由[`VehicleModelComponent`](#vehicle-model)处理。
您可以使用 [rqt_robot_steering](https://index.ros.org/p/rqt_robot_steering/) 等工具通过 Twist 消息移动您的机器人。
`RobotControl` 适合与 [ROS 2 导航堆栈](https://navigation.ros.org/).
可以使用此组件实现自己的控制机制。

### 关节和操纵器

为了控制机械臂等机器人关节系统，与 [MoveIt2](https://github.com/ros-planning/moveit2) 进行了一些集成。
支持两种模拟关节系统：
- 接合链接，受益于物理引擎中简化的坐标接合的稳定性。
- 铰链和棱柱关节组件。
当 [导入机器人](importing-robot.md) 带有关节时，您可以决定使用哪些系统。

有三个接口可用于控制关节系统： `JointsPositionControllerRequests`, `JointsManipulationRequests` and `JointsTrajectoryRequest`.
这些接口中的每一个在 ROS 2 Gem 中都有一个或多个实现，并且可以使用这些接口以模块化方式开发自定义行为。

`JointManipulationComponent` 允许您为所有关节设置目标位置。如果您希望使用 trajectory through 控制移动
[FollowJointTrajectory 操作](https://github.com/ros-controls/control_msgs/blob/master/control_msgs/action/FollowJointTrajectory.action), 使用 `JointsTrajectoryComponent`.

#### Joint States

`JointsManipulationComponent` 默认也发布 [关节状态](https://docs.ros2.org/latest/api/sensor_msgs/msg/JointState.html)。

### Vehicle Model

`VehicleModelComponent`用于将目标速度、转向或加速度等输入转换为车辆部件（机器人）上的物理力。 `VehicleModel` 有一个 `VehicleConfiguration`，用于定义车轴、参数化和分配车轮。 该模型需要每个轮子实体中都存在一个 `WheelControllerComponent` 。它还使用了 `DriveModel` 的实现，将车辆输入转换为作用在转向元件和车轮上的力。

### Vehicle Dynamics

见 [Vehicle Dynamics](vehicle-dynamics.md) 章节。

### Spawner

`ROS2SpawnerComponent` 处理在模拟期间生成实体。
在模拟之前，您必须设置为组件的可用可生成项，并通过 **O3DE 编辑器** 在组件的属性中定义命名的生成点。
这可以通过将 `ROS2SpawnPointComponent` 添加到具有 `ROS2SpawnerComponent` 的实体的子实体来完成。
在模拟期间，您可以使用 ROS 2 服务访问可用可生成对象的名称并请求生成。
服务的名称分布包括`/get_available_spawnable_names` 和 `/spawn_entity` 。
_GetWorldProperties.srv_ 和 _SpawnEntity.srv_ 类型用于处理这些功能。
为了请求定义的生成点名称，您可以使用带有`GetWorldProperties.srv` 类型的`/get_spawn_points_names`服务。
有关特定生成点的详细信息，例如姿势，可以使用具有`GetModelState.srv`类型的`/get_spawn_point_info`服务进行访问。
所有使用的服务类型都在 **gazebo_msgs** 包中定义。

- **Spawning**: 要生成，您必须将可生成名称传入`request.name` 中，将实体的位置传入 `request.initial_pose` 中。
  - 示例调用: 
  ```
  ros2 service call /spawn_entity gazebo_msgs/srv/SpawnEntity "{name: 'robot', initial_pose: {position:{ x: 4.0, y: 4.0, z: 0.2 }, orientation: { x: 0.0, y: 0.0, z: 0.0, w: 0.0 } } }"
  ```
- **Spawning in defined spawn point**: 将可生成对象传入 `request.name` 中，将生成点的名称传入 `request.xml` 中。
  - 示例调用:
    ``` 
    ros2 service call /spawn_entity gazebo_msgs/srv/SpawnEntity "{name: 'robot', xml: 'spawn_spot'}"
    ```
- **Spawning using WGS84 coordinates**: 启用 [Georeference 组件](georeference.md) 时，您还可以使用地理位置生成对象。将 `reference_frame` 设置为 _wgs84_ 并提供坐标。
  - 示例调用 生成在 52°14′22″N, 21°02′44″E:
    ```
    ros2 service call /spawn_entity gazebo_msgs/srv/SpawnEntity "{name: 'robot', initial_pose: { position: { x: 52.2406855386909, y: 21.04264386637526, z: 0.0 }, orientation: {  x: 0.0, y: 0.0, z: 0.0, w: 0.0 } }, reference_frame: 'wgs84'}"
    ```
- **Available spawnable names access**: 将可用可生成对象的名称发送到 `response.model_names` 中。
  - 示例调用:
    ```
    ros2 service call /get_available_spawnable_names gazebo_msgs/srv/GetWorldProperties
    ```
- **Defined spawn points' names access**: 将已定义点的名称发送到 `response.model_names`
  - 示例调用:
    ```
    ros2 service call /get_spawn_points_names gazebo_msgs/srv/GetWorldProperties
    ```
- **Detailed spawn point info access**: 将生成点名称传入 `request.model_name` 中，将定义的姿势传入 `response.pose` 中。
  - 示例调用:
    ```
    ros2 service call /get_spawn_point_info gazebo_msgs/srv/GetModelState "{model_name: 'spawn_spot'}"
    ```
