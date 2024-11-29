---
linkTitle: SDFormat 插件
title: SDFormat 插件
description: Robot Importer 中对 SDFormat 插件支持的详细说明。
weight: 100
---

## 介绍

以[SDFormat](http://sdformat.org/), [URDF](http://wiki.ros.org/urdf), 或 [XACRO](http://wiki.ros.org/xacro) 格式描述的机器人可以使用机器人导入器导入到您的 O3DE 模拟项目中。该工具将创建用于对机器人进行建模的 O3DE 实体和组件。您可以在 [文档](/docs/user-guide/interactivity/robotics/importing-robot/)中找到有关 Robot Importer 的更多详细信息。

SDFormat 标准允许通过在描述中添加 _plugins_ 来扩展导入的机器人的功能。可以使用 `<gazebo>` 标签将相同的描述合并到 URDF 和 XACRO 文件中。从技术上讲，这样的插件是一个动态加载的代码块，可以扩展 _world_、_model_ 或 _sensor_ 描述。目前，仅支持 _models_、_sensors_ 和 _plugins_。请参阅 [SDformat 传感器页面](./sdformat-sensors.md)以了解有关 _sensors_ 和 _plugins_ 的更多信息。

## 插件导入架构

_Plugin_ 导入，即 Gazebo 描述和 O3DE 组件之间的映射，基于 O3DE [反射系统](/docs/user-guide/programming/components/reflection/reflecting-for-serialization/)。特别是，旨在镜像 SDFormat _plugins_ 行为的 O3DE 组件是使用专用属性标记注册的。名为 _hook_ 的导入结构实现了机器人描述参数和 O3DE 数据之间的转换方案。Robot Importer 查找所有活动的 _hooks_ 并检查其中任何一个可用于导入 SDFormat 数据。映射是可扩展的，允许您添加 _hooks_ 并将它们映射到现有的 SDFormat 数据。

_hooks_ 和 robot 描述之间的匹配是根据插件的文件名完成的。这样，您可以使用与特定文件名连接的特定实施来覆盖 Robot Importer 的默认行为。

### 默认模型插件

两个用于扩展 _models_ 的 _hooks_ 在 Robot Importer 中预定义。它们简化了导入，但是，由于 O3DE 和 Gazebo 之间的差异，需要对 O3DE 组件进行一些手动调整才能使机器人可驾驶。对于启用了 _articulations_ 的模型，请考虑更改_Force 限位 Value_、_Stiffness Value_ 和 _Damping Value_ _Motor Configuration_车轮链接。最后，确保每个链路的惯量和质量配置正确。同样，在导入没有_articulation links_的机器人时，_Force_Motor Configuration_轮关节中的限制Value_是一个关键参数。

#### O3DE 滑移转向机器人控制

_ROS2SkidSteeringModel_是一个预定义的 _hook_，用于将 ROS 或 ROS 2 格式的 `libgazebo_ros_skid_steer_drive.so` 和 `libgazebo_ros_diff_drive.so` SDFormat 插件映射到多个 O3DE 组件中。具体而言，它在机器人的基本链接中创建 _ROS2RobotControlComponent_ 和 _SkidSteeringModelComponent_ O3DE 组件，并在车轮中创建 _WheelControllerComponent_ 组件。

#### O3DE Ackermann 机器人控制器

_ROS2AckermannModel_ 是一个预定义的钩子，用于将 `libgazebo_ros_ackermann_drive.so` SDFormat 插件映射到许多 O3DE 组件中。具体而言，它在机器人的基本链接中创建 _ROS2RobotControlComponent_ 和 _SkidSteeringModelComponent_ O3DE 组件，并在车轮中创建 _WheelControllerComponent_ 组件。

您可能需要手动启用转向关节的关节 （或铰接链接） 中的电机。此外，当 SDFormat 描述中的转向关节被定义为 _Universal Joints_ 时，您的导入将失败，这在 O3DE 中目前不受支持。

#### O3DE 联合状态发布者

_ROS2JointStatePublisherModel_是一个预定义的 _hook_，用于将`libgazebo_ros_joint_state_publisher.so` SDFormat 插件映射到 O3DE 中的 _JointsManipulationEditorComponent_。该组件将与 _JointsArticulationControllerComponent_ 或 _JointsPIDControllerComponent_ 一起添加到机器人的基本链接中，具体取决于您是否决定在导入的机器人中使用关节。

请注意，由于 O3DE 中的命名空间策略与 Gazebo 不同，因此无论您是否在 SDFormat 文件中指定了此钩子，此钩子都不会更改发布关节状态的命名空间。同样，您将无法指定要发布的关节的名称;O3DE _JointsManipulationEditorComponent_ 将以任何一种方式发布每个关节的状态。

#### O3DE 关节姿势轨迹

_ROS2JointPoseTrajectoryModel_是一个预定义的 _hook_，用于将 `libgazebo_ros_joint_pose_trajectory.so` SDFormat 插件映射到 O3DE 中的 _JointsTrajectoryComponent_。该组件将与 _JointsManipulationEditorComponent_ 和 _JointsArticulationControllerComponent_ 或 _JointsPIDControllerComponent_ 一起添加到机器人的基本链接中，具体取决于您是否决定在导入的机器人中使用关节。与 Joint State Publisher 钩子类似，此处不会解析命名空间重新映射。

如果您计划在一个 SDFormat 文件中同时导入 Joint State Publisher 和 Joint Pose Trajectory，则可能无法导入 Joint State Publisher，因为插件在文件中列出的顺序。如果发生这种情况，_JointsManipulationEditorComponent_ 仍将被添加，但创建的组件的参数可能与机器人描述中指定的参数不同。有关它的信息将显示在 Robot Importer 摘要窗口中。目前，有两种方法可以解决此问题：
1. 确保在 SDFormat 文件中将`libgazebo_ros_joint_state_publisher.so`插件列在 `libgazebo_ros_joint_pose_trajectory.so` 之前。如果没有，您只需使用复制粘贴交换它们的描述（包括标题和插件参数）即可手动更改它们的顺序。
2. 成功导入 SDFormat 文件（不包括 `libgazebo_ros_joint_state_publisher.so`）后，转到模型的基本链接并手动调整 _JointsManipulationEditorComponent_ 的参数。请注意，由于此 O3DE 组件不仅用于发布关节状态，还用于设置关节起始位置等操作，因此您可能会遇到更多可用的参数，而不仅仅是定义关节状态发布方式的参数。您可能需要调整的参数都显示在 _Joint State Publisher_ 部分下。

成功导入后，您可能需要手动在关节的关节 （或关节链接） 中启用 motor （motor in the joints （或关节链接） ），以便能够调整其轨迹。

### 扩展默认映射

您可以通过实现额外的 _hooks_ 并在基于 _SerializeContext_ 反射系统的系统中注册它们来扩展默认映射。为模型插件添加钩子的方案类似于 [SDformat sensors page](./sdformat-sensors.md) 中详细描述的扩展传感器支持的方案。唯一的区别是，模型的插件需要在`ModelPluginImporterHooks`属性标签下注册`ROS2::SDFormat::ModelPluginImporterHook`结构。

<!--- TODO：添加指向包含分步 hook 实现的教程的链接 -->
