---
description: ' 使用Screen Fader节点在 Open 3D 引擎轨道视图序列中淡入淡出屏幕。'
title: Screen Fader 节点
---


使用**Screen Fader**节点在场景中淡入淡出屏幕。

**在轨道视图中添加屏幕渐变器节点**

1. 在轨道视图中，右键单击序列（顶部节点）或树中的**Director**节点（如适用），然后选择 **dd Screen Fader**。

1. 单击 **ScreenFader** 节点下的 **Fader** 键。

1. 双击该键，将其定位在时间线上突出显示的行上。

1. 双击绿色标记，并在**Key Properties**下输入**Value**。



**Screen Fader节点关键属性**

| 属性 | 说明 |
| --- | --- |
| Type | 指定 FadeIn 或 FadeOut 值。 |
| ChangeType |  对于这种过渡类型，您可以指定以下其中一种：    |
| Color | 指定用于渐变的 RGB 值。 |
| Duration | 指定淡入或淡出屏幕的时间。 |
| Texture |  指定用作屏幕叠加的纹理文件。Alpha 纹理通常用于制作污垢或血迹等效果。纹理将乘以颜色值，以便在渐变过程中对亮度进行动画处理。 |
| Use Current Color | 忽略Color属性，使用前一个键的颜色。 |
