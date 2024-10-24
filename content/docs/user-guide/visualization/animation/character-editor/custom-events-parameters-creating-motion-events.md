---
description: ' 在 Open 3D Engine 中为您的游戏项目创建动作事件。 '
title: 创建动作事件
---

`EMotionFX::MotionEvent` 类继承于 `EMotionFX::Event` ，描述了动作过程中在指定时间或时间范围内发生的事件。

动作事件可以是播放的脚步声、生成的粒子系统或执行的脚本。由于动作事件是完全通用的，EMotion FX 不会为您处理它们。您必须创建并处理游戏所需的事件。

每个动作事件都有一个 [`EventData`](/docs/user-guide/visualization/animation/character-editor/custom-events-parameters-creating-eventdata-types/)  实例列表，这些实例附加到事件中。事件处理程序使用 `EventData`列表来执行所需的操作。

所有动作事件都存储在动作事件表中。动作事件表\(参见 `Motion.h`\)包含事件类型和参数的数据，这些数据可在事件之间共享。

要监听动作事件，请连接到 `ActorNotificationBus` 并实现 `OnMotionEvent()`。

有关创建 `EventData` 实例和向动作添加动作事件的信息，请参阅 [创建事件数据类型](/docs/user-guide/visualization/animation/character-editor/custom-events-parameters-creating-eventdata-types/)。

**主题**
+ [MotionEvent公开成员函数](/docs/user-guide/visualization/animation/character-editor/custom-events-parameters-motionevent-public-member-functions/)
