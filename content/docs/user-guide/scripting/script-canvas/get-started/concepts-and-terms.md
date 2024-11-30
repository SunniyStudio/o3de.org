---
linktitle: 概念和术语
title: Script Canvas 概念和术语
description: 了解常见的 Script Canvas 概念和术语。
weight: 200
---

以下概念和术语在 **Script Canvas** 中常用：

## Script（脚本）

脚本是节点、节点属性和节点连接的集合，当它们组合在一起时，将创建一个可视化脚本。

## Node（节点）

节点表示用于在 Script Canvas 中创建逻辑和行为的数据、事件和操作。

## 节点类型

Script Canvas 定义了以下_node types_：

### Event （事件节点）

事件节点订阅事件总线 （EBus） 处理程序以侦听要发生的事件。示例包括进入触发区域、与对象碰撞、关闭灯光以及游戏计时。有关使用 EBus 界面的更多信息，请参阅 [Open 3D Engine 事件总线 （EBus） 系统](/docs/user-guide/programming/messaging/ebus/).

### Action （操作节点）

操作节点用于跨事件总线获取或发送数据。操作节点的示例包括获取实体的质量、打开灯、设置 UI 元素的文本以及播放动画。

### 变量节点和数据节点

Variable 和 data 节点表示构建游戏逻辑可能需要的自定义数据。您可以使用这些节点创建计数器、存储实体引用、指定方向、定义颜色等。将变量节点添加到脚本中以声明和初始化它们。使用 _get_ 和 _set_ 节点检索或设置变量的值。

{{< note >}}
变量节点的替代方法是 [变量引用](/docs/user-guide/scripting/script-canvas/editor-reference/variables/variable-references).
{{< /note >}}

以下是 Script Canvas 中常用的数据类型：

+ Boolean
+ Color
+ Entity
+ Number
+ String
+ Transform
+ Vector 2/3/4

### Logic （逻辑节点）

Logic 节点包括 comparison 和 timing operations 。您可以使用 logic nodes 来检查两个值是否相等、控制节点的执行、将节点的执行延迟特定时间等等。

### Math （数学节点）

数学节点支持数学运算，例如算术、几何、代数和微积分。

### Debugging （调试节点）

调试节点验证脚本是否按预期运行。您可以使用调试节点将数据打印到控制台或视区并检查错误。这些节点传递 logic flow，但不在发布版本中执行。

### User-defined （用户定义节点）

您可以根据项目的特定需求构建自己的节点。有关详细信息，请参阅 [在 Script Canvas 中创建自定义节点](/docs/user-guide/scripting/script-canvas/programmer-guide/custom-nodes/).

## Node Palette

**Node Palette** 包含可搜索的节点列表。默认情况下，调色板停靠在 **Script Canvas Editor** 的左侧。

**显示 Node Palette**

1. 从 O3DE 编辑器中，依次选择 **Tools**、**Script Canvas**。

1. 在 Script Canvas 窗口中，执行以下操作之一：
   + 选择 **Tools**, **Node Palette**.
   + 按下 **Ctrl+Shift+L**.

   {{< note >}}
如果您打开了现有脚本，则可以 **右键单击** 画布以打开上下文菜单。
   {{< /note >}}

## Node Inspector

**Node Inspector **显示节点的属性。您可以在 Inspector 中或直接在节点中编辑每个属性。默认情况下，此窗口不会显示在编辑器中。

**显示 Node Inspector **

1. 在 **O3DE Editor**中， 选择 **Tools**, **Script Canvas**.

1. 执行以下操作之一：
   + 选择 **Tools**, **Node Inspector**.
   + 按下 **Ctrl+Shift+I**.
