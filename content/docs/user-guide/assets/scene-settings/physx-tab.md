---
linkTitle: PhysX 标签页
title: Scene Settings PhysX 标签页
description: 场景设置 PhysX 选项卡中可用的修改器和选项，用于为Open 3D Engine (O3DE)生成 PhysX 碰撞体。
weight: 600
toc: true
---

您可以通过定义 PhysX 网格组创建 PhysX 碰撞体产品资产，用于 PhysX 仿真、碰撞检测和触发。碰撞体资产可以是三角形网格，也可以根据源资产中包含的网格生成基元或凸形网格。单个源资产可处理多个 PhysX 网格组。每个 PhysX 网格组都会生成自己的 `.pxmesh` 产品资产。运行时产品资产作为源资产的子资产出现在**资产浏览器**中。

您可以在源资产中为网格命名后缀为 `_phys`的网格，以便自动将其作为 PhysX 碰撞体处理。当源资产中的网格后缀名为`_phys`时，它就会被识别为碰撞体，并自动添加到默认的 PhysX 网格组中。

{{< important >}}
创建 PhysX 碰撞体有很多选项。场景中的**最佳**选项取决于许多因素，包括网格复杂度、碰撞体的使用方式以及包含碰撞体的实体是静态（不动）、运动（动画）还是模拟（受力和碰撞驱动）。一般来说，原始碰撞体的仿真性能最好，但在需要碰撞体资产与可见网格的形状非常匹配的情况下，可以考虑用性能换取精度。
{{< /important >}}

如果源资产至少包含一个网格，则 PhysX 选项卡可用。

## PhysX 标签页属性

![The Scene Settings PhysX tab properties.](/images/user-guide/assets/scene-settings/physx-tab.png)
1
| 属性 | 说明 |
| - | - |
| **Add another physxmesh** | 从源资产中添加一个 PhysX 网格组，作为 PhysX 碰撞体\(`.pxmesh`\)产品资产进行处理。 |
| **Name PhysX Mesh** | 输入 PhysX 网格组的名称。这是作为源资产子资产出现在 “资产浏览器 ”中的`.pxmesh`产品资产的名称。 |
| **Select Meshes** | 点击 {{< icon browse-edit-select-files.svg >}} **Selection list** 按钮可显示源资产文件中的网格列表。从列表中选择网格，将其包含在 PhysX 网格组中。所选网格可以是作为碰撞网格构建的，也可以是可见网格。所选网格将用于自动生成碰撞产品资产。源资产中以 `_phys` 后缀命名的网格会被自动选入默认的 PhysX 网格组。 |
| **Export As** | 应用于该 PhysX 网格组的烹饪方法。此设置会显示所选烹饪方法的属性。有三个选项：`Triangle Mesh`, `Convex`, 和 `Primitive`三个选项，详见以下章节。 |
| **Decompose Meshes** | 激活后，V-HACD 算法会将所选网格分割成多个凸形部分，这些部分共同近似于原始网格形状。每个部分都会使用上面配置的属性以凸碰撞体的形式输出。分解网格 "公开了**Decomposition Properties**，这些属性决定了如何将所选网格分解成凸形部分。 只有在**Export As**属性中选择`Convex` 或 `Primitive`时，**Decompose Meshes**才可用。更多信息和结果示例，请参阅 [GitHub 上的 V-HACD 库](https://github.com/kmammou/v-hacd)。 |
| **Physics Materials** | A为所选网格中的每种材质关联物理材质。物理材料定义了表面的物理属性，如摩擦力。 |

## Triangle mesh asset

![The Scene Settings PhysX triangle mesh asset properties.](/images/user-guide/assets/scene-settings/physx-triangle-mesh-asset.png)

三角形网格碰撞体能精确再现所选网格的形状，但不能用于模拟实体。三角形网格碰撞体最适用于形状复杂的静态环境实体，这些实体需要与可见网格形状精确相似的碰撞体。

