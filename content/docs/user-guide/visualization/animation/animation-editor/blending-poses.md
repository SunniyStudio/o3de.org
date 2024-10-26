---
description: ' Learn how to blend poses using blend nodes in the Open 3D Engine Animation Editor
  . '
title: 使用 Blend 节点混合姿势
---

您可以在动画图形中使用**Blend**节点来创建混合两个输入姿势的动画。

**动画编辑器**有以下几种混合节点，每种节点以不同的方式混合姿势：
+ [Blend N 节点](/docs/user-guide/visualization/animation/animation-editor/blending-blend-n/)
+ [Blend Two Additive 节点](/docs/user-guide/visualization/animation/animation-editor/blending-blendtwoadditive/)
+ [Pose Subtract 节点](/docs/user-guide/visualization/animation/animation-editor/blending-posesubtract/)
+ [Blend Two 节点](/docs/user-guide/visualization/animation/animation-editor/blending-blendtwo/)
+ [Blend Two (Legacy) 节点](/docs/user-guide/visualization/animation/animation-editor/blending-blendtwolegacy/)

## Blend 节点属性

混合节点具有一组属性，可控制两个节点混合方式的不同方面。某些混合节点具有不同的属性，这些属性将在有关该节点类型的章节中进行描述。

### Sync Mode 

**Sync Mode** 属性决定同步动作片段的方法，以保持脚步同步。

![Blend node attributes: Sync Mode.](/images/user-guide/actor-animation/animation-editor-blending-attributes-1.png)


****

| 属性 | 说明 |
| --- | --- |
| Disabled |  同步已禁用。  |
| Event Track Based |  基于同步事件轨迹的同步。这种方法将同步事件配对，确保每个输入中的事件同时激活。  |
| Full Clip Based |  根据整个片段的持续时间进行同步。输入按已完成的百分比同步。例如，当第一路输入的播放时长为输入时长的 25%，则第二路输入的播放时长也为 25%。 |

### Event Filter Mode 

**Event Filter Mode** 属性决定哪个节点的事件会被触发。


****

| 属性 | 说明 |
| --- | --- |
| Leader Node Only |  仅从leader节点发出事件。跟随者节点与该节点同步。 |
| Follower Node Only  |  仅从follower节点发出事件。跟随节点与leader节点同步。  |
| Both Nodes |  同时从leader和follower节点发出事件。  |
| Most Active |  从更活跃的节点发出事件。叠加混合的特殊用例。  |

### Extraction Mode 

**Extraction Mode** 属性控制动作提取在混合时的行为方式。例如，对于状态机内部的转换，可以使用此节点来确保完成 180 度转弯。

![Blend node attributes: Extraction Mode.](/images/user-guide/actor-animation/animation-editor-blending-attributes-3.png)


****

| 属性 | 混合 |
| --- | --- |
| Blend |  在源和目标之间进行混合。该设置有助于确保适当的混合效果。例如，从空转过渡到转弯时，平移和旋转会混合在一起。部分转弯的旋转可能会在混合过程中丢失，这意味着 180 度的转弯可能只达到 160 度。使用该混合设置可确保完成转弯。  |
| Target Only |  只提取目标。例如，从空闲状态过渡到转弯时，只提取转弯动画的平移和旋转。骨架内部的节点仍正常混合。这只会影响角色的旋转和平移。  |
| Source Only |  例如，当从空转过渡到转弯时，只提取空转姿势的平移和旋转。骨架内部的节点仍然正常混合。这只会影响角色的旋转和平移。  |

### Mask 

**Mask** 来选择要包含在混合效果中的骨骼节点。

**示例**
行走动作可能是第一个输入姿势，挥手动作可能是第二个输入姿势。如果选择手臂、手掌和手指作为遮罩，并增加姿势的权重，手臂就会在身体行走时挥动。在本示例中，手臂骨骼将从行走动作转换为挥手动作。
