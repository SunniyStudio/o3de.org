---
title: 数组
description: 了解如何在 Open 3D Engine （O3DE） Script Canvas 中使用数组变量类型。
weight: 100
---

数组提供了一个动态的连续内存区域，可以保存给定类型的存储。

**创建数组变量**

1. 在 **Variable Manager** 中，执行以下操作之一：
    * 选择 **Create Variable**, 然后选择 **Array**.

    或

    * 在 **Variable Type**搜索框中，输入**Array**, 然后选择 **Array**.

    ![Creating an array variable in Script Canvas.](/images/user-guide/scripting/script-canvas/variable-array-containers-1.png)

1. 输入信息以创建数组，然后选择 **Create**.

    ![Enter information to create your array.](/images/user-guide/scripting/script-canvas/variable-array-containers-2.png)

    * 对于 **Variable Name**, 输入 Array 变量的名称。
    * 对于 **Container Type**, 使用 **Array**.
    * 对于 **Type**, 选择数组的数据类型。
    * （可选）要将数组固定到 **Variable Manager**中的变量列表**，请选择 **Pin To Variable List**。 然后，当您选择 **Create Variable**时，该数组在列表中显示为**Array<_`data_type`_>**. 当您经常重复使用同一类型时，这很有用。

    ![Array variable pinned to the variable list in Variable Manager.](/images/user-guide/scripting/script-canvas/variable-array-containers-3.png)

### Array pin icons

某些节点（如 **OnEntitiesSpawned** 或 **Get String Array**）以数组形式提供数据。此类节点上数组的数据引脚有一个方形图标。图标的颜色显示数组的数据类型。

![Array data pins color-coded by data type.](/images/user-guide/scripting/script-canvas/variable-array-containers-4.png)

### 数组操作节点

在 **Node Palette** 中，**Containers** 部分包含可用于在数组中添加、获取和删除元素的节点。

![Container node operations.](/images/user-guide/scripting/script-canvas/variable-array-containers-5.png)

* **Add Element at End** - 在容器的末尾添加元素。
* **Clear All Elements** - 从容器中删除所有元素。
* **Erase** - 擦除指定索引或键处的元素。
* **For Each** - 循环访问容器的每个元素。
* **Get Element** - 返回位于指定索引或键处的元素。
* **Get First Element** - 返回容器中的第一个元素。
* **Get Last Element** - 返回容器中的最后一个元素。
* **Get Size** - 返回容器中的元素数。
* **Insert** - 将元素插入到容器中的指定索引或键处。
* **Is Empty** - 返回容器是否为空。

所有容器操作节点都有一个 **In** 引脚和一个 **Source** 引脚。

所有引脚都有一个 **Out** 引脚，但 **For Each** 除外，它有一个 **Each** 引脚，在容器中的每个元素之后发出信号。**For Each** 节点还有一个 **Break** 引脚和一个 **Finished** 引脚。**Break** 引脚在发出信号时停止迭代。当容器中的所有元素都已迭代或 **Break** 引脚停止迭代时，将发出 **Finished** 引脚的信号。

![For Each node connection pins.](/images/user-guide/scripting/script-canvas/variable-array-containers-6.png)

#### 自动数据引脚类型

容器操作上的 pin 是上下文相关的。数据输入和输出引脚自动采用它们所连接的引脚的数据类型。

**示例**

在以下示例中，数组类型为 `string`.

![Connecting a Get container node to a container operation node.](/images/user-guide/scripting/script-canvas/variable-array-containers-7.png)

当您将 **Array\<String\>** 引脚连接到 **Add Element at End** 节点上的 **Source** 引脚时，以下更改会自动发生：

* **Source** 引脚 更改为`string` 类型。
* **Add Element at End** 节点框将展开以包含一个 **String** 文本框，您可以在其中输入字符串值。
* 图钉图标和线条颜色将更改为您正在使用的数据类型的颜色。在此示例中，颜色为蓝色。
* 用于链接输出的 **Container** 引脚将显示在目标节点上。

![Example of automatic data pin typing.](/images/user-guide/scripting/script-canvas/variable-array-containers-8.png)

#### 链接数组操作节点

数组操作节点通常具有 **Container** 输出引脚。您可以使用此输出引脚将多个操作链接到同一数组上，如下图所示。

![Chained array operation nodes.](/images/user-guide/scripting/script-canvas/variable-array-containers-9.png)
