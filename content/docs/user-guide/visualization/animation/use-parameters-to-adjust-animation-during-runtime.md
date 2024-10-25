---
description: ' 设置参数，以便在 Open 3D Engine中自定义模拟对象的动画。 '
title: 在运行期间使用参数调整动画
---

您可以调整**SimulatedObject**节点，以便在运行时改变其动画效果。为此，请创建一个**Parameter**节点，并将其附加到动画图形中的节点上。否则，**SimulatedObject** 节点将使用您在**Attributes**面板中输入的属性。

**在运行时调整动画**

1. 在 **Parameters** 面板中，点击 **+** 图标并选择 **Add parameter**。

1. 输入以下值：

    + 对于 **Value type**，选择`Float (slider)`。
    + 对于 **Name**， 输入 `Stiffness`。
    + 对于 **Default**， 输入 `1.0`。
    + 对于 **Minimum**， 输入 `1.0`。
    + 对于 **Maximum**， 输入 `100`。

   您的参数应如下所示。

    ![Create a stiffness parameter.](/images/user-guide/actor-animation/simulated-objects-25.png)

1. 点击 **Create**。

1. 重复 **第 2 步** 和 **第 3 步** ，但是更改 **Name** 为 `Gravity`, **Default** 为 `1.0`, **Minimum** 为 `0` 和 **Maximum** 为 `5`。

1. 重复 **第 2 步** 和 **第 3 步** ，**Name** 为 `Damping`, `Default` 为 `1.0`, **Minimum** 为 `1` and **Maximum** 为 `100`。

1. 重复 **第 2 步** 和 **第 3 步** ，**Name** 为 `Weight`, **Default** 为 `1`, **Minimum** 为 `0` and **Maximum** 为 `1`。

   您的参数应如下所示。

    ![Create your parameters for your anim graph.](/images/user-guide/actor-animation/simulated-objects-26.png)

1. 在**Anim Graph**网格中，右击并选择 **Create Node**, **Sources**, **Parameters**。

1. 在**Parameters0**节点上，将**Stiffness**输出连接到**Stiffness factor**，将**Gravity**输出连接到**Gravity factor**，等等。

   您的图表应如下所示。

    ![Connect the Parameters0 node to the SimulationObject0 node.](/images/user-guide/actor-animation/simulated-objects-27.png)

1. 支付动画图，调整**Parameters**的滑块，查看您的更改。

    **注意**
    
    模拟对象、模拟关节和动画图形上的参数共享以下属性：  **Stiffness**, **Gravity**, **Damping**。 调整属性时，**动画编辑器** 会使用以下方式计算这些属性的结果：

    + Stiffness factor parameter * simulated object stiffness * simulated joint stiffness
    + Gravity factor parameter * simulated object gravity * simulated joint gravity
    + Damping factor parameter * simulated object damping * simulated joint damping
