---
linkTitle: UI Transform2D
description: Transform2D 组件定义元素的位置、旋转、大小和缩放。
title: UI Transform2D Component
weight: 200
---

**Transform2D** 组件会自动添加到您创建的每个 UI 元素中。此组件定义元素相对于其父级边缘的位置、旋转、大小和缩放。父级可以是另一个元素（如果元素嵌套）或画布。

## 管理 UI 锚点和偏移量

您可以使用 **Transform2D** 组件中的锚点和偏移设置来设置 UI 元素相对于其父级边缘的位置和大小。**Transform2D** 组件是每个元素中的必需组件。

锚点值始终为`0.00%` 到 `100.00%`，由父级的边缘定义。偏移量以像素表示，并且是相对于锚点的。

锚点和偏移量在各种情况下都很有用：
+ 确保元素在其父元素的边缘内保持特定的填充，而不管元素的大小如何变化
+ 将元素锚定到其父元素的一角，而不管父元素的大小或位置如何变化
+ 构建与分辨率无关的 UI 元素

例如，您可以确保元素保持全屏状态，而不管屏幕的分辨率如何。

### 配置元素的锚点

1. 在 [**UI 编辑器**](/docs/user-guide/interactivity/user-interface/editor) 的 **Hierarchy** 窗格中，选择要修改其锚点的元素。

1. 在 **Properties** 窗格中的 **Transform2D** 下，从常用锚点放置选项中进行选择。

   1. 沿边锚定到父对象的中心、角或中间位置，而不更改大小。

   1. 锚定到左边缘、中间或右边缘;垂直大小会根据父级进行调整。

   1. 锚定到上边缘、中间或下边缘;水平大小调整为父级。

   1. 将元素的所有边缘锚定到父级。水平和垂直大小会根据父级进行调整。您可以使用此锚点预设来放置保持全屏的元素，而不管分辨率如何变化。如果画布是其父级，则适用。

   ![Transform 2D Anchor Settings](/images/user-guide/interactivity/user-interface/components/transform/ui-editor-presets-1.png)

**进一步编辑（微调）元素的锚点**
在 **Properties** 窗格中的 **Transform2D** 下，根据需要对 **Anchors** 执行以下操作：
+ 对于 **左 **，输入一个介于 '`0.00%`' 和 '`100.00%`' 之间的值。
+ 对于 **Right**，输入一个介于 '`0.00%`' 和 '`100.00%`' 之间的值。
+ 对于 **Top**，输入一个介于 '`0.00%`' 和 '`100.00%`' 之间的值。
+ 对于 **底部**，输入一个介于 `'0.00%`' 和 '`100.00%`' 之间的值。

锚点的位置可以可视化为网格上的点，按其父级边缘的长度从左到右、从上到下以百分比绘制。如果你想保持元素的绝对大小（这样当父元素改变大小时它不会改变大小），但想把它锚定在相对于父元素大小的特定垂直或水平点上，请确保顶部和底部（或左和右）锚点具有相同的数字。在这种情况下，锚点被称为在一起。

但是，例如，如果你希望元素的左边缘和右边缘分别保持相对于其父级的固定百分比，并随着其父级的大小变化而变化，那么就使数字不同。在这种情况下，锚点称为 split。

![Visual aid for setting Anchor values](/images/user-guide/interactivity/user-interface/components/transform/ui-editor-percent.png)

### 编辑元素的位置和大小

在 **Properties** 窗格中的 **Transform2D** 下，根据需要修改 **Offsets**：
如果元素的锚点在一起，请执行以下操作：
+ 对于 **X Pos**，输入负值或正值（以像素为单位）。这将调整相对于左右锚点位置的水平偏移。
+ 对于 **Y Pos**，输入负值或正值（以像素为单位）。这将调整相对于上下锚点位置的垂直偏移。
当元素的锚点在一起时，只有其位置会随父元素的大小而调整。元素的大小不会调整。因此，您可以手动调整元素的大小，当锚点在一起时，该大小将保持一致。
+ 对于 **Width**，输入一个以像素为单位的值。
+ 对于 **Height**，输入一个以像素为单位的值。
如果元素的锚点被拆分，请执行以下操作：
+ 对于 **Left**，输入负值或正值（以像素为单位）。这将调整相对于元素左锚点的大小偏移量。
+ 对于 **Right**，输入负值或正值（以像素为单位）。这将调整相对于元素右锚点的大小偏移量。
+ 对于 **Top**，输入负值或正值（以像素为单位）。这将调整相对于元素顶部锚点的大小偏移量。
+ 对于 **Bottom**，输入负值或正值（以像素为单位）。这将调整相对于元素底部锚点的大小偏移量。

