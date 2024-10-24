---
description: ' 创建用于 Open 3D Engine 动画编辑器的自定义事件和参数。 '
title: 使用 C++ 创建自定义动作事件和参数
---

您可以创建带有自定义参数的自定义事件类，并将其反映到编辑上下文中，以便在动画编辑器中使用。通过创建带有已知参数的预定义动作事件，可以最大限度地减少复杂参数对性能的影响，并降低人为错误的风险。

在动画编辑器中，用户可以使用创建的事件和参数执行以下任务：
+ 选择要添加到动作中的事件
+ 为参数提供自己的值
+ 为事件添加附加参数，而不是在两个事件的时间相同时创建重复事件
+ 快速创建带有复杂参数的事件
+ 用他们选择的颜色标记动作事件，创建对他们有意义的配色方案
+ 将事件和参数信息保存在可重新加载的 XML 文件中

**主题**
+ [创建动作事件](/docs/user-guide/visualization/animation/character-editor/custom-events-parameters-creating-motion-events/)
+ [创建 EventData 类型](/docs/user-guide/visualization/animation/character-editor/custom-events-parameters-creating-eventdata-types/)
