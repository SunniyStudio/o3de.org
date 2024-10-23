---
linkTitle: Motions 标签页
title: Scene Settings Motions 标签页
description: 场景设置动作选项卡中可用的修改器和选项，用于处理Open 3D Engine (O3DE)的动画源资产。 
weight: 500
toc: true
---

您可以将源资产中的动画序列处理为**Motions**。Scene Settings中的每个动作都会生成自己的`.motion`产品资产。运行时产品资产作为源资产的子资产出现在**资产浏览器**中。

{{< important >}}
源资产中的动画应该是**烘焙**的，也就是说，每个动画通道应该在每一帧都有关键帧。数字内容创建 (DCC) 应用程序可能会使用函数和模拟在关键帧之间进行插值，除非对动画进行烘焙，否则结果可能无法在源资产中准确呈现。
{{< /important >}}

更多信息，请参阅 [动画编辑器文件类型](/docs/user-guide/visualization/animation/character-editor/file-types/)。

如果源资产包含带有关键帧动画的骨架，则可使用 Motions选项卡。

## Motions 标签页属性

![The Scene Settings Motions tab.](/images/user-guide/assets/scene-settings/motions-tab.png)
1
| 属性 | 说明 |
| - | - |
| **Add another motion** | 添加要处理的动作。每个动作都会生成一个 `.motion` 产品资产。|
| **Name motion** | 动作名称。这是显示在 “资产浏览器 ”中的产品资产名称。|
| **Select root bone** | 指定动画骨骼层次结构的根骨骼。默认情况下，骨骼最顶端的父骨骼会被选为根骨骼。 |
| **Add Modifier** | 修改器为处理源资产添加了专门的选项。选择**Add Modifier**按钮可显示可用修改器列表。除非在项目中启用了提供修改器的 gem，否则某些修改器不可用。 |

## Additive motion

![The Scene Settings Motions tab Additive motion modifier.](/images/user-guide/assets/scene-settings/additive-motion-modifier.png)

创建叠加动作 叠加动作可以分层叠加在基本动作之上，而不会影响基本动作的功能。

| 属性 | 说明 |
| - | - |
| **Base Frame** | 指定包含参考姿势的基帧的编号。 |

## Comment

![The Scene Settings Motions tab Comment modifier.](/images/user-guide/assets/scene-settings/comment-modifier.png)

为动作文件添加注释。您可以添加对源资产文件所做更改的注释，以便跟踪或注释导出选项等。注释不会影响文件的处理方式。一个动作可以添加多个注释修改器。

## Coordinate system change

坐标系更改修改器用于将三维场景源重新定向为开放三维引擎（O3DE）坐标系。大多数数字内容创建 (DCC) 应用程序使用右手坐标系，**Y** 为向上轴。O3DE 的坐标系是左旋坐标系，**Z** 为向上轴。

坐标系更改修改器有以下两种模式：

### Basic coordinate system change

![The Scene Settings Meshes tab Coordinate system change modifier basic settings.](/images/user-guide/assets/scene-settings/coordinate-system-change-modifier-1.png)

| 属性 | 说明 |
| - | - |
| **Facing direction** | 在处理产品资产时，围绕向上轴旋转 180 度。当源资产的前轴朝向查看器时（这是大多数 DCC 应用程序的默认情况），该属性可用于将产品资产重新定向到 O3DE 的前轴。|
| **Use Advanced Settings** | 激活后，该属性将显示**Advanced coordinate system change**属性。 |

### Advanced coordinate system change

![The Scene Settings Meshes tab Coordinate system change modifier advanced settings.](/images/user-guide/assets/scene-settings/coordinate-system-change-modifier-2.png)

更改网格的**Translation** (position), **Rotation** (orientation), 和 **Scale**，使之与创建时的方式相对应。

