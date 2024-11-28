---
linkTitle: Sprite Sheets
description: ' 使用 Sprite 表在 Open 3D Engine 的 UI 编辑器中为您的游戏 UI 保存单独图像的集合。 '
title: Sprite Sheets
weight: 200
---

您可以将图像配置为 Sprite 表。

Sprite 表是存储在单个图像中的单独图像（如图标、按钮和其他 UI 资源）的集合。尽管您可以将所有图像保存在单独的文件中，但使用 Sprite 表具有以下几个优点：
+ 更快的性能 - 对于该资产集合，O3DE 只能从磁盘加载一个映像，而不是多个单独的映像。加载多个图像需要许多硬盘驱动器查找，并且性能昂贵。
+ 工作流程改进 - 例如，当动画包含多个帧时，仅管理包含所有动画帧的一张图像比管理单独的文件更容易。这使得编辑和其他工作流程更加容易。

  其他工作流程改进包括更轻松地管理您的资产。例如，您可以有一个名为`mainmenu_ui_assets_spritesheet.png` ，其中包含您的所有按钮，而不是一系列文件，例如 `mainmenu_button1.png`, `mainmenu_button2.png`等等。

**示例**
下图在一行中包含 12 个行走图像。

![Walking images.](/images/user-guide/interactivity/user-interface/editor/sprite-editor/ui-editor-component-sprite-sheets-walking.png)

在以下过程中，您可以在配置 Sprite 表时将图像分为 12 列。然后，您可以选择要为 **Image** 组件显示的图像片段。

**将图像配置为 Sprite 表**

1. 在 [Sprite Editor](/docs/user-guide/interactivity/user-interface/editor/sprite-editor) 中，在左下角点击 **Configure Spritesheet** 。

   **Configure Spritesheet** 视图显示两个新部分，**Configure Spritesheet** 和 **Select cell**。

   ![Configure Spritesheet and Select cell.](/images/user-guide/interactivity/user-interface/editor/sprite-editor/ui-editor-component-sprite-sheets-1.png)

1. 输入行数和列数。行走图像示例有 12 列和 1 行。这些值将 sprite 划分为一个统一的网格，并假设 sprite 表的每个单元格都具有相同的 （uniform） 大小。

1. 在 **Select cell** 部分中，单击一个单元格以将其选中并显示其属性。

1. 要使用切片缩放配置单个单元格，请将虚线拖动到首选位置。**Top**, **Bottom**, **Left**, 和 **Right** 属性会自动更新以反映当前位置。

1. 点击 **Save** 以保存更改并关闭 **Sprite Editor**。或点击 **Cancel** 以还原更改并关闭 **Sprite Editor**.

1. 要选择要使用的 sprite 表的特定单元格，请在 **Image** 组件属性中，选择相应的 **Index** 编号。

   **Sprite Editor** 在 Sprite 表的行和列中分配索引号，从左到右，然后从上到下，从 0（零）开始。

   如果您在 **Sprite Editor** 属性中定义了 **Alias**，该属性也会显示在索引号旁边。

   ![Select Index number of sprite sheet.](/images/user-guide/interactivity/user-interface/editor/sprite-editor/ui-editor-component-sprite-sheets-2.png)

   您选择的单元格将显示在 **UI Editor** 视区中。
