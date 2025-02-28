---
title: ROS 2 Frame 组件
linktitle: ROS 2 Frame
description: 用于 Open 3D Engine （O3DE） 中机器人操作系统 （ROS 2） 的 ROS 3 Frame组件。
---
**ROS 2 Frame** 组件反映了坐标的 ROS 参考框架的概念，它遵循[REP103 标准](https://www.ros.org/reps/rep-0103.html)。
它通常用于任何机器人系统，例如传感器通常在自己的参考系中发布，而定位是关于找到从机器人本地坐标系到更通用参考系的转换。
**ROS 2 Frame**组件还处理对多机器人仿真至关重要的命名空间。

## 提供者

[ROS 2 Gem](/docs/user-guide/gems/reference/robotics/ros2)

## 依赖

**ROS 2 Frame**组件依赖于 Transform 服务，它由 **Transform** 组件提供。

## 属性

![ROS 2 Frame component properties - default namespace](/images/user-guide/components/reference/robotics/ros2/ros2-frame-component-namespace-default.png)  
![ROS 2 Frame component properties - custom namespace](/images/user-guide/components/reference/robotics/ros2/ros2-frame-component-namespace-custom.png)  
2
| 属性                    | 说明                                                                                                                            | 值      | 默认值                                                     |
|-----------------------------|----------------------------------------------------------------------------------------------------------------------------------------|-------------|-------------------------------------------------------------|
| **Namespace Configuration** | 确定如何设置组件的命名空间，该命名空间可以是空的、自定义的或从实体名称派生的。                       | Enumeration | 默认值 (为顶级关卡实体的名称，否则为空) |
| **Frame Name**              | 帧的名称，用作已发布消息和广播转换的 `frame_id` 字段。                               | String      | `sensor_frame`                                              |
| **Joint Name**              | 该实体的关节名称，这是关节控制 API 所需的补充信息。                                | String      | empty                                                       |
| **Publish Transform**       | 确定到此帧的父级的转换是否包含在广播的转换中。                                | Boolean     | true                                                        |
| **Effective namespace**     | 只读值提供帧的有效命名空间。它会自动更新并考虑其他帧。 | String      | empty                                                       |

## 用法

**ROS 2 Frame** 组件处理与实体关联的命名空间、框架 ID 和关节名称，该实体是机器人的一部分。
传感器和控制器等许多其他组件都依赖于它。**ROS 2 Frame** 在内部与这些组件一起工作，以确保主题的命名空间、在每条消息中发送适当的`frame_id`以及广播到`/tf` 和 `/tf_static`主题的转换。

如果实体包含 JointComponent（不是固定关节）或 ArticulatedComponent，则转换会持续更新并发布到`/tf`。否则，它将在 `/tf_static` 主题上发布一次，即使实体移动也不会更新。
