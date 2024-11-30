---
linktitle: 添加注释
title: 在 Script Canvas 中添加注释
description: 添加注释以描述 Script Canvas 图形在 Open 3D Engine （O3DE） 中的工作原理。
weight: 200
---

您可以向脚本添加注释以描述其工作原理。

## 添加评论

Script Canvas 中的 **Comment** 节点是一个浮动文本块，您可以将其放置在图形画布中。

**将 Comment 节点添加到脚本中**

1. 在 Script Canvas 编辑器中，右键单击画布，然后选择**Add Comment**, **Note**.

1. 输入您的评论，然后按 **Enter**.

    ![Use the comment feature to add useful notes about your script.](/images/user-guide/scripting/script-canvas/nodes-commenting.png)

**编辑 Comment 节点**

1. 双击 Comment 节点。

1. 更新描述，然后按 Enter。

删除 Comment 节点

1. 要删除 Comment 节点，请执行以下操作之一：
    + 选择节点，然后按 **Delete**。
    + **右键单击** 节点，然后选择 **Delete**。

{{< tip >}}
Script Canvas 包括一个名为“Note”的预设 Comment 节点样式。要创建使用不同颜色或字体特征的新预设，请参阅 [创建注释和组预设](/docs/user-guide/scripting/script-canvas/editor-reference/nodes/organizing/creating-comment-and-group-presets).
{{< /tip >}}

## 自定义评论

您可以更改 Comment （注释） 节点上的颜色和字体设置。字体设置适用于注释中的整个文本。

**更改单个 Comment 节点的字体设置**

1. 选择 Comment 节点。

1. 在 **Node Inspector** 中，您可以进行以下更改：

    ![Use the Node Inspector to change font settings for comment nodes.](/images/user-guide/scripting/script-canvas/nodes-commenting-settings.png)

    + **Comment** - 键入节点的注释。
    + **Background Color** - 键入 RGB 值或使用 **Color Picker** 为节点选择背景颜色。
    + **Font Color** - 键入 RGB 值或使用 **Color Picker** 为节点中的文本选择颜色。
    + **Font Family** - 键入系统上安装的字体系列名称，例如 Arial。要使用默认字体系列，请键入`default`.
    + **Pixel Size** - 指定字体大小。默认值为 `16`.
    + **Weight** - 选择字体粗细，如粗体。默认值为 `Normal`.
    + **Style** - 选择字体样式，如斜体。默认值为 `Normal`.
    + **Vertical Alignment** - 在 Comment （注释） 中指定文本的垂直对齐方式，例如 bottom。默认值为 `Top`.
    + **Horizontal Alignment** - 在 Comment （注释） 中指定文本的水平对齐方式，例如 center。默认值为 `Left`.
