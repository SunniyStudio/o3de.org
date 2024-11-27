---
linkTitle: 预览画布行为
description: ' 使用 Open 3D Engine UI Editor Preview 来可视化游戏 UI 画布中的动画和元素在不同分辨率下的表现。 '
title: 预览画布行为
weight: 200
---

在 **UI 编辑器** **预览** 中，画布中的 UI 元素的执行方式与游戏运行时一样。

请尝试以下示例：
+ 在交互式元素上暂停以显示其悬停状态。
+ 按（单击）交互式元素可显示其按下状态。
+ 调整滑块。
+ 输入和编辑文本。
+ 使用键盘、鼠标或游戏手柄与 UI 交互。

{{< note >}}
如果取消选择（取消选中）交互式组件的 **启用输入** 设置，则该元素将以其禁用状态绘制，并且不会响应悬停或单击操作。
{{< /note >}}

## Animation List 

**动画列表**窗格列出了您正在预览的画布上找到的所有 UI 动画序列。选择动画以使用 reset、play、pause 和 set-to-end 控件。按住 Ctrl 或 Shift可一次选择和控制多个动画。您还可以独立地同时控制动画，例如，当您暂停另一个动画时，一个动画可以播放。

## Action Log 

**操作日志**窗格显示在 **预览** 中通过与 UI 画布中的交互式元素交互生成的操作。这些记录的操作可帮助画布设计者确保触发正确的操作。

要使用此功能，您必须在交互式元素属性的 **Actions** 部分中键入文本字符串。

**启用操作日志条目**

1. 在 **UI 编辑器** 视区或 **层次结构** 窗格中，选择交互式组件附加到的元素。

1. 在 **Properties** 窗格的 **Actions** 类别下，为要触发操作日志条目的每个操作输入文本字符串。

   文本字符串是完全可定制的;您可以输入任何有助于确保触发正确操作的字符串。

例如，在下图中，每当 **Enable Input** 复选框更改状态（从 off 到 on，或从 on 到 off）时，都会显示 **EnablerChanged**。选中此复选框时显示 **EnablerOn**，取消选中此复选框时显示 **EnablerOff**。

![Action Log pane and canvas with check box](/images/user-guide/interactivity/user-interface/canvases/preview/ui-editor-previewing-action-log.png)

在 **预览** 期间，Script Canvas 和 Lua 脚本处于非活动状态。在 UI 画布 **预览** 中执行的操作对画布之外的任何内容都没有影响。
