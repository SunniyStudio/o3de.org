---
linkTitle: Paint Brush
title: Paint Brush
description: ' Paint Brush 是一种通用工具，用于使用标准绘制控件在世界空间中处理任意数据。 '
---

在 O3DE 中，许多不同的组件将数据附加到世界空间位置和区域。您可以在内容创建工具中从外部编辑此数据，但有时直接在 O3DE 内部的组装世界上下文中编辑数据会更容易。

Paint Brush是某些类型的组件上可用的一种编辑模式，用于启用标准绘制控件，以便在世界空间中编辑组件的数据。

某些组件将像素从源图像直接映射到世界空间，但其他组件可能具有更间接的映射，例如存储在网格上顶点上的权重。Paint Brush （绘制笔刷） 旨在允许在世界空间中为任意类型的数据进行绘制。因此，下面的描述更通用地指值而不是像素，即使通常将绘制视为基于图像的操作。此外，数据值到世界空间的映射可以具有不同的密度，因此下面的描述区分了 **世界空间** 和 **值空间**。世界空间表示虚拟世界中的米数。值空间表示正在编辑的数据源中相邻数据值的数量。

## 使用Paint Brush

通过单击组件上的 **Edit** 按钮或在视区的 **Component Switcher** 中选择组件的图标来启用 Paint Brush。

激活 Paint Brush 后，有三组控件：

![Paint Brush Tool image.](/images/user-guide/components/reference/paintbrush/paintbrush-tool.png)

1. 一个可停靠的 **Paint Brush Settings** 窗口
2. 视口中的Paint Brush模式选择图标
3. 在视口中循环切换Paint Brush

绘制包括在视口中移动鼠标，同时按住鼠标左键。按下鼠标左键后，画笔笔触开始，释放鼠标左键后结束。撤消/重做命令适用于每个画笔笔触。

画笔描边由一系列重叠的圆圈组成，这些圆圈在鼠标移动时以恒定的间距应用。Paint Brush Settings 控制圆圈的颜色、不透明度、大小和间距。

![Example brush stroke.](/images/user-guide/components/reference/paintbrush/paintbrush-brush-stroke.png)

通过按 **Esc**、单击组件上的 **Done** 按钮或在视区的 **Component Switcher** 中选择其他组件的图标来结束 Paint Brush 模式。

## Paint Brush 模式

Paint Brush 具有三种模式 - **Paint**, **Eyedropper**, 和 **Smooth**。

| 模式 | 说明 |
| - | - |
| Paint | Paint Brush （绘制画笔） 根据 Paint Brush Settings （绘制画笔设置） 将新值混合到正在绘制的任何数据上。 |
| Eyedropper | Paint Brush 从画笔的确切中心下方读取当前值，并将 Paint Brush Settings 中的 **Intensity**、**Color** 和 **Opacity** 设置为该值。吸管始终使用精确的中心来读取单个值;画笔大小不会影响它。 |
| Smooth | Paint Brush （绘制画笔） 平滑现有数据，并根据 Paint Brush Settings （绘制画笔设置） 将结果混合回来。 |

## Paint Brush Settings

有三种类型的 Paint Brush Settings - 影响画笔笔触的设置、影响画笔笔触中各个圆圈的设置以及仅适用于平滑操作的设置。

### 影响画笔描边的设置

#### Color / Intensity

**Paint Brush Settings**为支持全色值的组件提供 **Color** 设置，或为仅支持单通道灰度值的组件提供 **Intensity** 设置。

**Color** 控制整个画笔描边的颜色和不透明度。**Intensity** 控制整个画笔描边的灰度值。

#### Opacity

**Opacity** 控制整个画笔描边的不透明度。从概念上讲，如果将整个画笔描边绘制到单独的绘画图层中，则 **Opacity** 是用于将该图层混合回主图层的 Alpha 值。如果画笔笔触与自身交叉，则不会进行双重混合。

如果组件支持全色值，则可以将不透明度作为颜色的一部分进行编辑，也可以通过 **Opacity** 滑块进行编辑。为方便起见，在两个位置都公开了相同的值。

画笔笔触与中等不透明度交叉的示例：
![Example of brush stroke crossing itself with medium opacity.](/images/user-guide/components/reference/paintbrush/paintbrush-opacity-overlap.png)

