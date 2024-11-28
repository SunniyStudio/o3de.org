---
linkTitle: UI Flipbook Animation
description: ' 将 Flipbook Animation 组件添加到元素中，以在 Open 3D Engine 中创建基于图像的简单动画。 '
title: UI Flipbook Animation 组件
---

您可以使用 **FlipbookAnimation** 组件，通过对 [sprite sheet](/docs/user-guide/interactivity/user-interface/editor/sprite-editor/sprite-sheets) 的帧或单元格进行动画处理，创建简单的基于图像的动画。

**在 UI Editor 中查看画布**

1. 浏览至 `\Gems\LyShineExamples\Assets\UI\Canvases\LyShineExamples\Comp\Flipbook` 目录。

1. 打开 `Flipbook.uicanvas`.

您可以将 **FlipbookAnimation** 组件添加到同样具有 [**Image**](../visual/components-image) 组件。您还必须将该 **Image** 组件设置为使用已配置为 [Sprite Sheet](/docs/user-guide/interactivity/user-interface/editor/sprite-editor/sprite-sheets) 的 sprite 文件。

![FlipbookAnimation component and Image component set as sprite/texture asset](/images/user-guide/interactivity/user-interface/components/other/ui-editor-components-other-flipbook-1.png)

**添加和配置 FlipbookAnimation 组件**

1. 如果您还没有这样做，请为您的动画图像创建一个 [sprite sheet](/docs/user-guide/interactivity/user-interface/editor/sprite-editor/sprite-sheets)。

1. 添加一个 [**Image**](../visual/components-image) 组件。

   对于 **SpriteType**，选择 **Sprite/Texture asset**。

   在 **Sprite path** 中，点击 **Browse** (…) 并导航到包含您创建的 Sprite Sheet 资源的目录。选择 Sprite 表。

1. 添加一个 **FlipbookAnimation** 组件。

1. 配置 **FlipbookAnimation** 属性:

* **Start Frame**

    将作为动画中第一帧的 sprite sheet 单元格的索引。该值必须等于或小于 **End Frame**值。

* **End Frame**

    将作为动画中最后一帧的 sprite sheet 单元格的索引。该值必须等于或大于 **Start Frame** 值。

* **Loop start frame**

    sprite sheet 单元格的索引，该单元格将成为动画循环部分中的第一帧。此值必须等于或大于 **Start Frame** 值且小于 **End Frame** 值。如果 **Loop Type**（循环类型）设置为 **None**（无），则此设置无效。

    {{< tip >}}
要循环播放整个动画，请指定一个等于 **Start Frame** 值的值。要创建在循环动画之前显示的介绍序列，请指定大于 **Start Frame** 的值。
    {{< /tip >}}

* **Loop Type**

    包括以下选项：

   * **None** - 无循环行为。动画在 **Start Frame** 和 **End Frame** 之间开始，然后停止。
   * **Linear** - 当动画到达 **End Frame** 时，它会循环播放 **Start Frame**，并继续播放 **End Frame**。这种情况一直持续到播放器手动停止它。
   * **PingPong** - 反转动画的方向。动画到达 **End Frame** 或 **Start Frame** 后，它会反转方向。循环在两个帧之间来回移动，直到播放器停止它。

* **Frame delay**

    显示下一帧之前延迟的秒数。

* **Auto Play**

    如果启用，则在加载画布时自动开始播放 Flipbook 动画。
