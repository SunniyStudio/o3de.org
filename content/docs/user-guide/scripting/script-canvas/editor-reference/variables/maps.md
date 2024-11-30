---
title: Map
description: 了解如何在 Open 3D Engine （O3DE） Script Canvas 中使用键值对映射。
weight: 200
---

Script Canvas 中的映射是键值对的容器。在 **Variable Manager** 中，映射用分色矩形图标表示。

![A map variable in the Script Canvas Variable Manager.](/images/user-guide/scripting/script-canvas/variable-map-containers-1.png)

**创建映射变量**

1. 在 **Variable Manager** 中，执行以下操作之一：
    * 选择 **Create Variable**，然后选择 **Map**.

    或

    * 在**Variable Type**搜索框中，输入**Map**, 然后选择**Map**.

    ![Creating a map variable in Script Canvas.](/images/user-guide/scripting/script-canvas/variable-map-containers-2.png)

1. 输入信息以创建映射，然后单击 **Create**.

    ![Enter information to create a key-value pair map.](/images/user-guide/scripting/script-canvas/variable-map-containers-3.png)

    * 对于 **Variable Name**,输入 map 变量的名称。
    * 对于 **Container Type**, 使用 **Map**.
    * 对于 **Key**, 选择键的数据类型。
    * 对于 **Value**, 为 value 选择 Data type （数据类型）。
    * （可选）要将映射固定到 **Variable Manager**中的变量列表**，请选择 **Pin To Variable List**。然后，当选择**Create Variable**时，映射出现在列表中显示为 **Map<_`key_data_type`_,_`value_data_type`_>**. 当您经常重复使用相同的密钥对组合时，这非常有用。

    ![A map variable pinned to the variable list in the Variable Manager.](/images/user-guide/scripting/script-canvas/variable-map-containers-4.png)

### Map引脚图标

您可以使用 **Get** 或 **Set** 节点从映射中获取值或更改映射。地图的数据图钉具有两种颜色的方形图标。一种颜色表示键的数据类型，另一种颜色表示值。

![Dual-color map data pin icons.](/images/user-guide/scripting/script-canvas/variable-map-containers-5.png)

### 映射操作节点、数据引脚类型和链接

映射使用与数组相同的容器操作节点。与数组一样，数据引脚会自动采用它们所连接的映射的数据类型，如下图所示。

![Container operations for maps.](/images/user-guide/scripting/script-canvas/variable-map-containers-6.png)

与数组一样，映射也提供了一个数据输出引脚，您可以使用该引脚在同一映射上链接操作。

![Chained map operations.](/images/user-guide/scripting/script-canvas/variable-map-containers-7.png)