| 属性 | 说明 |
| - | - |
| **Relative Origin Node** | 选择作为动作原点的源资产节点。动作将相对于所选节点进行转换。该节点不必是动画骨骼的一部分。一般情况下，这里会选择源资产的`RootNode`，因为它代表源资产的场景原点。但在某些情况下，最好是相对于源文件中不是`RootNode`的骨骼或网格应用变换。 |
| **Translation** | 设置产品资产的位置偏移。该属性最常用于使产品资产居中或与地面对齐。  |
| **Rotation** | 设置产品资产的方向偏移（单位：度）。该属性最常用于根据 O3DE 坐标系调整产品资产的方向。 |
| **Scale** | 设置产品资产的比例偏移。O3DE 的基本单位是一米。该属性最常用于对以厘米或英寸为基本单位的 DCC 应用程序中的产品资产进行统一缩放。 |

## Motion range

![The Scene Settings Motions tab Motion range modifier.](/images/user-guide/assets/scene-settings/motion-range-modifier.png)

设置动作的动画范围。此功能用于从源资产中提取动作，在单帧范围内包含多个动画片段，一个接一个。

| 属性 | 说明 |
| - | - |
| **Start frame** | 指定动作的起始关键帧。 |
| **End frame** | 指定动作的结束关键帧。 |

{{< important >}}
`.motion`产品资产的索引以`0`作为第一帧。如果您的动画有 30 帧，则它们在`.motion`文件中被索引为`0`至`29`帧。如果您通常使用帧 `1` 作为起始帧，或使用帧 `0` 或 `-1` 在源资产中存储休息姿势，请记住这一点。
{{< /important >}}

## Motion sampling

![The Scene Settings Motions tab Motion sampling modifier.](/images/user-guide/assets/scene-settings/motion-sampling-modifier.png)

当Actor有复杂的骨架、许多动画序列或较长的动画序列时，对动作产品资产的资源要求会很高。动作采样有几种属性，可以用来平衡动作的质量和性能以及动作产品资产的大小。

| 属性 | 说明 |
| - | - |
| **Motion data type** | 定义动作数据的存储方式。默认设置`Automatic`可在内存限制范围内创建最高质量的动画。`Evenly spaced keyframes`创建的动作产品资产可能更接近源资产，但可能需要更多内存。`Reduced keyframes`可优化关键帧并生成所需内存更少但处理时间更长的产品资产。 |
| **Sample rate** | 采样率的单位是帧每秒（FPS）。`From source scene`使用源资产中指定的采样率。`Custom sample rate`显示**Custom sample rate**属性。 |
| **Custom sample rate** | 指定以每秒帧数 (FPS) 为单位的动作采样率。数值范围从 `1` 到 `240`。数值越大，生成的动作产品资产越平滑、保真度越高，但会增加内存需求。 |
| **Translation quality (%)** | 设置骨骼平移的质量。百分比值越低，骨骼平移动画的关键帧数量就越少，从而可以节省内存，但会降低动画质量。 |
| **Rotation quality (%)** | 设置骨骼旋转的质量。百分比值越低，骨骼旋转动画的关键帧数量就越少，从而可以节省内存，但会降低动画质量。 |
| **Scale quality (%)** | 设置骨骼缩放的质量。百分比值越低，骨骼缩放动画的关键帧数量就越少，也就越能节省内存，但会降低动画质量。 |
| **Allowed memory overhead (%)** | 与最小产品资产大小相比，可使用的额外内存百分比。增加该值可以提高动画质量，同时限制产品资产的内存需求。 |
| **Keep duration** | 激活时，即使没有骨骼动画，动作产品资产的持续时间也与场景资产相同。否则，持续时间取决于骨骼是否被动画化。 |

## Scale motion

![The Scene Settings Motions tab Scale motion modifier.](/images/user-guide/assets/scene-settings/scale-motion-modifier.png)

为动作设置统一比例。如果源资产是在使用与 O3DE 不同的基本标准测量单位的 DCC 应用程序中创建的，则此设置非常有用。
1
| 属性 | 说明 |
| - | - |
| **Scale factor** | 动作按此值均匀缩放。 |
