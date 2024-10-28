---
description: ' 在Open 3D Engine中的<guilabel>Track View</guilabel>中添加组件实体。'
title: 添加组件实体
---

在 Track View 中，序列会根据您添加到序列中的内容决定要制作的动画。例如，如果您想为带有**Actor**或**Light**组件的实体制作动画，可以将该组件实体添加到序列中，然后修改其属性。

**将组件实体添加到序列**

1. 在视口中或 **Entity Outliner** 中，选择实体。

1. 在Track View中，选择或创建想要的序列。

1. 完成以下之一：
   + 在节点浏览器中，右击并选择 **Add Selected Entity**。
   + 在 **Sequence/Node** 工具栏上，点击 **Add Selected Entity** 图标。

   ![Add a component entity to a sequence.](/images/user-guide/cinematics/cinematics-track-view-editor-adding-a-component-entity.png)

所有组件实体都有**Transform**组件。这意味着您可以根据需要对序列中每个组件实体的**Position**和**Rotation**属性进行动画处理。

如果实体上还有其他组件，这些组件也会出现在序列中。不过，有些组件无法在轨迹视图中显示动画。

对于可用的组件，可以右键单击它们，查看有哪些额外的轨迹可以添加到序列中。某些组件属性不能作为轨道添加到序列中，也不能制作动画。

更多信息，请参阅 [组件实体和组件节点](/docs/user-guide/visualization/cinematics/track-view/nodes-component-entity/)。

请参阅以下主题，了解如何在序列中使用常用组件实体。

**主题**
+ [添加光源](/docs/user-guide/visualization/cinematics/adding-lighting-to-scenes/)
+ [添加Camera](/docs/user-guide/visualization/cinematics/cameras-intro/)
