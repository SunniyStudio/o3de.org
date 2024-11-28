---
linkTitle: UI Draggable
description: ' 使用 Open 3D Engine UI Editor 中的可拖动组件使 UI 元素可移动。'
title: UI Draggable 组件
weight: 190
---

您可以使用 **Draggable** 组件使 UI 元素在屏幕上从一个位置移动到另一个位置。将 **Draggable** 组件与 **DropTarget** 组件结合使用，以便拖动可以从可拖动元素开始，在放置目标元素上结束。拖放是 UI 屏幕（如库存系统）中的常见操作。

由于拖放行为是特定于游戏的，因此 **Draggable** 和 **DropTarget** 组件旨在与脚本或 C++ 一起使用，以定义拖放产生的操作。

![Animation of mouse dragging and dropping UI elements](/images/user-guide/interactivity/user-interface/components/interactive/ui-editor-components-draggable.gif)

要将可拖动组件添加到 UI 元素，请使用 **Properties** 窗格中的 **Add Component** 菜单。

下图显示了 **Draggable** 组件的示例，其中颜色已添加到 **Drag States**的状态操作中。

![UI Editor Draggable component](/images/user-guide/interactivity/user-interface/components/interactive/ui-editor-components-drag-drop-draggable.png)

**Draggable** 组件 是一个交互式组件。它具有标准的 [交互属性](properties).

**编辑 Draggable 组件**

在[**UI Editor**](/docs/user-guide/interactivity/user-interface/editor)的 **Properties** 面板中，展开 **Draggable** 并根据需要执行以下操作：

**Interactable**

见 [Properties](properties) 以编辑 Common Interactive Component 设置。

**Drag States**

为特定拖动状态定义此元素及其子元素的颜色、alpha、sprite 或字体。
当元素未被拖动时，具有可拖动组件的元素使用可交互状态（pause on、pressed 和 disabled）。

但是，在拖动时，可拖动组件具有另外三种状态：

    + **Normal** - 拖动状态开始时的 Automatic 状态。
   
    + **Valid** - 通常是可拖动组件在有效放置目标上暂停时使用的状态。此状态由您编写的脚本或连接到`UiDropTargetNotificationBus`并侦听`OnDropHoverStart`方法的 C++ 代码确定。
   
    + **Invalid** - 通常是可拖动组件在无效的放置目标上暂停时使用的状态。此状态由您编写的脚本或 C++ 代码确定。当触发有效的拖动状态时，将使用`UiDropTargetNotificationBus`自动发送通知。
   
脚本或 C++ 可以使用 `UiDraggableBus` 来设置 **Draggable** 组件的拖动状态。它还可以设置 **DropTarget** 的放置状态，以向用户指示有效的放置目标。

要查看简单拖动 Lua 脚本的示例，请打开`Gems\LyShineExamples\Assets\UI\Scripts\LyShineExamples\DragAndDrop`中的 `DraggableElement.lua`。
