---
linkTitle: 导入 Turtlebot 4
title: 导入 Turtlebot 4
description: Robot Importer 示例 - 导入 Turtlebot 4
weight: 100
---

## 介绍

[TurtleBot 4](https://clearpathrobotics.com/turtlebot-4/) 是一个用于教育和研究的开源机器人平台。此机器人的 URDF 描述以 ROS 2 包的形式提供，这使得它易于安装和使用 Robot Importer 进行测试。您应该参考 [介绍](./_index.md)到 Robot Importer 工具，以获得导入步骤的更详细说明。

### 先决条件

从 ROS 2 安装必要的软件包：
```bash 
sudo apt install ros-${ROS_DISTRO}-turtlebot4-description ros-${ROS_DISTRO}-turtlebot4-msgs ros-${ROS_DISTRO}-turtlebot4-navigation ros-${ROS_DISTRO}-turtlebot4-node
```

## 导入机器人

### 机器人导入器向导

打开 Robot Importer 工具并选择输入文件。您应该在 ROS 2 安装文件夹中找到它。默认情况下，该文件位于路径 `/opt/ros/${ROS_DISTRO}/share/turtlebot4_description/urdf/standard/turtlebot4.urdf.xacro` 中。接下来，关闭前两个切换开关，如下图所示。

![Turtlebot tutorial](/images/user-guide/gems/ros2/URDF_importer_turtlebot0.png)

`Use Articulations`切换确定是否应将 [PhysX 关节组件]https://nvidia-omniverse.github.io/PhysX/physx/5.1.0/docs/Articulations.html) 用于关节和刚体。尽管与添加关节相比，铰接通常优于模拟机构，但它们更难设置（例如，它们对链接惯性配置非常敏感）。使用关节可能会让您跳过一些问题。第二个开关`Preserve URDF fixed joints`将允许导入器降低模型的复杂性。此方法的可用性取决于 URDF 实现，您可能希望尝试在设置和未设置此选项的情况下导入机器人，以检查是否获得更好的结果。

## 导入后修改

根据机器人描述，导入器可能会创建彼此相交的碰撞器，从而导致在启动模拟时机器人发生 _explosion_。Turtlebot 4 的`base_link` 和 `wheels`之间存在碰撞。为了缓解此问题，您可以删除 `base_link` 碰撞器或将碰撞器放在不同的碰撞层上。最后但同样重要的是，您可以重新设计碰撞器，使它们不相交。我们建议在`base_link`中禁用碰撞器作为快速修复，并相应地设置碰撞层作为最后的润色。

### 机器人控制

Turtlebot 4 由`libgazebo_ros2_control`插件控制，机器人导入器不支持该插件。相反，您可以手动将  [Skid Steering](https://www.docs.o3de.org/docs/user-guide/interactivity/robotics/vehicle-dynamics/) 添加到机器人以使其驱动。为此，请在 `base_link`中添加三个组件：
- `Skid Steering Twist Control`
- `ROS2 Robot Control`
- `Skid Steering Vehicle Model`

最后一个组件 `Skid Steering Vehicle Model` 至关重要，因为它包含车辆动力学的参数并链接到车轮实体。首先，设置车辆限制、轨道和轴距。接下来，添加一个车轴，并链接到 `left_wheel` 和 `right_wheel` 实体。切换切换开关以将此车桥标记为驱动车桥。最后，设置车轮半径。ROS 2 主题配置可以在 `ROS2 Robot Control` 组件中更改。示例配置如下所示：

![Turtlebot tutorial](/images/user-guide/gems/ros2/URDF_importer_turtlebot1.png)

最后，您需要将 `Wheel Controller` 组件添加到 `left_wheel` 和 `right_wheel` 实体，并在两个车轮实体的铰链关节中切换 `Use Motor` 标志。您可以将`Force Limit Value`  更改为更高的值，以确保平稳驾驶。`right_wheel`的配置如下所示。

![Turtlebot tutorial](/images/user-guide/gems/ros2/URDF_importer_turtlebot2.png)

### 传感器

机器人导入器正确导入了连接到 Turtlebot 4 机器人的多个 [传感器](./sdformat-sensors.md) ，但并非所有传感器都被正确解析，也不是所有传感器都按设计添加 - 根据您的需要，需要进行一些手动调整：
- _LiDAR_ 传感器连接到`rplidar_link`，如机器人描述中所定义
- 由于`libgazebo_ros_create_imu`插件不受支持， `imu_link`中缺少 _IMU_ 传感器
- _RGBD_ 相机传感器已添加到`oakd_rgb_camera_frame`链接中，但应调整传感器的方向
- _ir_ 传感器作为 LiDAR 传感器导入
- Robot Importer 将完全跳过这些_contact sensors_，因为不支持

激光雷达传感器可能无法正常工作的原因与机器人本身相同，即由于自碰撞。要解决此问题，您可以删除或禁用一些导致这些自碰撞的碰撞器。然而，更理想的解决方案是分配不同的碰撞层，以区分应该和不应该与 Lidar 传感器交互的物体，从而防止不必要的干扰。
