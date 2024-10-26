---
description: ' 使用自定义状态机控制 Open 3D Engine动画编辑器中的动作。 '
title: 利用稀疏动作集定制状态机路由
---

您可能会遇到这样的情况，即您有不同的角色，而这些角色的动作与您为其创建动画图形的角色并不完全相同。您可以为所有角色共享同一个动画图，而不必为不同角色复制大部分动作或创建新的动画效果图。

您可以根据给定动作条目，允许或拒绝状态机中动作状态的转换和路由。

**Topics**
+ [Sparse Motion Sets](#sparse-motion-sets)
+ [Hierarchical Motion Sets and Overwriting Motion Entries](#hierarchy-motion-sets-and-overwriting-motion-entries)
+ [Customizing State Machines Based on Motion Sets](#customizing-state-machines-based-on-motion-sets)

## 稀疏动作集

动作集包含动作条目列表。每个动作条目都有一个可以定义的动作 ID。例如，您可以将动作 ID 命名为 **Walk** 或 **Jump**。动作条目可将 **Walk** 等自定义动作 ID 映射到特定的动作文件，例如 `/SampleProject/Animations/Human_Walk.motion`.

您可以使用 **+** 图标或动作组窗口中的文件夹图标将动作条目添加到动作组中。**+** 图标会创建一个新的动态条目，您可以在其中指定动态 ID，但不会将其分配给动态文件。您可以单击文件夹图标选择要添加的动议。系统会根据动作文件名为您生成默认的动作 ID。

**稀疏动作集**是指没有为其分配动作文件的动作条目。

在状态机中的动作状态上选择未指定的动作条目时，该动作会在节点周围显示红色边框。如果激活了动作状态，角色就会进入绑定姿势。在下面的示例中，由于 **Jump** 节点使用的是稀疏动作集，没有分配任何动作文件，因此角色的动画图不会从空闲状态过渡到跳跃状态。角色仍处于**Idle**节点。

![You can specify an unassigned motion entry to a sparse motion set.](/images/user-guide/actor-animation/animation-editor-sparse-motion-sets.png)

## 分层动作集和重写动作条目

动作集可包含存储在同一文件中的子动作集。子动作集继承父动作集的所有动作条目。

在下面的示例中，分层动作集的根层是**Human**动作。**Zombie**动作集是子动作集，继承自父动作集 **Human**。**Zombie**动作集没有 `Jump`动作条目。

**示例：分层动作集**

**Human (motion set)**
+ + Idle - `GenericIdle.motion`
+ + Walk - `GenericWalk.motion`
+ + Jump - `GenericJump.motion`

**Zombie (motion set)**
  + + Walk - `ZombieWalk.motion`

由于父动作集使用动作文件定义了`Jump`动作条目，因此子动作集将继承该动作。这意味着当**Zombie**动作集被激活时，使用该动作集的角色就会执行人类的`Jump`动作。

如果不想让僵尸角色跳跃，可以禁用该指定动作的继承。在子动作集中，您可以创建一个名为`Jump`的新动作条目，然后将其标记为未指定。这样就可以覆盖父动作条目，不为子动作集分配该动作条目。

**示例：未指定跳跃动作的子动作集**

**Human (motion set)**
+ + Idle - `GenericIdle.motion`
+ + Walk - `GenericWalk.motion`
+ + Jump - `GenericJump.motion`

**Zombie (motion set)**
  + + Walk - `ZombieWalk.motion`
  + + Jump - *Unassigned*

**Zombie**动作集使用父动作集的`Idle`动作，自定义`Walk`动作，并禁用`Jump`动作。

对于分层动作集，可以创建一个动作条目并取消分配，以禁用从父动作集继承的功能。如果不使用分层动作集，就等于没有指定动作 ID 的动作条目。

## 根据动作集定制状态机

动画图形可在不同角色间共享。使用相同动画图形的两个不同角色可以使用两种不同的动作集进行操作。例如，可以有一个使用人类动作集的人类角色和一个使用**僵尸**动作集的僵尸角色。这两个角色可以使用相同的动画图形。

您可以对状态机进行配置，以避免未指定的动作状态。在这个例子中，你不希望僵尸进入**Jump**状态，因为这个动作是未指定的僵尸动作集。

**配置状态机以避免动作集**

1. 在**Anim Graph**中，选择动作节点之间的过渡线。例如，您可以选择 **Idle** 节点和 **Jump** 节点之间的过渡线。

1. 在**Attributes**窗格中，单击**Add Condition**，然后选择**Motion Condition**。

1. 在**Motion**中，选择**Jump**状态。

1. 在**Test Function**中，选择**Is Motion Assigned?**。

![Add motion conditions so that some motion states are unassigned.](/images/user-guide/actor-animation/animation-editor-motion-condition.png)

   由于**僵尸**动作集没有为 `Jump`分配动作文件，角色无法从空闲状态过渡到跳跃状态。该条件的红绿灯显示为红色并阻止过渡。这样您就可以控制是否允许角色进入特定的动作状态。

   ![Customize state machines based on motion sets.](/images/user-guide/actor-animation/animation-editor-sparse-motion-sets-02.png)