#### Blend Mode

**Blend Mode** 控制如何将整个画笔描边混合回主图层。在下表中，`a` 是主图层值，`b`是绘制的值，`c`是使用基于 **Opacity** 设置的线性插值混合回主图层的值。

| 混合模式 | 数学函数 | 说明 |
| - | - | - |
| Normal | $$c = b$$ | 直接使用绘制值进行混合。 |
| Multiply | $$c = ab$$ | 在混合之前将 main 值和 painted 值相乘，这将产生较暗的值。 |
| Screen | $$c = 1 - (1 - a)(1 - b)$$ | 在混合之前，在 main 值和 painted 值之间执行反转乘法，从而产生较轻的值。 |
| Linear Dodge (Add) | $$c = a + b$$ | 在混合之前添加 main 值和 painted 值，这将产生较轻的值。 |
| Subtract | $$c = a - b$$ | 在混合之前减去 main 值和 Painted 值，这会导致值更暗。 |
| Darken (Min) | $$c = min(a, b)$$ | 在混合之前保持 main 值和 painted 值之间的最小值。这会导致值始终与主值相同或更暗。 |
| Lighten (Max) | $$c = max(a, b)$$ | 在混合之前保持 main 和 painted 值之间的最大值。这会导致值始终等于或小于主值。 |
| Average | $$c = (a + b) / 2$$ | 使用 main 和 painted 值的平均值进行混合。 |
| Overlay | $$a < 1/2 : c = 2ab$$<br>$$a >= 1/2 : c = 1 - 2(1 - a)(1 - b)$$ | 如果主值为深色，则应用正片叠底模式并变暗;如果 Main 值为 Light，则应用 Screen mode 和 lighten。 |

### 影响画笔圆圈的设置

#### Size

**Size** 控制每个画笔圆的直径。大小在世界空间坐标中以米为单位指定，而不是以像素或组件中受影响的值的数量指定。

#### Flow

**Flow**控制每个画笔圆圈的最大不透明度。**Flow** 值为 0% 是完全透明的，而 100% 是完全不透明的。

| Flow | Illustration |
| - | - |
| 1% | ![Paintbrush flow of 1%.](/images/user-guide/components/reference/paintbrush/paintbrush-flow-1.png) |
| 100% | ![Paintbrush flow of 100%.](/images/user-guide/components/reference/paintbrush/paintbrush-flow-100.png) |

由于 **Flow** 不透明度会影响每个画笔圆圈，因此同一画笔描边中重叠的圆圈将相互混合。

画笔笔触与中等流向交叉的示例：
![Example of brush stroke crossing itself with medium flow.](/images/user-guide/components/reference/paintbrush/paintbrush-flow-overlap.png)

#### Hardness

**Hardness** 控制每个画笔圆圈的不透明度衰减。**Hardness** 是开始衰减时沿圆半径的距离百分比。衰减始终在外半径处结束。例如，**Hardness** 值为 50% 意味着画笔圆圈的内侧 50% 将使用 **Flow** 指定的最大不透明度，而画笔圆圈的外侧 50% 的不透明度将下降，直到它完全透明。值为 100% 将使整个画笔圆圈使用最大不透明度。

硬度 50% 的插图 - 内圈显示衰减开始的位置，外圈显示衰减结束的位置。外圆是画笔大小：![Illustration of hardness opacity falloff.](/images/user-guide/components/reference/paintbrush/paintbrush-falloff-50-percent.png)

下图显示了整体圆圈的不透明度如何通过 **Hardness** 和 **Flow** 的不同组合而变化。尽管圆圈看起来大小不同，但它们都是相同的画笔大小，但中心和外边缘之间的不透明度不同。较低的 **Flow** 值具有较低的最大不透明度，这会导致衰减更快地达到 0。

