---
linkTitle: Actors 标签页
title: Scene Settings Actors 标签页
description: 场景设置Actors选项卡中可用的修改器和选项，用于处理具有Open 3D Engine (O3DE)骨架和皮肤数据的源资产。
weight: 400
toc: true
---

Actors是至少有一个骨骼的资产（不一定是角色），可以包含一个或多个蒙皮网格。默认情况下，源资产中的所有角色都会被处理。不过，您可以手动排除单个演员。您也可以处理单个源资产中的多个Actor。每个**Actor group**都会生成自己的`.actor` 和 `.skinmeta`产品资产。运行时产品资产作为源资产的子资产出现在**资产浏览器**中。

如果源资产至少包含一个骨骼，则Actors选项卡可用。

## Actors标签页属性

![The Scene Settings Actors tab.](/images/user-guide/assets/scene-settings/actors-tab.png)
1
| 属性 | 说明 |
| - | - |
| **Add another actor** | 添加一个actor组进行处理。每个角色组都会生成自己的`.actor` 和 `.skinmeta`产品资产。 |
| **Name actor** | actor组名称。这是资产浏览器中显示的产品资产名称。 |
| **Select root bone** | 指定骨骼层次结构的根骨。默认情况下，骨骼最顶端的父骨骼被选为根骨骼。 |
| **Add Modifier** | 修改器为处理源资产添加了专门的选项。选择**Add Modifier**按钮可显示可用修改器列表。除非在项目中启用了提供修改器的 gem，否则某些修改器不可用。 |

## Comment

![The Scene Settings Meshes tab Comment modifier.](/images/user-guide/assets/scene-settings/comment-modifier.png)

为actor组的文件添加注释。您可以添加对源资产文件所做更改的注释，以便跟踪或注释导出选项等。注释不会影响文件的处理方式。一个actor组可以添加多个注释修改器。

## Coordinate system change

坐标系更改修改器用于将三维场景源重新定向为Open 3D Engine (O3DE)坐标系。大多数数字内容创建 (DCC) 应用程序使用右手坐标系，**Y** 为向上轴。O3DE 的坐标系是左手坐标系，**Z** 为向上轴。

坐标系更改修改器有以下两种模式：

### Basic coordinate system change

![The Scene Settings Meshes tab Coordinate system change modifier basic settings.](/images/user-guide/assets/scene-settings/coordinate-system-change-modifier-1.png)

| 属性 | 说明 |
| - | - |
| **Facing direction** | 在处理产品资产时，围绕向上轴旋转 180 度。当源资产的前轴朝向查看器时（这是大多数 DCC 应用程序的默认情况），该属性可用于将产品资产重新定向到 O3DE 的前轴。 |
| **Use Advanced Settings** | 激活后，该属性将显示**Advanced coordinate system change**属性。 |

### Advanced coordinate system change

![The Scene Settings Actors tab Coordinate system change modifier advanced settings.](/images/user-guide/assets/scene-settings/coordinate-system-change-modifier-2.png)

更改网格的**Translation** (position), **Rotation** (orientation), 和 **Scale**，使之与创建时的方式相对应。

| 属性 | 说明 |
| - | - |
| **Relative Origin Node** | 选择作为演员组原点的源资产节点。演员组将相对于所选节点进行转换。该节点不必是演员组的一部分。一般来说，这里选择源资产的 `RootNode`，因为它代表源资产的场景原点。不过，在某些情况下，最好是相对于源文件中不是 `RootNode` 的骨骼或网格应用变换。 |
| **Translation** | 设置产品资产的位置偏移。该属性最常用于使产品资产居中或与地面对齐。  |
| **Rotation** | 设置产品资产的方向偏移（单位：度）。该属性最常用于根据 O3DE 坐标系调整产品资产的方向。|
| **Scale** | 设置产品资产的比例偏移。O3DE 的基本单位是一米。该属性最常用于对以厘米或英寸为基本单位的 DCC 应用程序中的产品资产进行统一缩放。 |

