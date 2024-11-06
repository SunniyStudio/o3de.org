---
title: ROS 2 System 组件
linktitle: ROS 2 System
description: Open 3D Engine （O3DE） 中机器人操作系统 （ROS 2） 的系统组件。
---

**ROS 2 System**组件使用执行程序创建默认的 ROS 2 节点，并处理单例行为，例如发布模拟时钟和广播转换。

## 提供者

[ROS 2 Gem](/docs/user-guide/gems/reference/robotics/ros2)

## 依赖

ROS 2 系统组件仅依赖于 Physics System Service。

## 属性

系统组件没有属性。

## 用法

ROS 2 系统组件处理模拟的几个类似单例的行为。
您可以利用其 **Node** 方便地创建发布者和订阅者。
在创建或更新 ROS 消息时，您可以使用它从模拟时钟中获取当前的 ROS 时间戳。
它还在内部用于发布静态和动态转换，这些转换是通过 **ROS 2 Frame** 组件计算的。

请注意，通过组件的 API 访问的模拟 ROS 节点是为了方便，如果您愿意，可以创建自己的节点和执行器。

## ROS2RequestsBus 和 ROS2Interface

`ROS2RequestBus` 和 `ROS2Interface` 是一个 API 系统总线和接口，用于内部 ROS 2 Gem 组件和外部 Gem。

| 请求名称           | 说明                                                                                                      | 参数                                                              | 返回值                                                     | 可脚本化 |
|------------------------|------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------|------------------------------------------------------------|-|
| `GetNode`              | 返回已设置并执行的模拟节点。                                                 | None                                                                    | Node: rclcpp::Node                                         | No |
| `GetROSTimestamp`      | 返回基于模拟时钟的 ROS 时间戳。时间戳对于任何带有标头的消息都很有用。         | None                                                                    | Time: ROS 格式的模拟时间                      | No |
| `BroadcastTransform`   | 广播静态或动态转换。此 API 在内部用于处理 ROS 2 帧转换发布。 | T: 转换为广播; IsDynamic: 是否为动态转换 | None                                                       | No |
