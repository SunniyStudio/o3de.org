---
linkTitle: Tooltip 组件
description: ' 使用工具提示组件在将鼠标悬停在元素上时为其提供工具提示，并在 O3DE 的 UI 编辑器中配置其显示属性。 '
title: UI Tooltip 和 Tooltip Display 组件
weight: 600
---

您可以向元素添加 **Tooltip** 组件或 **TooltipDisplay** 组件。使用这些组件，您可以在将鼠标悬停在交互式元素上时显示工具提示。

要查看带有 **Tooltip** 组件的完整画布的游戏内示例，请打开项目 SamplesProject 中的关卡 UiFeatures。按 Ctrl+G** 玩游戏，然后选择 **Components**、**Other Components**、**Tooltips**。您可以查看工具提示文本选项和显示样式的示例。按 **Esc** 退出游戏。

要在 UI 编辑器 中查看这些相同的画布，请导航到`\Gems\LyShineExamples\Assets\UI\Canvases\LyShineExamples\Comp\Tooltips` 目录。

您可以打开以下画布：
+ `TextOptions.uicanvas`
+ `Tooltips.uicanvas`

## Tooltip 

您可以使用 **Tooltip** 组件来提供工具提示的文本。将工具提示组件添加到任何交互式元素中，以在 pause 状态下显示工具提示。

**编辑Tooltip组件**

1. 在[**UI Editor**](/docs/user-guide/interactivity/user-interface/editor)的**Properties**窗格中，展开 **Tooltip**.

1. 输入文本字符串。

## TooltipDisplay 

**TooltipDisplay** 组件定义工具提示的显示行为。向元素添加 **TooltipDisplay** 组件，以直观地表示工具提示。还必须将画布的 **Tooltip display element** 属性设置为此元素。有关详细信息，请参阅 [配置 Canvas 属性](/docs/user-guide/interactivity/user-interface/canvases/canvas-properties).

**编辑 TooltipDisplay 组件**

+ 在[**UI Editor**](/docs/user-guide/interactivity/user-interface/editor)的**Properties**窗格中，展开 **TooltipDisplay** 并根据需要使用以下设置：

**Trigger Mode**

选择工具提示触发条件：

+ **On Hover** - 当指针悬停在交互式元素上时，工具提示会出现，当指针离开交互式元素时，工具提示会消失。
+ **On Press** - 工具提示在按住交互式元素时显示，并在释放按时消失。请注意，在发生释放操作时，指针可能已移动到画布上的其他位置。
+ **On Click** - 当交互式元素上发生指针单击（包括按下和释放）时，将显示工具提示。当下一次指针单击发生在画布上的任意位置时，工具提示将消失。请注意，如果指针单击同一实体，则工具提示将消失，但在指定的 **Delay time** 之后会重新出现。

    {{< tip >}}
在移动设备上，您可能希望使用 **On Press** 或 **On Click** 而不是 **On Hover**。
    {{< /tip >}}

    {{< note >}}
在所有情况下，工具提示的显示都会延迟 **Delay time** 中指定的时间量。此外，在所有情况下，工具提示都将在 O3DE 设置的固定时间后消失，而不管触发条件中指定的其他条件如何。
    {{< /note >}}

**Auto position**

根据定位模式自动定位元素。定位模式在 **Positioning** 属性中指定。

**Positioning**

选择定位模式：
+ **Offset from mouse** - 定位元素，使其枢轴与指针保持一定距离。距离在 **Offset** 属性中指定。
+ **Offset from element** - 定位元素，使其枢轴与触发工具提示显示的元素枢轴保持一定距离。

**Offset**

自动定位元素时使用的偏移量。

**Auto size**

自动调整元素的大小以匹配工具提示字符串的大小。text 元素是元素的子元素，其 text 在 **Text** 属性中指定。如果选择了 **Auto size**，则文本元素的锚点应分开，以便文本元素可以随其父元素一起放大和缩小。

**Text**

用于显示工具提示字符串的子元素。

**Delay time**

显示元素之前等待的时间。

**Display time**

元素的显示时间。

**Show sequence**

元素即将出现时要播放的动画序列。

**Hide sequence**

当元素即将消失时要播放的动画序列。
