---
linkTitle: 动画
description: ' 学习使用 EMotionFX 动画编辑器在 Open 3D Engine中制作角色动画。 '
title: 动画概述
---

大多数游戏项目都需要一个动画角色在环境中移动。这可以是玩家控制的角色，也可以是与关卡互动的人工智能驱动实体。

使用**EMotion FX 动画编辑器**在 Open 3D Engine 中制作角色动画。要创建角色，您需要将一个或多个带皮肤的模型与动画骨架（在 Maya 等数字内容创建工具中创建）关联起来。然后将角色导入**动画编辑器**，并指定您希望角色具有的动画。

然后，您可以混合动画，使角色从一个动画过渡到另一个动画，并指定角色发生动画的条件。例如，您可以指定角色开始时处于空闲状态。几秒钟后，角色开始行走、奔跑，然后再次放慢速度，直到角色回到空闲位置。

在**动画编辑器**中，您可以预览角色的动画和动画之间的混合。

O3DE 具有**FBX 设置**工具，可将静态 `.fbx`网格、骨骼、皮肤、动画和材质转换为 O3DE 资产。更多信息，请参阅 [使用 FBX 设置自定义 FBX 资产导出](/docs/user-guide/assets/scene-settings/)。

**主题**
+ [设置Actor实体](actor-component-entity-setup)
+ [为Actor使用多个蒙皮附件](actor-multiple-skin)
+ [动画编辑器概念和术语](/docs/user-guide/visualization/animation/character-editor/concepts-and-terms/)
+ [动画编辑器用户界面](/docs/user-guide/visualization/animation/animation-editor/user-interface/)
+ [动画编辑器文件类型](/docs/user-guide/visualization/animation/character-editor/file-types/)
+ [动画编辑器入门](/docs/user-guide/visualization/animation/animation-editor/quick-start/)
+ [引用外部Anim Graph](/docs/user-guide/visualization/animation/referencing-character-animation-editor-anim-graph/)
+ [同步Anim Graph： 示例](/docs/user-guide/visualization/animation/character-editor/sync-graph/)
+ [动画编辑器组件](/docs/user-guide/visualization/animation/character-editor/components/)
+ [使用Morph Target变形角色](/docs/user-guide/visualization/animation/animation-editor/using-morph-targets-to-deform-characters/)
+ [利用稀疏运动集定制状态机路由](/docs/user-guide/visualization/animation/animation-editor/customizing-state-machines-with-sparse-motion-sets/)
+ [动画编辑器节点](/docs/user-guide/visualization/animation/animation-editor/node/)
+ [在Anim Graph中使用标签](/docs/user-guide/visualization/animation/animation-editor/using-tags/)
+ [自定义EMotion FX对象](/docs/user-guide/visualization/animation/animation-editor/customizing-emotionfx-objects/)
+ [使用C++创建自定义动作事件和参数](/docs/user-guide/visualization/animation/character-editor/custom-events-parameters/)
+ [创建和模拟PhysX Ragdoll](/docs/user-guide/visualization/animation/animation-editor/ragdoll/)
+ [重定向动作](/docs/user-guide/visualization/animation/animation-editor/retargeting-animations/)
+ [创建模拟对象](/docs/user-guide/visualization/animation/animation-editor/creating-simulated-objects/)
+ [向Actor添加布料碰撞体](/docs/user-guide/visualization/animation/character-editor/cloth-colliders/)
