---
description: ' 在Open 3D Engine编辑器中模拟布娃娃。'
title: 在编辑器中模拟布娃娃
weight: 300
---

创建好布偶和动画图形后，就可以在 **O3DE 编辑器**中以游戏模式模拟布偶了。

**模拟布娃娃**

1. 在 O3DE 编辑器中，右键单击视口并选择 **Create entity**。

1. 在**Entity Inspector**中，对**Name**，输入 **Ragdoll**。

1. 添加 **Actor** 组件：

   1. 点击 **Add Component**, **Actor**。

   1. 在 **Actor** 组件中，对 **Actor asset**，点击 (**...**) 按钮。

   1. 在 **Pick EMotion FX Actor** 窗口中，选择要为其设置布娃娃的Actor，然后点击 **OK**。

   ![Select the actor for which you set up the ragdoll from the Pick EMotion FX Actor window in the Animation Editor](/images/user-guide/actor-animation/ragdoll-simulation-pick-ragdoll-actor.png)

1. 添加 **Anim Graph** 组件：

   1. 点击 **Add Component**, **Anim Graph**。

   1. 在 **Anim Graph** 组件中，对 **Motion set asset**，点击浏览 (**...**) 按钮。

   1. 在 **Pick EMotion FX Motion Set** 窗口中，选择你的动作集，然后点击 **OK**。

      ![Select a motion set from the Pick EMotion FX Motion Set window in the Animation Editor](/images/user-guide/actor-animation/ragdoll-simulation-pick-motion-set.png)

   1. 在 **Anim Graph** 组件中，对于 **Anim graph**， 点击浏览(**...**) 按钮。

   1. 在**Pick EMotion FX Anim Graph** 窗口中，选择你的动画图表，然后点击**OK**。

      ![Select an animation graph from the Pick EMotion FX Anim Graph window in the Animation Editor](/images/user-guide/actor-animation/ragdoll-simulation-pick-animation-graph.png)

1. 点击 **Add Component**, **PhysX Ragdoll**。

1. 要启动关卡并模拟布娃娃，按下 **Ctrl+G**。
