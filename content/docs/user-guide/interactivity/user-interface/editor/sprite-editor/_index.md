---
linkTitle: Sprite Editor
description: ' 使用 Sprite 编辑器在 Open 3D Engine 的 UI 编辑器中配置图像和纹理。 '
title: Sprite Editor
weight: 400
---

**Sprite Editor ** 配置以下 Sprite 配置：
+ 切片图像类型的边框值
+ 精灵表

您可以从 **Image** 组件的属性中打开 **Sprite Editor**。

打开 Sprite Editor

1. 如上一节所述，打开 **UI 编辑器**。

1. 选择 **Sprite path** 旁边的省略号按钮，然后选择 Sprite 文件。

1. 在 **Sprite path**的右侧，单击箭头 ![Open in Editor Icon](/images/user-guide/interactivity/user-interface/editor/sprite-editor/ui-editor-components-button-1.png) 图标。

![To open the Sprite Editor, click the arrow button next to Sprite path.](/images/user-guide/interactivity/user-interface/editor/sprite-editor/ui-editor-sprite-editor-1.png)

**Sprite Editor** 具有以下特点：

![Sprite Editor UI.](/images/user-guide/interactivity/user-interface/editor/sprite-editor/ui-editor-sprite-editor-2.png)
+ **Sprite viewport** - 显示 Sprite 图像。
+ **Border manipulators** - 设置切片图像类型的边框属性。要调整边界，请拖动虚线，这些虚线称为操纵器位置。更改这些位置会更新相应的 **Border Properties** 值。
+ **Properties**
  + **Image resolution** - 图像的大小。
  + **Alias** - 单元格所代表内容的简短描述。使用此设置可提高 sprite-sheet 索引值的可读性。您可以对多个 Sprite 表单元格使用相同的别名字符串。
  + **Top**, **Bottom**, **Left**, **Right** - 切片区域所在的图像的相应边缘的像素数。
+ **Configure Spritesheet** - 仅适用于当前未配置为 Sprite 表的 Sprite。
