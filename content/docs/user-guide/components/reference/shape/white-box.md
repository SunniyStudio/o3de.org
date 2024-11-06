---
linkTitle: White Box
description: ' 使用白盒组件在 Open 3D Engine （O3DE） 中快速绘制实体和关卡的几何图形。 '
title: 'White Box 组件'
---

**White Box** 组件是一个工具，可用于在 **Open 3D Engine （O3DE） Editor** 中绘制 3D 代理网格。将 White Box （白盒） 组件添加到实体中，选择原始形状以用作代理网格的基础，然后进入编辑模式以访问工具以快速粗略地绘制出实体的网格。

![White Box component interface.](/images/user-guide/components/reference/shape/white-box-A.gif)

由于 White Box 是作为组件实现的，因此您可以在 O3DE Editor 中创建定义明确的实体，这些实体可以准确表示最终生产实体的大小、形状和功能，然后再投入时间和精力为您的实体构建成品模型。使用白盒创建的网格可以作为白盒网格资产 \(`.wbm`\) 保存到磁盘，并在其他白盒组件中重复使用。白盒网格还可以导出为“.obj”文件，并用作第三方 3D 建模应用程序中的模板，以构建最终的生产资产。

## 提供者

[White Box Gem](/docs/user-guide/gems/reference/design/white-box)

## White Box 属性 

{{< tabs name="white-box-component-ui" >}}
{{% tab name="Primitive Shapes" %}}

![Primitive Shapes](/images/user-guide/components/reference/shape/white-box-component-ui-01.png)

{{% /tab %}}
{{% tab name="Mesh Asset" %}}

![Mesh Asset](/images/user-guide/components/reference/shape/white-box-component-ui-02.png)

{{% /tab %}}
{{< /tabs >}}

| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **Save as asset** | 选择此按钮可将代理网格保存到白色盒体网格\(`.wbm`\)资产中。你可以在其他 White Box 组件中加载保存的\(`.wbm`\) 资源。'`.wbm`' 文件的功能类似于实例，对网格所做的任何更改都会传播到所有使用 '`.wbm`' 文件的 White Box 组件。 |||
| **Export** | 选择 **Export** 按钮将网格导出到 '`.obj`' 文件。“`.obj`”文件可以加载到 3D 建模应用程序中，并用作为实体创建生产网格资产的模板。 |||
| **Default Shape** | 白色盒体网格的默认基元形状。从列表中选择一个原始形状，或者选择加载保存的白盒网格\(`.wbm`\)资产。默认基本体大小在世界空间中为 1 米。 | Cube, Tetrahedron, Icosahedron, Cylinder, Sphere, Custom Mesh Asset | `Cube` |
| **Mesh Asset** | 请参阅下方的 [Mesh Asset 属性](#mesh-asset-properties)。 |||
| **Tint** | 为白色盒体网格设置色调颜色。选择色板以打开拾色器，或在字段中输入以逗号分隔的红色、蓝色和绿色 8 位值，以设置“白框”组件的色调颜色。 | 每通道 8 位颜色： 0-255 | `255,255,255` |
| **Use Texture** | 启用 **Use Texture** 以在白色框网格上显示棋盘纹理。每个正方形的大小为半米，纹理投影在网格的局部 X、Y 和 Z 轴上。这样可以维护代理网格大小的简单参考，而不管实体在关卡中的方向如何。 | Boolean | `True` |
| **Visible** | 启用 **Visible** 以使白色盒体网格在运行时可见。当您使用 White Box 创建自定义不可见的碰撞网格时，请禁用 **Visible** 属性以在运行时隐藏网格。 | Boolean | `True` |
| **Edit** | C选择 **Edit** 按钮进入 Edit 模式。在编辑模式下，您可以使用下面的 [编辑模式操作](#edit-mode-actions) 中概述的方法在视区中修改白盒网格。在 Edit 模式下，菜单栏中的 Edit （编辑） 菜单显示可用的操作和热键。要退出 Edit 模式，请在组件界面中选择 **Done**。 |  |  |

### Mesh Asset 属性
Mesh Asset （网格资源） 属性仅适用于 Mesh Asset **Default Shape（默认形状）** 类型。

| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **Mesh Asset** | 一个预先存在的白盒网格\(`.wbm`\)资产，用于默认的白盒基元形状。 | 网格资源的路径。  | None |

![White Box .wbm mesh instancing.](/images/user-guide/components/reference/shape/white-box-mesh-instancing.gif)

## White Box 编辑模式

在编辑模式下，您可以通过选择并拖动白框网格的面、边和顶点组件，在 O3DE 编辑器中快速绘制实体的网格。首先，将 White Box 组件添加到实体，在 White Box 组件界面中选择一个默认的原始形状，然后选择 **Edit** 进入编辑模式。

#### 移动多边形

1. 将鼠标悬停在多边形上。

1. 按住 **鼠标左键**。

1. 沿多边形的法线拖动多边形。

    ![White Box move face.](/images/user-guide/components/reference/shape/white-box-move-face.gif)

### 移动边

1. 将鼠标悬停在边上。

1. 按住 **鼠标左键**。

1. 拖动边。

    ![White Box move edge.](/images/user-guide/components/reference/shape/white-box-move-edge.gif)

### 和移动顶点

1. 将鼠标悬停在顶点上。顶点将高亮显示。

1. 按住 **鼠标左键**。

1. 沿边参考线拖动顶点。

    ![White Box move vertex.](/images/user-guide/components/reference/shape/white-box-move-vertex.gif)

### 缩放多边形

1. 选择一个多边形。

1. 将鼠标悬停在多边形的一个顶点上。

1. 按住 **鼠标左键**。

1. 将顶点拖向或拖离所选多边形的中心。

    ![White Box scale face.](/images/user-guide/components/reference/shape/white-box-scale-face.gif)

### 缩放边缘

1. 选择一条边。

1. 将鼠标悬停在边的其中一个顶点上。

1. 按住 **鼠标左键**。

1. 沿选定边的长度拖动顶点。

    ![White Box scale edge.](/images/user-guide/components/reference/shape/white-box-scale-edge.gif)

### 非均匀缩放边缘

1. 选择一条边。

1. 将鼠标悬停在边的其中一个顶点上。

1. 按住 **Alt** 并沿所选边的长度拖动顶点。

    ![White Box scale edge.](/images/user-guide/components/reference/shape/white-box-non-uniform-scale-edge.gif)

### 挤出多边形

1. 将鼠标悬停在多边形上。

1. 按住 Ctrl + 鼠标左键。

1. 沿多边形的法线拖动多边形。

    ![White Box extrude face.](/images/user-guide/components/reference/shape/white-box-extrude-face.gif)

### 挤出边

1. 将鼠标悬停在边上。

1. 按住 Ctrl + 鼠标左键。

1. 拖动边。

    ![White Box extrude edge.](/images/user-guide/components/reference/shape/white-box-extrude-edge.gif)

### 挤出比例

1. 选择一个多边形。

1. 将鼠标悬停在多边形的一个顶点上。

1. 按住 Ctrl + 鼠标左键。

1. 将顶点拖向或拖离要缩放的选定多边形的中心。

1. 将鼠标悬停在所选多边形上。

1. 按住 **鼠标左键**。

1. 沿多边形的法线拖动多边形。

    ![White Box extrude scale.](/images/user-guide/components/reference/shape/white-box-extrude-scale.gif)

### 翻转边缘

1. 按住 **Ctrl + Shift** 可显示隐藏的边缘。

1. 在隐藏的边缘上 **右键单击** 以翻转其方向。

    ![White Box flip edge.](/images/user-guide/components/reference/shape/white-box-flip-edge.gif)

### 隐藏边

1. 选择一条边。

1. 按 **H**。

    ![White Box hide edge.](/images/user-guide/components/reference/shape/white-box-hide-edge.gif)

### 显示边

1. 按住 **Ctrl + Shift** 显示边缘。

1. 选择一条边以取消隐藏它。

    ![White Box extrude edge.](/images/user-guide/components/reference/shape/white-box-show-edge.gif)

### 隐藏顶点

1. 选择一个顶点。

1. 按 **H**。

    ![White Box hide edge.](/images/user-guide/components/reference/shape/white-box-hide-vertex.gif)

### 显示顶点

1. 按住 **Ctrl + Shift** 以显示隐藏的顶点。

1. 选择一个顶点以取消隐藏它。