| 属性 | 说明 |
| - | - |
| **Merge Meshes** | 激活后，所有选中的网格节点都会合并为一个碰撞网格。否则，选定的网格节点将作为单独的图形导出。通常，使用单个碰撞网格会更有效率。 |
| **Weld Vertices** | 激活时，将执行网格顶点焊接。当**Weld Vertices**开启时，**Disable clean mesh**必须关闭，否则不会执行顶点焊接。 |
| **Disable Clean Mesh** | 激活时，不进行网眼清洁。这样可以加快烹饪速度。当**Weld Vertices**开启时，**Disable clean mesh**必须关闭，否则不会执行顶点焊接。 |
| **Force 32-bit Indices** | 启用后，无论三角形数量多少，都会为碰撞体资产创建 32 位索引。 |
| **Suppress Triangle Mesh Remap Table** | 激活时，不会创建面重新映射表。这样可以节省大量内存，但如果没有重映射信息，就无法返回碰撞、扫描或射线投射命中的内部网格三角形。 |
| **Build Triangle Adjacencies** | 激活时，会创建三角形邻接信息。您可以使用 `getTriangle`方法获取给定三角形的三角形邻接信息。 |
| **Mesh Weld Tolerance** | 当**Weld Vertices**激活时，此值为焊接顶点的距离。当**Weld Vertices**处于非激活状态时，此值定义了网格验证的接受距离。**Weld Vertices**采用网格捕捉方法，使用**Mesh Weld Tolerance**将每个顶点截断为整数值。一旦生成了这些咬合顶点，所有与网格上给定顶点咬合的顶点都会重新映射为一个顶点。然后，所有三角形的索引都会重新映射，以引用这个干净的顶点子集。顶点的位置不会被修改。网格捕捉只是为了识别附近的顶点。网格验证方法使用相同的 “捕捉到网格 ”方法来识别附近的顶点。如果有多个顶点卡入给定的网格坐标，则会检查顶点之间的距离，确保其大于 **Mesh Weld Tolerance**。如果顶点之间的距离在 **Mesh Weld Tolerance** 范围内，系统将发出警告。 |
| **Number of Triangles Per Leaf** | 设置网格烹饪提示为每叶最大三角形。每叶三角形数越少，烹饪速度越慢，生成的网格尺寸越大，运行性能越好。每叶三角形数越多，烹饪速度越快，生成的网格尺寸越小，运行性能越差。 |

## Convex asset

![The Scene Settings PhysX convex asset properties.](/images/user-guide/assets/scene-settings/physx-convex-asset.png)

凸面船体是生成的碰撞体，可以近似所选网格的形状。凸面船体可用于静态实体、运动实体和模拟实体，通常用于需要刚体物理特性和与可见网格形状相似的碰撞网格的交互式实体。

| 属性 | 说明 |
| - | - |
| **Area Test Epsilon** | 如果船体中三角形的面积小于此值，则剔除该三角形。只有当**Check Zero Area Triangles**处于激活状态时，才会执行此测试。有效值范围从 `0` 到 `100`。|
| **Plane Tolerance** | 此值用于建造船体。如果添加到船体中的新点比**Plane Tolerance**值更接近船体，则会被拒绝。如果**Plane Tolerance**值为 `0.0`，则所有点都会被接受。这可能会导致边缘情况，即新点被合并到现有的多边形中，使多边形的平面等式发生微小变化，从而导致在弧体计算的多边形合并阶段出现故障。**Plane Tolerance**值会根据船体大小增加，建议使用默认值`0.0006`。但是，如果必须接受所有点，或者必须创建大而薄的凸形船体，则可以指定一个较低的值。有效值范围从 `0` 到 `100`。 |
| **Use 16-bit Indices** | 激活时，将生成 16 位三角形或多边形索引。否则，将生成 32 位顶点索引。 |
| **Check Zero Area Triangles** | 删除面积小于 **Area Test Epsilon**中指定值的三角形。 |
| **Quantize Input** | 使用 K-means 聚类对输入顶点进行量化。K 均值聚类将输入网格数据划分为 Voronoi 单元。  |
| **Use Plane Shifting** | 平面移动是一种替代算法，适用于计算出的全壳顶点数量超过每个全壳顶点数量限制的情况。默认算法会计算全壳和输入顶点周围的包围盒。然后用全壳平面对边界框进行切分，直到达到顶点限制。默认算法产生的结果通常比平面移动产生的结果质量要好得多。当激活平面移动时，当达到顶点限制时，全壳计算就会停止。然后会移动全图平面以包含所有输入顶点，然后使用新的平面交叉点生成具有给定顶点限制的最终全图。平面移动可能会在远离输入网格的顶点处产生尖锐的边缘，而且不能保证所有输入顶点都在生成的曲面内。 |
| **Shift Vertices** | 激活后，输入网格的顶点会围绕原点移动，以提高计算的稳定性。当输入网格不是以原点为中心时，请使用此功能。 |
| **Gauss Map Limit** | 顶点限值，超过此限值将为每个凸网格计算额外的加速结构。数值越大，内存使用量越少。较低的值可提高性能，具体取决于目标平台。|
| **Build GPU Data** | 激活后，将创建用于 GPU 加速刚体模拟的数据。这会增加内存使用量和烹饪时间。在内部，顶点限制设置为 64，每个面的顶点限制设置为 32。 |

