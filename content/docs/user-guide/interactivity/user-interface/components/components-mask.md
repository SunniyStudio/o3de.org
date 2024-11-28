---
linkTitle: UI Mask
description: ' 将 Mask 组件添加到元素中，以在 Open 3D Engine 中显示子元素的各个部分。 '
title: UI Mask 组件
---

您可以向元素添加 **Mask** 组件，以仅显示掩码视觉对象中的子元素部分（例如，图像或文本）。换句话说，掩码视觉对象的不透明区域定义了一个形状，该形状的作用类似于一个窗口，您可以通过该窗口查看后代元素。


当您添加 **Mask** 组件时，默认掩码是该元素上的视觉组件，通常是 **Image** 组件。如果要使用非矩形蒙版，则必须将此 **Image** 组件设置为使用包含 [alpha通道](/docs/user-guide/appendix/glossary#alpha-channel) 的纹理，该纹理指定透明和不透明区域。子元素被掩码视觉对象屏蔽。这意味着子元素中唯一可见的部分是掩码视觉对象中的部分。换句话说，掩码的可见区域显示子元素，而掩码视觉对象的透明区域隐藏子元素。您可以使用其他视觉组件（如 **Text** 组件或 **Particle Emitter** 组件）来指定遮罩视觉对象。

您可以使蒙版可移动，以便在动画中使用。若要在不移动其所有子元素的情况下执行此操作，除了 mask 元素上的视觉组件之外，还可以使用特殊的子元素作为 mask 视觉对象。此子掩码元素可以具有绘制掩码视觉对象所需的任意数量的子元素。

掩码通常与 [ScrollBox Prefab元素](../interactive/components-scrollbox).

**添加简单蒙版，并将 Image （图像） 组件作为视觉对象**

1. 在 [**UI 编辑器**](/docs/user-guide/interactivity/user-interface/editor) 工具栏中，选择**New**、**Empty Element**。这是父元素。

1. 在 **Properties** 窗格中，选择 **Add Component**、**Image** 以添加 **Image** 组件。

1. 选择 **Add Component**、**Mask**。

1. 右键单击父元素，然后选择 **New**、**Empty Element** 以添加子元素。

1. 选择子元素并向其添加 **Image** 组件。

1. 在 **Properties** 窗格中，单击 **Image**、**Sprite Path** 旁边的文件夹图标，为子元素选择图像。

1. 在当前项目目录中打开一个图像文件。

1. 在父元素的 **Properties** 窗格中，单击 **Image**、**Sprite Path** 旁边的文件夹图标，以选择要用作蒙版的纹理或图像。

1. 在当前项目目录中打开一个图像文件。用作遮罩的图像应具有不透明区域（显示子元素中的内容）和透明区域（隐藏子元素中的内容）以及 AlbedoWithGenricAlpha 的 Texture 设置。

1. 在 **Properties** 窗格中，在 **Mask** 下，选择**Use alpha test**。

**编辑 Mask 组件**

在 [**UI 编辑器**](/docs/user-guide/interactivity/user-interface/editor) 的 **Properties** 窗格中，展开 **Mask** 并根据需要指定以下参数：

| 名称 | 说明 |
| --- | --- |
| **Enable Masking** | 如果设置，则只有掩码显示的子元素部分可见。默认情况下，该参数处于启用状态。 |
| **Draw behind** | 在子元素后面绘制掩码视觉对象。 这对于调试很有用。 |
| **Draw in front** | 在子元素前面绘制掩码视觉对象。 这对于调试很有用。 |
| **Use alpha test** | 使用掩码视觉对象纹理中的 Alpha 通道来定义掩码。 您必须为矩形以外的任何遮罩启用此参数。 |
| **Mask interaction** | 防止将输入事件设置为掩码之外的元素。 |
| **Child Mask Element** | 将其中一个子元素指定为作为掩码视觉对象的一部分绘制的特殊元素。 此参数有助于屏蔽其余子项。 |