### 编辑元素的枢轴、旋转和缩放

在 **Properties** 窗格中，在 **Transform2D**下， 根据需要对 **Pivot**、**Rotation** 和 **Scale** 执行以下操作：
+ 对于 **Pivot**，选择枢轴预设或输入 X 和 Y 的值，其中“`0`”和“`1`”表示元素的边缘。
+ 对于 **Rotation**，请输入一个以度为单位的值。
+ 对于 **X Scale**，输入一个值以用作元素宽度的乘数。
+ 对于 **Y Scale**，输入一个值以用作元素高度的乘数。
+ 如果您希望 UI 元素及其子元素随设备分辨率缩放，请选择 **Scale to Device**。

{{< note >}}
元素围绕其枢轴点旋转、调整大小并从其枢轴点计算位置。枢轴点不受元素边界的限制。您可以将枢轴放置在元素的外部。
{{< /note >}}

**示例：使用锚点相对于其父元素调整元素的大小**
在下面的示例中，锚点用于调整元素相对于其父元素的大小。按钮的布局列会根据需要调整大小，以保持在屏幕上。由于按钮的布局列不使用 **Scale to Device** 设置，因此按钮文本不会随其父按钮一起更改大小。

{{< note >}}
您可以单独 [配置文本元素](/docs/user-guide/interactivity/user-interface/components/visual/components-text) 。
{{< /note >}}

按钮的布局列具有以下设置。

**布局列元素设置**

| 属性 | 值 |
| --- | --- |
| Anchors | 左 = 20%, 顶 = 6%, 右 = 80%, 底 = 94% |
| Pivot | 默认设置: X = 0.5, Y = 0.5 |
| Scale to device | None (未选择) |

![Using anchors to resize buttons to stay on the screen.](/images/user-guide/interactivity/user-interface/components/transform/ui-editor-transform-scale-3.gif)

## Scale to Device

**Scale to Device** 属性有助于构建可在多种屏幕分辨率上显示的游戏 UI。您可以在 UI Editor 的 **Preview Mode** 中以不同的分辨率预览画布。

设备缩放是使用创作的画布大小与运行时画布大小的比率来计算的。然后，根据所选的 **Scale to Device** 设置调整设备比例。当您选择除 **None** 之外的任何 **Scale to Device** 设置时，设备缩放将乘以 **Transform2D** 组件的 **Scale** 属性，以获得元素的最终局部缩放。

以下 **Scale to Device** 设置可用：

| 属性 | 说明 |
| --- | --- |
| None | 不随设备分辨率缩放。 |
| Scale to fit (uniformly) | 在保持纵横比的同时进行缩放以适应。X 和 Y 的最终设备比例是创作的画布大小与视区大小之间的宽度和高度比率的最小值。 |
| Scale to fill (uniformly) | 缩放以填充，同时保持纵横比。X 和 Y 的最终设备比例是创作的画布大小与视区大小之间的宽度和高度比率的最大值。 |
| Scale to fit X (uniformly) | 缩放以适合水平方向，同时保持长宽比。X 和 Y 的最终设备比例是创作的画布宽度与视区宽度之间的比率。 |
| Scale to fit Y (uniformly) | 缩放以适合垂直方向，同时保持纵横比。X 和 Y 的最终设备比例是创作的画布高度与视区高度之间的比率。 |
| Stretch to fill (non-uniformly) | 拉伸以水平和垂直填充，而不保持长宽比。最终的设备比例是创作的画布大小与视区大小之间的比率。 |
| Stretch to fit X (non-uniformly) | 可以水平拉伸以适应大小，但不可以垂直拉伸。X 的最终设备比例是创作的画布宽度与视区宽度之间的比率。Y 不随设备分辨率缩放。 |
| Stretch to fit Y (non-uniformly) | 可垂直拉伸，但不可水平拉伸。Y 的最终设备比例是创作的画布高度与视区高度之间的比率。X 不随设备分辨率缩放。|