| | Flow 1% | Flow 50% | Flow 100% |
| - | - | - | - |
| Hardness 0% | ![0% Hardness 1% Flow.](/images/user-guide/components/reference/paintbrush/paintbrush-hardness-0-flow-1.png) | ![0% Hardness 50% Flow.](/images/user-guide/components/reference/paintbrush/paintbrush-hardness-0-flow-50.png) | ![0% Hardness 100% Flow.](/images/user-guide/components/reference/paintbrush/paintbrush-hardness-0-flow-100.png) |
| Hardness 50% | ![50% Hardness 1% Flow.](/images/user-guide/components/reference/paintbrush/paintbrush-hardness-50-flow-1.png) | ![50% Hardness 50% Flow.](/images/user-guide/components/reference/paintbrush/paintbrush-hardness-50-flow-50.png) | ![50% Hardness 100% Flow.](/images/user-guide/components/reference/paintbrush/paintbrush-hardness-50-flow-100.png) |
| Hardness 100% | ![100% Hardness 1% Flow.](/images/user-guide/components/reference/paintbrush/paintbrush-hardness-100-flow-1.png) | ![100% Hardness 50% Flow.](/images/user-guide/components/reference/paintbrush/paintbrush-hardness-100-flow-50.png) | ![100% Hardness 100% Flow.](/images/user-guide/components/reference/paintbrush/paintbrush-hardness-100-flow-100.png) |

由于 **Hardness** 不透明度会影响每个画笔圆圈，因此同一画笔笔触中的重叠圆圈将相互混合。

画笔笔触以中等硬度交叉自身的示例：![Example of brush stroke crossing itself with medium hardness.](/images/user-guide/components/reference/paintbrush/paintbrush-hardness-overlap.png)

#### Distance

**Distance** 控制鼠标移动时画笔描边内每个圆的间距。具体来说，**Distance**值表示鼠标在应用另一个圆圈之前需要移动的总距离，因此使用鼠标来回小幅移动可以产生比**Distance**值更近的圆圈。**Distance** 是圆大小的百分比，因此值 50% 将使每个圆重叠 50%，值 100% 将产生完全不重叠的圆。大于 100% 的值将在每个圆圈之间留出空间。

| Distance | Illustration |
| - | - |
| 1% | ![Paintbrush distance of 1%.](/images/user-guide/components/reference/paintbrush/paintbrush-distance-1.png) |
| 25% | ![Paintbrush distance of 25%.](/images/user-guide/components/reference/paintbrush/paintbrush-distance-25.png) |
| 50% | ![Paintbrush distance of 50%.](/images/user-guide/components/reference/paintbrush/paintbrush-distance-50.png) |
| 100% | ![Paintbrush distance of 100%.](/images/user-guide/components/reference/paintbrush/paintbrush-distance-100.png) |
| 200% | ![Paintbrush distance of 200%.](/images/user-guide/components/reference/paintbrush/paintbrush-distance-200.png) |

### 应用于平滑操作的设置

以下设置仅适用于平滑画笔。

#### Smooth Mode

**Smooth Mode** 是将一组 NxN 值组合成一个平滑值，以便在 NxN 正方形的中心使用的方法。

| 模式 | 说明 | 示例 1 | 示例 2 |
| - | - | - | - |
|  | (Input images) |![Sample smoothing input image.](/images/user-guide/components/reference/paintbrush/paintbrush-smooth-mode-input.png) |![Sample smoothing input image 2.](/images/user-guide/components/reference/paintbrush/paintbrush-smooth-mode-input2.png) |
| `Gaussian` | 使用高斯钟形曲线分布函数取 NxN 值的加权平均值。这是绘制程序最常用的平滑方法。它在平滑噪点和保留细节之间提供了良好的平衡。 |![Sample smoothing Gaussian image.](/images/user-guide/components/reference/paintbrush/paintbrush-smooth-mode-gaussian.png) |![Sample smoothing Gaussian image 2.](/images/user-guide/components/reference/paintbrush/paintbrush-smooth-mode-gaussian2.png) |
| `Mean` | 取 NxN 值的未加权平均值。此方法可消除杂色，并且是最不可能保留清晰细节的方法。 |![Sample smoothing mean image.](/images/user-guide/components/reference/paintbrush/paintbrush-smooth-mode-mean.png) |![Sample smoothing mean image 2.](/images/user-guide/components/reference/paintbrush/paintbrush-smooth-mode-mean2.png) |
| `Median` | 从排序的 NxN 值集中获取中间值。与其他平滑方法相比，此方法保留更锐利的边缘，并且可以完全消除杂色，但它通常也会产生不太平滑的结果，并且可能会引入明显的伪影。 |![Sample smoothing median image.](/images/user-guide/components/reference/paintbrush/paintbrush-smooth-mode-median.png) |![Sample smoothing median image 2.](/images/user-guide/components/reference/paintbrush/paintbrush-smooth-mode-median2.png) |

