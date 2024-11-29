---
linkTitle: SDFormat 传感器
title: SDFormat 传感器
description: Robot Importer 中 SDFormat 传感器支持的详细说明。
weight: 100
---

## 介绍

[SDFormat](http://sdformat.org/), [URDF](http://wiki.ros.org/urdf), 或 [XACRO](http://wiki.ros.org/xacro) 格式描述的机器人可以使用机器人导入器导入到您的 O3DE 模拟项目中。该工具将创建用于对机器人进行建模的 O3DE 实体和组件。您可以在 [文档](/docs/user-guide/interactivity/robotics/importing-robot/)中找到有关 Robot Importer 的更多详细信息。

同样，机器人的 [传感器](http://sdformat.org/spec?ver=1.10&elem=sensor)  使用 [ROS 2 Gem](/docs/user-guide/gems/reference/robotics/ros2/) 中提供的 ROS 2 传感器组件从描述文件导入到 O3DE 中。请注意，传感器的描述可以直接存储在 SDFormat 文件中，也可以使用 URDF 和 XACRO 文件中的`<gazebo>`标签。

## 传感器导入架构

传感器导入，即 Gazebo 描述和 O3DE 组件之间的映射，基于 O3DE [反射系统](/docs/user-guide/programming/components/reflection/reflecting-for-serialization/)。特别是，旨在镜像 SDFormat 传感器和/或插件行为的 O3DE 组件使用专用属性标记进行注册。名为 _hook_ 的导入结构实现了机器人描述参数和 O3DE 数据之间的转换方案。Robot Importer 查找所有活动的 _hooks_ 并检查其中任何一个可用于导入 SDFormat 数据。映射是可扩展的，允许您添加 _hooks_ 并将它们映射到现有的 SDFormat 数据。

需要注意的是，在 [ROS 2 Gem](/docs/user-guide/gems/reference/robotics/ros2/)中实现传感器在设计上公开了 ROS 2 通信接口，这使得它与 Gazebo 传感器插件相当。因此，如果要使 ROS 2 主题可用，则不必定义 Gazebo 传感器插件。另一方面，如果机器人描述文件中存在插件定义，则传感器和插件名称都必须与 _hook 的 s_ 定义匹配，才能添加到机器人的 O3DE 表示中。这样，您可以使用同一传感器的特定实现来覆盖 Robot Importer 的默认行为。

### SDF 传感器标记映射

四个基本传感器的 _hooks_ 在 Robot Importer 中预定义。这些可以总结如下：

| _Hook_ 名称 |支持的 SDFormat 传感器 |支持的 SDFormat 插件 |O3DE 传感器组件       |
| ------------------ | -------------------------- | ----------------------------- | --------------------------- |
| _CameraSensorHook_ | `camera_sensor`            | `libgazebo_ros_camera`        | `ROS2CameraSensorComponent` |
|                    | `depth_camera`             | `libgazebo_ros_depth_camera`  |                             |
|                    | `rgbd_camera`              | `libgazebo_ros_openni_kinect` |                             |
| _GNSSSensorHook_   | `gps`, `navsat`            | `libgazebo_ros_gps_sensor`    | `ROS2GNSSSensor`            |
| _ImuSensorHook_    | `imu`                      | `libgazebo_ros_imu_sensor`    | `ROS2ImuSensorComponent`    |
| _LidarSensorHook_  | `lidar`, `ray`             | `libgazebo_ros_ray_sensor`    | `ROS2LidarSensorComponent`  |
|                    | `gpu_lidar`, `gpu_ray`     | `libgazebo_ros_laser`         | _请参阅以下信息_     |

光线/激光雷达传感器的 GPU 实现不包含在 ROS 2 Gem 中，需要额外的 [O3DE RGL Gem](https://github.com/RobotecAI/o3de-rgl-gem)。GPU 传感器在导入期间进行映射，以便在此 Gem 不可用时在 CPU 上运行。

### 扩展默认映射

您可以通过实现额外的 _hooks_ 并在基于 _SerializeContext_ 反射系统的系统中注册它们来扩展默认映射。通常，这包括三个任务。

首先，你需要声明 `ROS2::SDFormat::SensorImporterHook` 结构，它由以下内容组成：
* 与您的导入方案关联的 SDFormat 传感器集
* 与您的导入方案关联的插件名称集
* 输入机器人描述中支持的参数集（仅用于导入详细）
* 已注册的回调函数，当 _hook Robot Importer 的定义与输入数据匹配时s_该函数

已注册的回调函数会创建许多模拟特定传感器所需的 O3DE 组件。此外，它还会解析机器人描述文件以读取某些处理参数，并列出支持的传感器和插件。在 O3DE 中实现`libgazebo_mygps_sensor.so`的`sdf::SensorType::NAVSAT`的示例实现如下所示：

```cpp
ROS2::SDFormat::SensorImporterHook ROS2SensorHooks::MyGNSSSensor()
{
    ROS2::SDFormat::SensorImporterHook importerHook;
    importerHook.m_sensorTypes = AZStd::unordered_set<sdf::SensorType>{ sdf::SensorType::NAVSAT };
    importerHook.m_supportedSensorParams = AZStd::unordered_set<AZStd::string>{ ">update_rate", ">my_parameter" };
    importerHook.m_pluginNames = AZStd::unordered_set<AZStd::string>{ "libgazebo_mygps_sensor.so" };
    importerHook.m_sdfSensorToComponentCallback = [](AZ::Entity& entity, const sdf::Sensor& sdfSensor) 
        -> ROS2::SDFormat::SensorImporterHook::ConvertSensorOutcome
    {
        if (!sdfSensor.NavSatSensor())
        {
            return AZ::Failure(AZStd::string("Failed to read parsed SDFormat data of %s NavSat sensor", sdfSensor.Name().c_str()));
        }
        const float myUpdateRate = sdfSensor.UpdateRate();
        const float myParameter = sdfSensor.Element()->Get<float>("my_parameter", 1.0f).first;
        if (Utils::CreateComponent<MyO3DEEditorComponent>(entity, myUpdateRate, myParameter))
        {
            return AZ::Success();
        }
        else
        {
            return AZ::Failure(AZStd::string("Failed to create NavSat MyO3DEEditorComponent."));
        }
    };

    return importerHook;
}
```

您的第二个任务是在 O3DE 组件中实现所需的模拟行为，该组件将由 Robot Importer 创建。最后，您需要使用 `SensorImporterHooks` 属性标签通过 _SerializeContext_ 反射系统定义和注册 _hook_。这允许 Robot Importer 查找您的 _hook_ 并将其添加到映射中。可以在任何 O3DE 编辑器组件中完成注册，并且可以一次注册多个 _hooks_。为简单起见，您可能希望将注册直接添加到 O3DE 组件。实现注册方案的示例代码可以如下所示：
```cpp
void MyO3DEEditorComponent::Reflect(AZ::ReflectContext* context)
{
    if (auto serializeContext = azrtti_cast<AZ::SerializeContext*>(context))
    {
        const ROS2::SDFormat::SensorImporterHook& myImporterHook = CreateMyHook();
        const ROS2::SDFormat::SensorImporterHook& anotherImporterHook = CreateAnotherHook();
        serializeContext->Class<MyO3DEEditorComponent, BaseClassOfMyO3DEEditorComponent>()
                ->Attribute(
                    "SensorImporterHooks",
                    ROS2::SDFormat::SensorImporterHooksStorage{ AZStd::move(myImporterHook), AZStd::move(anotherImporterHook) });
    }
    // more reflection code goes here
}
```

<!--- TODO：添加指向包含分步 hook 实现的教程的链接 -->
