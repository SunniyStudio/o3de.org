---
description: ' 在 Open 3D Engine中为电影序列设置摄像机焦点。 '
title: 景深动画
---

摄像机对焦或景深（DoF）用于增加场景的真实感，模拟真实世界中摄像机的工作方式。您可以使用宽景深对整个场景进行对焦，也可以使用浅景深只对离摄像机有特定距离的物体进行清晰对焦。

请参阅以下设置摄像机焦点的指南和最佳实践：
+ 始终聚焦人物。
+ 慢慢地、有意识地转移焦点。
+ 不要过度对焦
+ 不要在远景中使用 DoF。近景最适合用来区分特写和背景。
+ 用眼睛在不同距离对焦，看看哪些清晰，哪些模糊。你可以用拇指来帮助对焦。这应该能让你感觉到在场景中应该是什么样子。

只有在 O3DE 编辑器视口中的单视图窗格布局（默认）下才会渲染 DoF。如果您使用的是多视图窗格布局，且序列摄像机不在活动窗格中，则不会渲染多视角。如果需要设置，请执行以下操作。

**为单视图窗格布局设置视口**

1. 在 O3DE 编辑器中，右击视口中的 **Perspective** 标题栏并选择 **Configure Layout**

1. 在 **Layout Configuration** 对话框，选择单个视图面板，然后点击 **OK**。

![Create animation tracks for a Camera component in the timeline for a sequence.](/images/shared/cinematics-cameras-focus-layout-configuration.png)

1. 再次右击 **Perspective** 标题栏并选择 **Sequence Camera**。

**添加Depth of Field 节点**

1. 在 Track View中，选择并创建序列。

1. 在节点浏览器中，右击 **Director** 节点或 **Camera** 节点并选择 **Add Depth of Field Node**。

**Camera**节点优先于**Director**节点。如果您想对多台摄像机进行相同的景深设置，请使用**Director**景深节点。在大多数情况下，您需要为每台摄像机单独设置特定的景深，以获得更多控制权。

您可以添加任意数量的节点，并使用 ****Curve Editor**** 进一步调整 DoF 设置，使其随时间变化。

更多信息，请参阅 [使用动画曲线](/docs/user-guide/visualization/cinematics/track-view/editor-animation-curves/)。