使用 **Scale to Device** 设置时，请注意以下事项：
+ 围绕元素的枢轴执行缩放。
+ 缩放元素不会影响其与锚点的偏移值。
+ 元素的最终缩放包括从其父级继承的任何缩放。在某个 UI 元素上设置 **Scale to Device** 属性，您也希望该元素的子元素随设备分辨率进行缩放。
+ 在 UI 元素和后代元素上设置 **Scale to Device** 会导致后代元素上出现双重缩放。
+ 避免在没有锚点在一起的 UI 元素上设置 **Scale to Device**。这样做可能会导致意外行为。这是因为锚点会影响元素相对于其父元素的大小，并且 **Scale to Device** 缩放在此之上应用。

**示例**

如果将锚点设置为以下值，则元素的大小将与视区的大小匹配：
  + Left = 0%
  + Top = 0%
  + Right = 100%
  + Bottom = 100%

但是，如果您随后在这些锚点值的基础上添加缩放，则元素大小将不再与视区大小匹配。

### Scale to Device 示例

以下每个示例都演示了不同的 **Scale to Device** 设置。

在每个示例中，背景图像覆盖整个屏幕并使用以下设置：
+ Anchors (apart): Left = 0%, Top = 0%, Right = 100%, Bottom = 100%
+ Image type: Tiled
+ Scale to Device: None

### Uniform Scaling 

在此统一缩放示例中，UI 父元素具有固定的纵横比，并且居中并适合显示它的屏幕。

背景图像是具有简单 [settings （设置） 的纹理](#uniform-scaling).

构成 UI 的元素都包含在父元素中，并具有以下设置。

**父级 UI 元素设置**

| 属性 | 说明 |
| --- | --- |
| Anchor | Left = 50%, Top = 50%, Right = 50%, Bottom = 50% |
| Pivot | 默认设置: X = 0.5, Y = 0.5 |
| Width and Height | 匹配创作的画布大小（例如, 1280x720) |
| Scale to device | 缩放以适应 （均匀）|

![Scale to fit uniformly example.](/images/user-guide/interactivity/user-interface/components/transform/ui-editor-transform-scale-1.gif)

### Scale to Fit Y 

在这个统一缩放以适应 Y 示例中，布局列及其按钮被统一缩放，以便它们垂直适合屏幕，而不管其分辨率如何。

背景图像是具有简单 [settings](#uniform-scaling) 的纹理.

组成按钮的元素包含在 layout column 元素中。布局列元素包含 UI 按钮，并具有以下设置。

**用于均匀缩放以适应 Y 的布局列元素设置**

| 属性 | 说明 |
| --- | --- |
| Anchors | Left = 50%, Top = 50%, Right = 50%, Bottom = 50% |
| Pivot | 默认设置: X = 0.5, Y = 0.5 |
| Scale to device | 缩放以适合 Y（均匀） |

![Scale to fit Y uniformly example.](/images/user-guide/interactivity/user-interface/components/transform/ui-editor-transform-scale-2.gif)

### 在保持相对位置的同时均匀缩放

在此示例中，**Scale to Device** 设置根据屏幕分辨率缩放运行状况条和速度指示器。锚点设置会保持其位置，以便运行状况条始终显示在右上角，而速度指示器始终显示在中心。

背景图像是具有简单 [settings （设置） 的纹理](#uniform-scaling).

Health Bar 元素具有以下设置。锚点值将其保持在屏幕的右上角。

**生命条元素设置**

| 属性 | 说明 |
| --- | --- |
| Anchors | Left = 100%, Top = 0%, Right = 100%, Bottom = 0% |
| Pivot | X = 1.0, Y = 0.0 |
| Scale to device | 缩放以适应 （均匀）|

速度指示器元素具有以下设置。锚点值将其保持在屏幕的顶部中心。

**生命条元素设置**

| 属性 | 说明 |
| --- | --- |
| Anchors | Left = 50%, Top = 0%, Right = 50%, Bottom = 0% |
| Pivot | X = 0.5, Y = 0.0 |
| Scale to device | 缩放以适应 （均匀） |

下图显示了生命条和速度指示器如何根据屏幕分辨率进行缩放，同时保持它们在屏幕上的位置。

{{< note >}}
指示的分辨率不会按比例显示。
{{< /note >}}

![Example of scaling elements while maintaining relative positions.](/images/user-guide/interactivity/user-interface/components/transform/ui-editor-transform-scale-uniform-position.png)
