---
linkTitle: 切片图像类型
description: ' 使用切片图像类型在 Open 3D Engine 的 UI 编辑器中智能地调整游戏 UI 的某些部分的大小。 '
title: 切片图像类型
weight: 100
---

O3DE 使用切片大小调整来智能地调整某些图像的大小。例如，使用常规调整大小来加宽下面的图像会导致角和边缘扭曲。

![Image with corner detail.](/images/user-guide/interactivity/user-interface/editor/sprite-editor/ui-editor-component-9-sliced-1.png)

![Scaled without slice scaling.](/images/user-guide/interactivity/user-interface/editor/sprite-editor/ui-editor-component-9-sliced-3.gif)

切片大小调整将图像分为 9 个部分，这些部分的缩放方式可以保留边框和角细节。此技术避免了典型图像缩放时发生的失真。

每个区域的大小调整如下：
+ 水平和垂直调整中心大小
+ 角根本不调整大小
+ 顶部和底部边缘仅水平调整大小
+ 右侧和左侧边缘仅垂直调整大小

切片大小调整对于具有边框和角细节的图像（如具有圆角的按钮）非常有用。

![Scaling of each section in slice scaling.](/images/user-guide/interactivity/user-interface/editor/sprite-editor/ui-editor-component-9-sliced-2.png)

![Scaled with slice scaling.](/images/user-guide/interactivity/user-interface/editor/sprite-editor/ui-editor-component-9-sliced-4.gif)

使用 [Sprite Editor](/docs/user-guide/interactivity/user-interface/editor/sprite-editor)，您可以通过拖动预览图像上的虚线来操纵部分的位置。

{{< tip >}}
您可以实时查看您的更改。为此，在打开 **Sprite Editor** 之前，请将 **Image** 组件的 **ImageType** 属性更改为 **Sliced**。
{{< /tip >}}

![Select Sliced as your ImageType.](/images/user-guide/interactivity/user-interface/editor/sprite-editor/ui-editor-sprite-editor-3.png)

![Preview your sliced image.](/images/user-guide/interactivity/user-interface/editor/sprite-editor/ui-editor-sprite-editor-3.gif)