## Scale actor

![The Scene Settings Actors tab Scale actor modifier.](/images/user-guide/assets/scene-settings/scale-actor-modifier.png)

为actor组设置统一比例。如果源资产是在使用与 O3DE 不同的基本标准测量单位的 DCC 应用程序中创建的，则此设置非常有用。

| 属性 | 说明 |
| - | - |
| **Scale factor** | actor按此值统一缩放。 |

## Skeleton LOD

![The Scene Settings Actors tab Skeleton lod modifier.](/images/user-guide/assets/scene-settings/skeleton-lod-modifier.png)

骨架 LOD 是一种简化的骨架，其中一些叶骨已从骨架中移除。实体距离摄像机越远，对演员的细节要求就越低。以角色为例，在距离摄像机一定距离之外，可能不需要面部动画和单个手指骨骼等细节。这些细节所对应的骨骼可以从骨架中优化出来。在网络游戏中，专用服务器上使用的骨架可以优化为只包含转换碰撞器的骨骼。可以删除碰撞器下游的叶状骨骼。

您最多可以为actor组创建五个 LOD，它们的编号从\[`0`\]到 \[`4`\]，其中  \[`0`\] 是**最高**的细节级别。LODs 不是必需的，但建议使用，因为它们有助于在一系列具有不同硬件能力的平台上获得最佳性能和视觉保真度。

* 点击 {{< icon add.svg >}} **Add** 按钮添加一个 LOD。

* 点击 {{< icon delete.svg >}} **Delete** 按钮移除一个 LOD。

* 点击 {{< icon browse-edit-select-files.svg >}} **Selection list** 按钮来指定要包含在 LOD 中的骨骼。

{{< important >}}
取消选择骨骼 LOD 中的骨骼时，只应取消选择叶状骨骼（骨骼链末端的骨骼）。骨骼 LOD 中包含的骨骼应与网格 LOD 使用的骨骼相对应。不要取消选择位于骨链中间的骨骼。

例如，角色手部的 LOD 可能如下所示：

 * **LOD0** - 有独立的手指，每根手指都有指基、指中和指尖骨。
 * **LOD1** - 从 LOD 中取消选择并移除中指、无名指和小指的指尖骨骼。手指几何图形的皮肤与其余骨骼相连。
 * **LOD2** - 将中指、无名指和小指的网格融合成一个手套，并为两根中指骨骼剥皮。无名指和小指的骨骼从 LOD 中取消选择并移除。
 * **LOD3** - 将所有手指的几何形状融合为一个手套，并在食指基骨上剥离。食指的中指骨和指尖骨以及中指骨被取消选择并从 LOD 中移除。
 * **LOD4** - 将所有手指几何图形与手骨融合并蒙皮。从 LOD 中取消选择并移除所有手指骨骼。
{{< /important >}}

## Skeleton optimization

![The Scene Settings Actors tab Skeleton optimization modifier.](/images/user-guide/assets/scene-settings/skeleton-optimization-modifier.png)

骨架优化为自动生成骨架 LOD 提供了几个选项。
1
| 属性 | 说明 |
| - | - |
| **Auto Skeleton LOD** | 激活时，客户端骨骼 LOD 会根据皮肤权重数据自动生成。在相应的网格 LOD 中没有皮肤权重的叶骨会从骨架中优化出来。 |
| **Server Skeleton Optimize** | 激活时，服务器端骨架 LOD 会根据碰撞检测碰撞器自动生成。没有碰撞器的叶骨会从骨架中优化出来。 |
| **Critical bones** | 选择 {{< icon browse-edit-select-files.svg >}} **Selection list** 按钮。**Selection list**按钮来选择在自动 LOD 生成过程中不会被优化掉的骨骼。这对于叶状骨骼没有皮肤权重但用于附加（例如角色手持物体）的情况非常有用。 |
