---
title: Ragdoll
description: 学习如何在 Open 3D Engine (O3DE) 动画系统中使用布娃娃模拟角色的逼真行为。
---

布偶是动画系统中角色的物理表现形式，您可以用它来模拟角色的行为，如被击中的反应和角色死亡。物理表示由具有简单形状的刚体层次结构组成，这些刚体通过关节相连。动画系统和 PhysX 系统共同模拟逼真的行为。布娃娃的设置在动画系统中进行，而 PhysX 系统则负责角色如何根据环境交互和外力进行移动。例如，您可以对布偶进行设置，当您对角色的外侧肩部施加外力时，角色会随着倒塌而旋转。

要使用 **[PhysX Ragdoll](/docs/user-guide/components/reference/physx/ragdoll/)**组件，请在O3DE编辑器中将其添加到实体中。然后你就可以按照下面的步骤来创建和控制布娃娃的物理表示。

本主题将教您如何进行以下操作：

+ [为布娃娃设置物理配置](/docs/user-guide/visualization/animation/animation-editor/ragdoll/ragdoll-physics-setup.md)
+ [将布娃娃添加到动画图形中](/docs/user-guide/visualization/animation/animation-editor/ragdoll/ragdoll-adding-to-animation-graph.md)
+ [在 O3DE 编辑器中模拟布娃娃](/docs/user-guide/visualization/animation/animation-editor/ragdoll/ragdoll-simulating-in-editor.md)

