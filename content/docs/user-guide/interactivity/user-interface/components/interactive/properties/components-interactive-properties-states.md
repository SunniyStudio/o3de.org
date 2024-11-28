---
linkTitle: States
description: ' States 属性组定义 O3DE 中交互式元素及其子 UI 元素的外观。'
title: States
weight: 300
---

当交互式元素处于 **Hover**, **Pressed**, 和 **Disabled*** 状态时，**States** 属性组定义该元素及其子 UI 元素的外观。

视觉元素（定义为具有视觉组件的元素，如图像或文本）的正常外观由该视觉组件的属性定义。但是，可视化组件的某些属性可以被处于 **Hover**、**Pressed** 和 **Disabled** 状态的交互式组件覆盖。

**Hover**, **Pressed**, 和 **Disabled** 状态有一个 **状态操作**列表，这些操作定义该状态的外观，并覆盖可视组件的相应属性：
+ **Color** - RGB color tint
+ **Alpha** - Opacity
+ **Sprite** - Texture
+ **Font** - 文本字体和字体效果（例如，文本组件的字体和字体效果）

状态操作 --**Color**、**Alpha**、**Sprite** 和 **Font**-每个都有一个 **Target** 属性，用于指定要受影响的视觉元素。您可以从中进行选择的元素包括列为 **<This element>** 的当前元素、其子元素及其子元素的后代。使用 **Target** 属性，您可以准确选择要覆盖的视觉元素。

例如，按钮 UI 组件具有一个名为 **Button** 的 top 元素，该元素具有用于定义其颜色的视觉组件。它还具有一个子元素，该子元素带有一个文本组件，用于定义按钮的文本（及其颜色）。顶部元素 （**Button**） 还具有 **Interactable** 组件。**color** 状态操作的 **Target** 可以覆盖 **Button** 元素的颜色或 **Text** 元素的颜色，具体取决于您从列表中选择的内容。

![Example State Action](/images/user-guide/interactivity/user-interface/components/interactive/properties/ui-editor-components-interactive-states.png)

首次将交互式组件添加到元素时，默认情况下不会添加 **状态操作**。您必须将状态操作添加到要使用和修改的状态。

**将状态操作添加到状态**

在 [**UI 编辑器**](/docs/user-guide/interactivity/user-interface/editor) 的**Properties**窗格中，在交互式组件的名称（例如，**Button**）下，执行以下操作：

1. 在 **Interactable**下, **States**, 点击 **Add new element** (绿色的 **+**).

1. 从列表中，选择以下选项之一： **Color**, **Font**, **Sprite**, **Alpha**.

![Adding new State Actions](/images/user-guide/interactivity/user-interface/components/interactive/properties/ui-editor-components-interactive-stateactions.png)

**删除状态操作**
+ 单击要删除的状态操作旁边的 **Remove elemen**（红色 x）。

从状态中清除所有状态操作
+ 单击要从中清除所有状态操作的状态旁边的 **Clear container**（框图标）。
