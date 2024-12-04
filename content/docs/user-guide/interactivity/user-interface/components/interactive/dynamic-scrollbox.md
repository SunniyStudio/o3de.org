---
linkTitle: UI Dynamic Scroll Box
description: 使用动态滚动框组件可在运行时在 Open 3D Engine （O3DE） 中创建滚动框元素。
title: UI Dynamic Scroll Box 组件
weight: 185
---

使用 **DynamicScrollBox** 组件，您可以在运行时更改滚动框元素的子元素的数量。要使用 **DynamicScrollBox** 组件，请将其放置在同样具有 **ScrollBox** 组件的元素上。

内容元素会动态调整大小以适应其子元素。内容元素的第一个子元素充当 prototype 元素。在运行时，UI 系统会克隆 prototype 元素，以在布局中实现指定数量的子元素。

使用 **DynamicScrollBox** 组件时，实际上只创建最小数量的子元素以供显示。这与 [**DynamicLayout**](../layout/dynamic-layout) 组件，其中所有子元素都是在运行时创建的，并且可能会消耗大量资源。当用户滚动时，将重复使用 **DynamicScrollBox** 的元素;因此，Scroll Box 可以在保持良好性能的同时模拟大量子项。

**DynamicScrollBox** 组件会自动定位其子元素并调整内容元素的大小以匹配其子元素的边界框。每个子元素的大小与 prototype 元素相同。默认情况下，子项从左到右排成一行。如果启用了垂直滚动，则子项将从上到下放置在列中。

**使用动态滚动框组件**

1. 在 [**UI Editor**](/docs/user-guide/interactivity/user-interface/editor) 中，添加 **Scrollbox** Prefab。为此，请单击**New**, **Element from Prefab**, **ScrollBox**.

   这用作保存动态内容的结构或框架。

1. 将 **DynamicScrollbox** 组件添加到您的滚动框组件中。为此，请在 **Properties** 窗格中选择 **Add Component**, **DynamicScrollbox**.

   对于 **Default Num Elements（默认元素数）**，输入要创建的初始子项数。这主要用于在 **Preview** 模式下预览画布，因为子项的数量最终来自实现`UiDynamicScrollBoxDataBus`.

1. 创建具有 **Image** 组件的子实体。为此，请右键单击 **Hierarchy** 窗格中的滚动框组件，然后选择**New**, **Element from Prefab**, **Image**.

   此图像用作原型元素，该元素将被克隆并填充动态内容。

**DynamicScrollBox** 组件使用一个名为`UiDynamicScrollBoxDataBus`的总线来检索内容元素应具有的子项数。它还使用一个名为`Ui DynamicScrollBoxElementNotificationBus`的总线来通知子元素何时即将可见。这是设置具有动态内容的子元素的位置。为此，您必须创建一个实现这两个总线的自定义组件，并将其添加到 scroll box 元素中。

## EBus 接口 

将以下通知函数与 EBus 界面结合使用，以便与游戏的其他组件进行通信。

有关使用事件总线 （EBus） 接口的更多信息，请参阅 [使用事件总线 （EBus） 系统](/docs/user-guide/programming/messaging/ebus).

### UiDynamicScrollBoxDataBus:GetNumElements 

实现此总线以提供动态滚动框应克隆的子对象数。

返回动态滚动框应克隆的子项数。
**参数**
None

**返回值**
要克隆的子项数。

### Ui DynamicScrollBoxElementNotificationBus:OnElementBecomingVisible 

实现此总线，以便在动态滚动框的元素即将可见时接收通知。

当动态滚动框的元素即将变得可见时发送信号。

**参数**
`entityID` - 即将变为可见的元素的实体 ID。
`index` - 即将变为可见的元素的索引。

**返回值**
None
