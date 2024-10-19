---
linkTitle: 3D 视口
title: 使用3D 视口
description: 使用 3D 视口与 Open 3D Engine (O3DE)中的实体进行交互。
weight: 400
---

在**Open 3D Engine** (O3DE)中，您可以使用三维视口直接与实体交互。 三维视口提供了多种功能来完成常见任务，如编辑实体的位置、旋转和缩放、实体的层次结构及其组件。 [用户界面部件](#viewport-ui-widgets)、[操纵器](#manipulators)、[快捷键](#hotkeys) 和视觉反馈机制简化了编辑和排列实体的过程。有了[参考空间](reference-spaces)，你可以快速选择实体是在世界空间、本地空间、与父实体的关系还是自定义空间中转换。

![Screenshot of 3D Viewport with UI widgets, entity, and a manipulator visible.](/images/user-guide/editor/3d-viewport.png)

## 视口UI控件

### 变换模式控件

您可以使用变换模式控件从**平移**、**旋转**或**缩放**三种变换模式中选择一种。 每种变换模式都会在 3D 视口中的实体上显示一个独特的 [操纵器](#manipulators)，您可以用鼠标对其进行调整。 用鼠标点击部件或使用下表列出的键盘热键选择变换模式。按下 **Ctrl + 鼠标滚轮上/下**，即可在平移和旋转模式之间快速切换。

![Image of transform mode widget](/images/user-guide/editor/transform-widget.png)

| 模式 | 键盘快捷键 | 说明 |
| - | - | - |
| 平移 | **1** | 沿轴、平面或表面移动实体。|
| 旋转 | **2** | 在三个轴中的一个轴上旋转实体，或相对于摄像机旋转实体。 |
| 缩放 | **3** | 统一缩放实体的大小。 |

### 参考空间控件

参照空间设定了操纵器轴线的对齐方式。默认情况下，实体将在**本地空间**中进行平移和旋转，这是由实体在[Transform组件](/docs/user-guide/components/reference/transform)中的**Rotate**和**Translate**值决定的。所有实体的**世界空间**在任何时候都是不变的。 世界空间始终以**Translate**坐标 `(0, 0, 0)`为中心，不会发生任何旋转。 如果一个实体是另一个实体的子实体，**父空间**将引用父实体变换组件的 **Rotate** 和 **Translate** 值。 您可以**左键单击**部件中的图标来锁定参照空间选择；再次**左键单击**图标可将参照空间返回到默认的本地空间。

![Image of reference space widget](/images/user-guide/editor/reference-space-widget.png)

| 模式 | 键盘快捷键 | 说明 |
| - | - | - |
| 世界空间 | 按住 **Shift** | 操纵者与一个关卡中所有实体共有的不变世界空间保持一致。 |
| 父空间 | | 如果一个实体有一个父实体，操纵器就会与父实体的旋转对齐。 |
| 本地空间 | | 操纵器与所选实体的旋转保持一致。 |

### 组件模式切换器

![Image of component mode switcher widget](/images/user-guide/editor/component-mode-switcher.png)

三维视口中的每个实体都有一个 Transform组件。 您可以添加到实体中的其他组件具有一些属性，这些属性可以在三维视口中使用组件的**编辑模式**轻松编辑。 您可以**左键单击**可编辑组件的图标，进入该组件的编辑模式。 默认情况下，Transform组件的编辑模式被选中。

下表列出了一些具有编辑模式的组件，并介绍了在组件模式切换器中选择该组件时可以完成的任务。 有关编辑模式下的操作说明，请参阅[组件参考](/docs/user-guide/components/reference/)中的组件页面。
1
| 组件 | 编辑模式任务 |
| - | - |
| [Shape](/docs/user-guide/components/reference/shape) | 编辑各种形状组件的尺寸。 |
| [White Box Tool](/docs/user-guide/components/reference/shape/white-box) | 使用白盒工具设计模型 |
| [Spline](/docs/user-guide/components/reference/shape/spline) | 添加、移除和定位Spline控制点。 |
| [Non-uniform Scale](/docs/user-guide/components/reference/non-uniform-scale) | 分别编辑实体在 X、Y 和 Z 轴上的缩放比例。 |
| [PhysX Collider](/docs/user-guide/components/reference/physx/collider) | 编辑碰撞体的尺寸。 |
| [PhysX Ball Joint](/docs/user-guide/components/reference/physx/ball-joint) | 编辑关节的位置和限制。 |

## 操纵器

使用操纵器，您可以直接在三维视口中而不是在[Entity Inspector](../entity-inspector).中编辑实体的Transform组件属性。 选择实体时，只显示当前变换模式的操纵器。 操纵器的方向与参考空间选择相匹配。 

### 平移操纵器

![Image of the translation manipulator with the linear, planar, and surface manipulator highlighted](/images/user-guide/editor/transform-manipulator.png)

平移机械手由三种不同类型的机械手组成。红色、绿色和蓝色箭头是线性操纵器，分别对应 X 轴、Y 轴和 Z 轴。 您可以**左键点击**并拖动线性操纵器，一次沿单个轴移动一个实体。 悬停在操纵器上时，操纵器会变成黄色。

使用**平面操纵器**（以正方形表示）可沿两个轴移动实体，同时限制未选定轴上的移动。

平移操纵器中心的黄色球体是**表面操纵器**。 您可以沿三维视口中可见的其他网格的表面移动实体。 如果没有与实体相交的表面，实体将在摄像机前方移动固定的距离。

![Image of translation manipulator with a ghost axis](/images/user-guide/editor/ghost-axis.png)

平移操纵器会显示一个红色、蓝色和绿色的 “幽灵轴”，指示实体在使用操纵器时的原始位置。

### 旋转操纵器

![Image of the rotation manipulator](/images/user-guide/editor/rotation-manipulator.png)

旋转操纵器由两类操纵器组成。一组红色、绿色和蓝色圆圈是**矩形操纵器**，分别在 X 轴、Y 轴和 Z 轴上旋转实体。 较大的白色圆圈是 **相机空间操纵器**，它可以在编辑器相机的前轴上旋转实体。**左键单击***并拖动操纵器，可在轴上旋转实体。

![Image of the rotation manipulator with a yellow sector that shows how much the entity has been rotated](/images/user-guide/editor/rotation-feedback.png)

旋转机械手在使用时会显示一个黄色扇形，指示实体的原始方向。

### Scale manipulator

![Image of the scale manipulator](/images/user-guide/editor/scale-manipulator.png)

缩放操纵器由三维轴上的一组白色立方体表示。 您可以**左键点击**并拖离操纵器的中心，以增加实体的均匀比例。

## 支点

![Side-by-side images of an entity showing the locations of its default and center pivot](/images/user-guide/editor/pivots.png)

具有三维形状的实体可能有不止一个支点。 在视口中，支点由一个黄色的小立方体表示。 默认的支点位于实体的本地原点，通常是在三维对象用 DCC 工具建模并导入 O3DE 时设置的。 实体的第二个支点位于对象体积的几何中心。 您可以通过按键盘上的**P**键在两个支点之间切换。

实体的支点决定了实体旋转和缩放的固定点。

## 快捷键

有关 3D 视口的键盘热键摘要，请参阅下表。
1
| 默认热键 | 操作 | 注意 |
| - | - | - |
| **1** | 显示平移操纵器。 |  |
| **2** | 显示旋转操纵器。 |  |
| **3** | 显示缩放操纵器。 |  |
| **Ctrl + 鼠标滚轮向上/向下滚动** | 在平移和旋转模式操纵器之间切换。 |  |
| **R** | 重置实体的变换。 | 仅重置当前变换模式的变换值。 |
| **Ctrl + 拖拽** 平移操纵器 | 将所有操纵器设置为自定义平移。| 为旋转和缩放设置自定义支点。|
| **Ctrl + 拖拽** 旋转操纵器 | 将所有操纵器设置为自定义旋转。| 为平移轴设置自定义旋转。 |
| **Ctrl + 拖拽** 缩放操纵器 | 设置所有操纵器的大小。 |  |
| **Ctrl + R** | 重置当前操纵器。 |  |
| **P** | 切换选中实体的支点。 |  |
