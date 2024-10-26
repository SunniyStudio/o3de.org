---
description: ' 在 Open 3D 引擎中创建可随角色（Actor）移动的模拟对象。 '
title: 创建模拟对象
---

在制作角色（Actor）的动画时，演员可能会穿戴一些与主运动不同的物体。例如，如果演员边跑边背着背包，背包可能会前后摇晃。要创建这种动态运动，需要创建一个模拟对象。模拟对象是演员身上骨骼的容器。在**动画编辑器**中，您可以指定松散物体（如链条、背包或长发）相对于演员的运动方式。

{{< note >}}
模拟物体不会与关卡中的布娃娃碰撞器或 PhysX 实体发生碰撞。
{{< /note >}}

在此程序中，您将执行以下操作：

1. 创建一个连接到演员的模拟对象。

1. 为骨架添加碰撞器，使对象与Actor的身体发生碰撞。

1. 调整设置，使动画看起来更加流畅逼真。

1. 在渲染窗口中查看模拟效果。

## 先决条件 

在开始前，你必须完成以下事项：
+ 在**动画编辑器**中完成以下操作：
  + 导入actor，如 `rinActor.fbx` 文件
  + 创建动作集
  + 导入动作，如 `rin_Run.fbx` 文件

**主题**
+ [设置模拟对象](/docs/user-guide/visualization/animation/set-up-a-simulated-object/)
+ [设置模拟对象Anim Graph节点](/docs/user-guide/visualization/animation/set-up-simulated-object-anim-graph-node/)
+ [设置模拟对象碰撞体](/docs/user-guide/visualization/animation/set-up-simulated-object-collider/)
+ [使用调试模式改进模拟](/docs/user-guide/visualization/animation/refine-simulationg-using-debug-mode/)
+ [在运行期间使用参数调整动画](/docs/user-guide/visualization/animation/use-parameters-to-adjust-animation-during-runtime/)