#### Smoothing Radius

**Smoothing Radius** 是在计算平滑值时，从中心值开始在每个方向上使用的值数。平滑方法使用 NxN 值，其中`N = (radius * 2) + 1`。半径 1 是一组 3x3 的值，半径 2 是一组 5x5 的值，半径 3 是一组 7x7 的值，依此类推。

这个数字以 **value space** 表示。换句话说，它描述在每个方向上使用的相邻值的数量，而不管这些值是如何映射到世界空间的。

较大的半径通常会在较远的距离上产生更平滑的结果，但也会增加性能成本。

**Smoothing Radius**与Paint Brush**Size**是分开的。画笔 **Size** 控制将平滑的值数量，而 **Smoothing Radius** 控制用于平滑每个值的相邻值的数量。只有 Paint Brush （绘制画笔） 中的值才会被平滑，但 Paint Brush （绘制画笔） 之外的值可能用于计算平滑的结果。

## 快捷键和CVars

| 快捷键 | 说明 |
| - | - |
| **[** | 减小画笔 **Size**。 |
| **]** | 增加画笔 **Size**。 |
| **{** | 降低画笔 **Hardness**。 |
| **}** | 增加画笔 **Hardness**。 |

| CVar | 说明 |
| - | - |
| `ed_paintBrushSizeAdjustAmount` | 控制 `[` 和 `]` 热键以米为单位调整 Paint Brush Size 的程度。 |
| `ed_paintBrushHardnessPercentAdjustAmount` | 控制 `{` 和 `}` 热键以百分比 （0 - 100） 调整 Paint Brush Hardity（绘制笔刷硬度）的程度。 |
| `ed_paintBrushManipulatorInnerColor` | Paint Brush （绘制笔刷） 内圆的颜色，用于显示硬度衰减的开始位置。 |
| `ed_paintBrushManipulatorOuterColor` | Paint Brush 外圆的颜色，显示整体 Paint Brush 大小和硬度衰减结束的位置。 |

## PaintBrushSessionBus

PaintBrush 提供了一个 EBus，该 EBus 为支持运行时绘制的任何组件公开高级绘制 API。

| 方法名称 | 说明 | 参数 | 返回值 | 脚本化 |
|-|-|-|-|-|
| `StartPaintSession` | 启动运行时绘制会话并创建修改所需的临时数据缓冲区。 | PaintableEntityId: 包含可绘制组件的实体。  | `Uuid`: 新的绘制会话 ID。 | Yes |
| `EndPaintSession` | 结束给定 ID 的运行时绘制会话并清理临时数据缓冲区。 | sessionId: 要结束的绘制会话 ID。 | None | Yes |
| `BeginBrushStroke` | 使用给定的颜色/强度/不透明度画笔设置启动绘画会话的画笔描边。 | sessionId: 绘制会话 ID, brushSettings: 此画笔描边的画笔设置。 | None  | Yes |
| `EndBrushStroke` | 结束绘制会话的画笔描边。 | sessionId: 绘制会话 ID。 | None  | Yes |
| `IsInBrushStroke` | 返回绘制会话当前是否位于画笔描边的中间。 | sessionId: 绘制会话 ID。 | `bool`: 如果已启动画笔笔触，则为 True，否则为 false。  | Yes |
| `ResetBrushStrokeTracking` | 重置画笔跟踪，以便将下一个操作视为笔触移动的开始，而不是延续。 | sessionId: 绘制会话 ID。 | None  | Yes |
| `PaintToLocation` | 使用给定的画笔设置，将绘制颜色应用于从上一个位置到提供位置的基础数据。 | sessionId: 绘制会话 ID, brushCenter: 将画笔中心移动到的位置，brushSettings：此画笔描边的画笔设置。 | None  | Yes |
| `SmoothToLocation` | 使用给定的画笔设置将基础数据从上一个位置平滑到提供的位置。 | sessionId: 绘制会话 ID, brushCenter: 将画笔中心移动到的位置, brushSettings: 此画笔描边的画笔设置。 | None  | Yes |
