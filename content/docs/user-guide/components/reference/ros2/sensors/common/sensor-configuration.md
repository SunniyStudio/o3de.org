---
title: Sensor Configuration
linktitle: Sensor Configuration
description: Open 3D Engine （O3DE） 中的机器人操作系统 （ROS） 2 传感器配置。
---

**传感器配置** 是所有传感器的通用部分，封装了主题以及 [Quality of Service](https://docs.ros.org/en/rolling/Concepts/Intermediate/About-Quality-of-Service-Settings.html) 设置和发布频率。


## 属性

| 属性                     | 说明                               | 值       | 默认值  |
|------------------------|----------------------------------|---------|------|
| **Visualise**          | 是否显示传感器在模拟中工作的可视化效果，例如绘制激光雷达的点云。 | Boolean | true |
| **Publishing Enabled** | 打开或关闭发布传感器数据。                    | Boolean | true |
| **Frequency**          | 发布传感器数据的频率（每秒）。                  | Float   | 10.0 |

**Sensor Configuration** 可以包含多个主题。对于每个主题，列出了以下属性：


| 属性                     | 说明                                               | 值           | 默认值         |
|------------------------|--------------------------------------------------|-------------|-------------|
| **Topic**              | 主题名称。请注意，它不应包含命名空间，因为命名空间由 **ROS 2 Frame** 组件处理。 | String      | 取决于传感器      |
| **Reliability Policy** | 服务质量 （QoS） 可靠性设置。它控制是需要交付已发布的数据（经确认）还是仅尽最大努力发送。  | Enumeration | Best Effort |
| **Durability Policy**  | 服务质量 （QoS） 持久性设置。它控制是否为稍后加入的订阅者保留已发布的数据。         | Enumeration | Volatile    |
| **History**            | 服务质量 （QoS） 历史记录深度，即发件人队列中保留的消息数。                 | Integer     | 5           |
