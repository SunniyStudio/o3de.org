---
linkTitle: UI Text
description: 使用 Open 3D Engine 的 UI 编辑器中的文本组件将文本字符串添加到 UI 元素。
title: UI Text 组件
weight: 300
---

您可以使用 **Text** 组件向元素添加文本字符串。 使用 [UI 编辑器](/docs/user-guide/interactivity/user-interface/editor) 的 **Properties** 窗格为 **Text** 组件配置以下设置。


**Text 设置**

|名称 |描述 |
| --- | --- |
| Text |  输入首选文本字符串，然后按 Enter。您还可以应用 [文本样式标记](#text-markup).  |
| Enable markup |  如果选中，则为标记标签解析文本字符串。有关更多信息，请参阅 [Text Markup](#text-markup).  |
| Color |  单击色样以选择其他颜色。  |
| Alpha |  使用滑块选择介于 **0** 和 **1** 之间的 alpha 值。  |
| Font path |  单击按钮并选择字体 `.font` 文件。有关详细信息，请参阅 [添加新字体](/docs/user-guide/interactivity/user-interface/fonts/adding-fonts).  |
| Font size |  输入字体大小，然后按 Enter。  |
| Font effect |  从列表中选择一个效果。可用的字体效果由字体 `.font`文件决定。 |
| Horizontal text alignment |  选择 **Left**, **Center**, 或 **Right** 将文本与元素的左边框和右边框对齐。  |
| Vertical text alignment |  选择 **Top**, **Center**, 或 **Bottom** 将文本与元素的上边框和下边框对齐。  |
| Overflow mode |  选择 **Overlay** 以允许文本显示在元素边缘之外。选择 **Clip text** 以隐藏或剪切流出元素边缘的任何文本。  |
| Wrap text |  选择 **No wrap** 以防止文本换行到后续行。选择 **Wrap text** 以允许将文本分成单独的行。 |

## Text Markup 

您可以在单个文本字符串中使用粗体和斜体样式、多种文本颜色和多种字体来自定义游戏 UI 中的文本外观。您还可以在文本中嵌入图像。

为此，请直接在 **Text** 框中输入特定标签以及您的字符串。简单标记语言松散地基于 HTML。

要使用文本样式标记功能，您必须在 **Font path** 设置中使用字体系列 `*.fontfamily`  资产文件\(而不是单个 `.font` 资产文件\)。有关将字体系列添加到项目的更多信息，请参见 [实现新字体](/docs/user-guide/interactivity/user-interface/fonts/)。

**使用文本样式标记**

1. 在[**UI Editor**](/docs/user-guide/interactivity/user-interface/editor)中，添加**Text** component 拖动到画布上的元素（或修改现有组件）。

1. 选择元素后，在 **Properties** 窗格中，将 **Font path** 属性设置为`*.fontfamily`文件。

1. 在 **Text** 框中输入带有标记样式的字符串。有关示例，请参阅下一节。

### Styling Tags and Attributes 

**示例**
使用标记设置文本样式时，可以使用以下标记和属性：

**Bold** tag: 

![Example that uses a Text component to add bold in the UI Editor.](/images/user-guide/interactivity/user-interface/components/visual/this-text-bold.png)

**Italic** tag: 

![Example that uses a Text component to add italics in the UI Editor.](/images/user-guide/interactivity/user-interface/components/visual/this-text-italic.png)

**Font color** tag:

![Example that uses a Text component to add font color in the UI Editor.](/images/user-guide/interactivity/user-interface/components/visual/this-text-red.png)

**Font face** tag:

![Example that uses a Text component to add different fonts in the UI Editor.](/images/user-guide/interactivity/user-interface/components/visual/this-text-font.png)

### Image 标签和属性

使用  `<img>` 标签在文本中嵌入图像。

**示例**

```
<img src="images/icons/button" vAlign="center" height="fontHeight" scale="1.5" yOffset="5" xPadding="6"/>

<img src="images/icons/button" vAlign="bottom" height="100" scale="0.8" yOffset="-3" lPadding="2" rPadding="6"/>
```

使用以下属性自定义映像。只需要 `src` 属性。所有其他属性都是可选的。


****

| 属性 | 示例 | 说明 |
| --- | --- | --- |
| src | src="images/icons/button" |  要显示的纹理的 Asset path。  |
| vAlign | vAlign="center" |  文本区域中的图像对齐方式。文本区域是指字体中最大字形的总高度。如果未指定此属性，则默认对齐方式为 `baseline`.   |
| height | height="100" | 设置图像高度并调整其宽度以保持长宽比。如果未指定此属性，则默认对齐方式为 `fontAscent`.   |
| scale | scale="0.8" | 按指定的比例因子缩放图像。如果未指定，则此属性默认为 1.0。 |
| yOffset | yOffset="-3" |  按指定的浮点值（以像素为单位）垂直偏移图像。如果未指定，则此属性默认为 `0`.  |
| xPadding | xPadding="6" |  在图像前后按指定的浮点值（以像素为单位）添加相等的间距。如果未指定，则此属性默认为 0.0。  |
| lPadding | lPadding="10" |  按指定的浮点值（以像素为单位）向图像左侧添加间距。如果未指定，则此属性默认为 0.0。  |
| rPadding | rPadding="2" |  按指定的浮点值（以像素为单位）向图像右侧添加间距。如果未指定，则此属性默认为 0.0。 |