## Primitive asset

![The Scene Settings PhysX primitive asset properties.](/images/user-guide/assets/scene-settings/physx-primitive-asset.png)

原始碰撞体是与输入网格相匹配的简单参数化形状基元（盒状、囊状、球状），可用于静态、运动和模拟实体。原始碰撞体通常能提供最佳的仿真性能，但可能与可见网格的形状不完全匹配。它们最适用于具有简单网格的模拟实体、弹丸、触发器和不需要精确代表可见网格形状的碰撞体的实体。

| 属性 | 说明 |
| - | - |
| **Target Shape** | 拟合到输入网格的原始形状。`Automatic`决定哪个形状最适合。 |
| **Volume Term Coefficient** | 指定拟合算法尝试将原始形状与输入网格拟合的紧密程度。对于大多数网格，建议使用 `0`（无体积最小化）。如果输入网格的顶点较少，或顶点主要位于边缘上，拟合结果可能会使原始形状与输入网格内部相匹配。增加该值可以改善结果，但过高的值可能会产生负面结果。 |

## Decompose meshes

![The Scene Settings PhysX decompose meshes properties.](/images/user-guide/assets/scene-settings/physx-decompose-meshes.png)

如果网格的形状是凹形的，或者与某个基本形状不匹配，那么将 PhysX 网格导出为凸形碰撞体或原始碰撞体可能不会产生很好的效果。将 PhysX 网格导出为三角形网格碰撞体会创建一个与原始网格非常相似的碰撞体，但无法与模拟实体一起工作。针对这些情况，O3DE 支持近似凸分解。在通过资产管道单独处理每个部分之前，任意网格会被分解成近似原始形状的凸形部分。

分解网格的优点是，每个单独的凸形部分都可以作为凸形或基元近似输出。由于生成的资产不包含任何三角形网格，因此可用于模拟实体。

更多信息和结果示例，请参阅 [GitHub 上的 V-HACD 库](https://github.com/kmammou/v-hacd)。

| 属性 | 说明 |
| - | - |
| **Maximum Hulls** | 指定可生成的最大船体数量。取值范围从 `1` 到 `1024`。 |
| **Maximum Vertices Per Hull** | 指定每个凸壳的最大顶点数。数值范围从 `4` 到 `1024`。 |
| **Concavity** | 每个凸壳的最大凹度。 |
| **Resolution** | 体素化阶段生成的体素的最大数量。有效值范围从最小值`10000`到最大值`64000000`。 |
| **Mode** | 使用 `Voxel-based` (cubic) 或 `Tetrahedron-based`的体积进行近似凸分解。|
| **Alpha** | 沿对称平面剪切的偏差。数值范围从 `0.0` 到 `1.0`。|
| **Beta** | 指定沿旋转轴剪切的偏差。数值范围从 `0.0` 到 `1.0`。|
| **Minimum Volume Per Hull** | 指定生成的凸壳的自适应采样。取值范围从 `0.0` 到 `0.01`。 |
| **Plane Downsampling** | 搜索**最佳**剪切平面的粒度。取值范围从 “1 ”到最大值 “16”。 |
| **Hull Downsampling** | 剪切平面选择阶段凸壳生成过程的精度。取值范围为 `1` 至 `16`。 |
| **Enable PCA** | 激活后，在应用凸分解之前会对输入网格进行归一化处理。 |
| **Project Hull Vertices** | 激活时，输出的凸壳顶点会投影到输入网格上，以提高结果的浮点精度。 |

## Comment

![The Scene Settings PhysX tab Comment modifier.](/images/user-guide/assets/scene-settings/comment-modifier.png)

为 PhysX 网格组文件添加注释。您可以添加对源资产文件所做更改的注释，以便跟踪或注释导出选项等。注释不会影响文件的处理方式。一个 PhysX 网格组可以添加多个注释修改器。
