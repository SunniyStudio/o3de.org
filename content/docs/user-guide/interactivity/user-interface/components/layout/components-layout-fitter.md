---
linkTitle: UI Layout Fitter
description: ' 在 Open 3D Engine UI Editor 中向元素添加布局拟合器组件，以使元素适合其内容。 '
title: UI Layout Fitter 组件
weight: 500
---

您可以使用 **LayoutFitter** 组件使元素调整自身大小以适应其内容。将此组件与提供单元格大小调整信息的其他组件一起使用，例如 **Text** 组件、**Image** 组件 (将 **ImageType** 设置为 **Fixed**), 或 **Layout** 组件 (**Cell**, **Row**, **Column**, **Grid**)。

要查看带有 **LayoutFitter** 组件的完整画布的游戏内示例，请在项目 SamplesProject 中打开关卡 UiFeatures。按 **Ctrl+G** 玩游戏，然后选择**Components**, **Layout Components**, **Layout Fitter**。按下 **Esc** 退出游戏。

To view that same canvas in the **UI Editor**, navigate to the `\Gems\LyShineExamples\Assets\UI\Canvases\LyShineExamples\Comp\Layout` directory and open the `\fitter.uicanvas` file.

![fitter.uicanvas canvas](/images/user-guide/interactivity/user-interface/components/layout/ui-editor-component-layout-fitter-canvas.png)

**编辑布局拟合器组件**

在 [**UI Editor**](/docs/user-guide/interactivity/user-interface/editor) 的 **Properties** 面板中，展开 **LayoutFitter** 并根据需要执行以下操作：

**Horizontal Fit**

选中该复选框可调整元素宽度的大小以适合其内容。

**Vertical Fit**

选中该复选框可调整元素的高度以适合其内容。
