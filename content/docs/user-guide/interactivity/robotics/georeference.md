---
linkTitle: Georeference  
title: Georeference
description: 对 O3DE 级别应用地理配准
toc: true
weight: 520
---

## 概述

机器人模拟通常需要处理地理位置、
为了模拟使用全球定位设备和地理参考 API 的机器人的行为和数据流。
Georeference Component 将允许您处理此类数据。


### 如何设置模拟地理位置

首先，添加一个**Georeference Editor Level Component**到你的关卡实体。

下一步，您需要通过将实体放置在所需位置来确定虚拟环境中的哪个位置将固定到真实世界的位置。
此类地点的良好候选项包括特征位置，例如交叉路口。你需要：
- 了解此实体的真实 [WGS-84](https://en.wikipedia.org/wiki/World_Geodetic_System#WGS84)  位置。
- 按如下方式设置模拟实体的方向 （ENU）：
  - X 应指向东方向
  - Y 应指向北方
  - Z 应指向 Up

现在，剩下的工作就是在 **Georeference Editor Level Component** 的配置中将此实体与 WGS-84 坐标一起设置。


## 用法

正确设置 Georeference Editor Level Component 后，您可以使用：
 - ROS 2 GNSS 传感器，用于使用标准消息模拟和发布 ROS 2 地理位置数据。
 - 公共 API，用于将地理位置转换为水平坐标系，并使用`GeoreferenceRequestsBus`进行_vice versa_。

## 例

以下是构建地理配准并将其应用于包含纹理的简单关卡的详细过程。在此特定示例中，纹理是市中心的正射影像地图。
此纹理已导入 O3DE 并作为纹理应用于平面基元网格。
根据下载的地图的比例和分辨率对下一个平面进行缩放。

接下来，在知道位置（例如市政厅）时，放置了具有 WGS-84 坐标的 ENU 实体：
```
50.06175556 North 
19.93727500 East 
```
这些坐标需要添加到 Georeference Editor Level 组件中： \
![GeoreferenceComponent](/images/user-guide/interactivity/robotics/georeference_component.png)

{{< note >}}
Georeference Editor Level Component 只能添加到级别实体。
{{< /note >}}


使用的 ortophotomap 具有以下地理方向：
```
North - up
East - left
```
ENU 实体需要相应地旋转以遵循底层 ortophoto 地图的方向。
ENU 实体的正确位置如图所示： \
![enu_location](/images/user-guide/interactivity/robotics/enu_location.png)

{{< note >}}
ENU 实体不需要任何其他组件。
{{< /note >}}

您可以通过放置一些来自 ROS 2 Gem 的模拟 GNSS 传感器并将其移动到场景中来测试一切是否按预期工作。
在下图所示的示例中，左侧 O3DE 编辑器中标有彩色球体的位置有三个类似的 GNSS 传感器。
在右侧，您可以看到在 [Foxglove studio](https://foxglove.dev). 

![simulated_gnss](/images/user-guide/interactivity/robotics/simulated_gnss.png)

