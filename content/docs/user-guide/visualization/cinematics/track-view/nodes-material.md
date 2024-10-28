---
description: ' 在 O3DE 编辑器的<guilabel>Track View</guilabel>编辑器中使用材质节点，将常用的材质属性制成动画。 '
title: Material 节点
---

**Material**节点可帮助您将通常在**材质编辑器**中设置的常用材质属性制作成动画。您可以通过序列或**Director**节点添加**Material**节点。

{{< note >}}
**Material**节点的名称必须是要制作动画的材质的完整路径，如**材质编辑器**所示。
{{< /note >}}

推荐的工作流程是在**材质编辑器**中选择要制作动画的材质。

**在轨迹视图中添加材质节点**

1. 在 O3DE 编辑器中，选择**Tools**，**Material Editor**。

1. 导航到要制作动画的材质。

1. 右键单击选中的材质，然后选择**Copy Path to Clipboard**。如果材质在材质组中，请选择该组并将组名复制到剪贴板。

    ![Copy the material name path in the Material Editor.](/images/user-guide/cinematics/cinematics-track-view-nodes-material-4.png)

1. 选择**Tools**、**Track View**。

1. 创建或选择要包含动画的现有序列或 **Director** 节点。

1. 在节点树中，右键单击并选择**Add Material Node**，然后在**Material Name**对话框中，按**Ctrl+V**粘贴在步骤 3 中复制的材质的完整路径，然后单击**OK**。

1. 如果该材质在材质组中，则文字会显示在轨迹视图的**Material**节点上，显示为红色。这意味着尚未选择子材质。

   要添加子材质，请执行以下操作：

   1. 在轨迹视图中，右键单击**Material**节点。

   1. 选择要制作动画的子材质。

   1. 材**Material** 节点文本应不再显示为红色。

1. 要为**Material**节点添加轨道，请右键单击节点名称并选择**Add Track**。有关可用轨道，请参见下表。


**Material 节点轨道**
1
| 轨道 | 关键属性 | 说明 |
| --- | --- | --- |
| Diffuse  | Color (RGB) |  RGB 值，用于指定材质的基色。  |
| Emissive Color  | Color (RGB) |  RGB 值可使物体发光并在黑暗中可见。  |
| Emissive Intensity | Float (0.00 to 1.0) |  浮点数值，用于控制模拟物体表面发光的亮度。  |
| Emittance Map Gamma | Float (1.0 to 2.0) |  浮点数值，用于扩展幅射图的下限范围。这会使较暗的颜色显得不那么明亮。  |
| Glossiness  | Float (0 to 255) | 镜面反射的清晰度或锐利度。`10`或更小的值会产生散射反射。大于 `10`的值会产生锐利的反射。  |
| Indirect Color | Color (RBG) |  RGB 值，用于指定全局照明反弹光线的色调。  |
| Opacity  | Float (0.00 to 1.0) | 透明度。小于 `50`的值更偏向阿尔法通道图的白色端。大于 `50`的值更偏向阿尔法通道图的黑色一端。 |
| SSSIndex  | Float (0.00 to 3.99) |  控制次表层散射轮廓和数量。 对于大理石，请指定一个介于 `0.01` 到 `0.99` 之间的值。 对于皮肤，请在`1.00` 到 `1.99`之间指定一个值。  |
| Specular  | Color (RGB) |  当光线照射在物体上时，材质的反射亮度和颜色。数值越大，材质越闪亮。 要应用黑白反射，请为 R、G 和 B 指定相同的值。  |
