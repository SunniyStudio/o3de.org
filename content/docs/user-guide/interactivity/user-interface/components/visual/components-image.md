---
linkTitle: UI Image
description: 使用 Open 3D Engine 的 UI 编辑器中的可视化组件向 UI 元素添加色调或纹理。
title: UI Image 组件
weight: 100
---

您可以使用 **Image** 组件向元素添加颜色、色调或纹理。使用 [UI 编辑器](/docs/user-guide/interactivity/user-interface/editor) 的 **Properties** 窗格为 **Image** 组件配置以下设置。

**Image 设置**

|名称 |描述 |
| --- | --- |
| SpriteType |  选择以下选项之一：   |
| Sprite path |  点击浏览 (**…**) 图标，然后选择合适的文件。单击 **Sprite path** 旁边的 open-in（箭头）图标以打开 [Sprite 编辑器](/docs/user-guide/interactivity/user-interface/editor/sprite-editor).  |
| Render target |  单击文件夹图标以选择附件图像资源。 |
| Index |  组件将呈现的 sprite 表图像索引。  |
| Color |  单击色样以选择其他颜色。 仅当 **SpriteType** 为 **Sprite/Texture asset** 并且已使用 [Sprite 编辑器](/docs/user-guide/interactivity/user-interface/editor/sprite-editor) 将图像配置为 Sprite 表时显示。  |
| Alpha |  使用滑块选择介于 **0** 和 **1** 之间的 alpha 值。  |
| Image type |  选择以下选项之一：   |
| Blend Mode |  选择以下选项之一：    |
| Fill Type |  选择以下选项之一：  |
| Fill Amount |  要填充的图像量，从 `0.00` 到 `1.00`.  |
| Fill Start Angle |  填充的起始角度，以垂直方向顺时针方向的度数为单位。  |
| Corner Fill Origin |  从中填充图像的起始角。选择以下选项之一：  |
| Edge Fill Origin |  图像填充的边缘（径向角）或填充（线性）。选择以下选项之一：   |
| Fill Clockwise |  如果选中，则图像将围绕填充中心顺时针径向填充。  |
| Fill Center |  如果选中，则切片调整大小的 sprite 的中心段可见。  |
