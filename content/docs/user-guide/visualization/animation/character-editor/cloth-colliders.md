---
description: ' 在 Open 3D Engine中为 NVIDIA Cloth 仿真的Actor添加碰撞体。 '
title: 向Actor添加布料碰撞体
---


将布料模拟应用于Actor的每一个动作，都能为其增添真实感和动感。添加布料碰撞体可大大增强效果。布料碰撞体可以防止布料穿透Actor的网格。要添加布料碰撞体，必须启用 NVIDIA Cloth Gem。

更多信息，请参阅 [NVIDIA Cloth Gem 文档](/docs/user-guide/interactivity/physics/nvidia-cloth/)。

## 向Actor添加布料碰撞体

1. 在O3DE编辑器中，选择 **Tools**, **Animation Editor**。

1. 在 **Animation Editor**中，在菜单栏的右侧，从下拉列表中选择 **Physics** 。这会改变布局。 

1. 选择 **File**, **Open Actor**， 然后选择你的Actor。

1. 在 **Skeleton Outliner**中，选择想要添加布料碰撞体的关节。

1. 您可以添加碰撞体（胶囊或球体）。您也可以从Hit Detection, Ragdolls 或 Simulated Objects中复制它们。

![Add cloth collider to character.](/images/user-guide/actor-animation/nvidiacloth/ui-cloth-add-collider.png)

1. **Cloth Colliders** 选项卡会显示所选关节的布碰撞体列表。

1. 调整布料碰撞体的尺寸、偏移和旋转。

![Adjust cloth collider added to character.](/images/user-guide/actor-animation/nvidiacloth/ui-cloth-adjust-collider.png)

1. 选择 **File**, **Save Selected Actors**。
