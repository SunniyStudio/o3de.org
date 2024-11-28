---
linkTitle: UI Drop Target
description: ' 将 Drop Target 组件与 Draggable 组件结合使用，可在 Open 3D Engine UI Editor 中实现拖放行为。 '
title: UI Drop Target 组件
weight: 200
---

您可以使用 **DropTarget** 组件通过 **Draggable** 组件实现拖放行为。

由于拖放行为是特定于游戏的，因此 **Draggable** 和 **DropTarget** 组件旨在与脚本或 C++ 一起使用，以定义拖放产生的操作。

要将 **DropTarget** 组件添加到 UI 元素，请使用 **Properties** 窗格中的 **Add Component** 菜单。

下图显示了 **DropTarget** 组件的示例，其中颜色已添加到 **Drop States** 的状态操作中。

![UI Editor Drop Target component](/images/user-guide/interactivity/user-interface/components/interactive/ui-editor-components-drag-drop-droptarget.png)

**DropTarget** 组件与交互式组件共享属性，例如状态操作和导航设置。

**编辑 DropTarget 组件**

在 [**UI 编辑器**](/docs/user-guide/interactivity/user-interface/editor) 的 **Properties** 窗格中，展开 **DropTarget** 并根据需要执行以下操作：

**Drop States**

定义此元素及其子元素的颜色、alpha、sprite 或字体，使其处于有效或无效的放置状态。默认情况下，放置目标的放置状态为 normal，这意味着没有视觉覆盖。

在放置过程中，放置目标组件可以是 **Valid** 或 **Invalid**。由于放置目标组件不知道什么是有效的拖放操作，因此您可以使用脚本或 C++ 将放置目标切换到 **Normal**, **Valid**, 和 **Invalid**状态。这通常是通过连接到 `UiDropTargetNotifications` 总线并侦听`OnDropHoverStart` 和 `OnDropHoverEnd`通知来实现的。

**Navigation**

导航设置控制键盘或游戏手柄导航在拖放操作期间的工作方式。使用键盘时，您可以在可拖动元素上按 **Enter** 以进入拖动模式。然后，您可以使用箭头键，使用此处指定的导航设置将元素从一个放置目标移动到另一个放置目标。

**Actions, On Drop**

输入文本字符串。每当将可拖动对象拖放到此放置目标上时，此字符串都会作为 UI 画布上的操作发送。为了更好地控制，我们建议您改用`UiDropTargetNotifications`总线。

要查看简单的放置目标 Lua 脚本示例，请在`Gems\LyShineExamples\Assets\UI\Scripts\LyShineExamples\DragAndDrop`中打开`DropTarget.lua`。
