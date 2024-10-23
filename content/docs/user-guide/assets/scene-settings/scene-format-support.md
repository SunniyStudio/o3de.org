---
linkTitle: 场景格式支持
title: 场景格式支持
description: 有关Open 3D Engine (O3DE)支持的 3D 场景文件类型和数据的信息。
weight: 100
toc: true
---

**Open 3D Engine (O3DE)** 集成了[Open Asset Import Library](https://github.com/assimp/assimp)来解析三维场景源资产。O3DE 支持的主要三维场景源资产格式是 `.fbx`。O3DE 还支持 `.stl`（一种广泛应用于计算机辅助设计（CAD）和 3D 打印的格式），并在开发中支持 `.glTF`（一种用于运行时应用程序的开源传输格式，越来越受欢迎）。

开放资产导入库支持许多常见的[场景格式](https://github.com/assimp/assimp/blob/master/doc/Fileformats.md)。如果您想尝试 Open Asset Import Library 支持的其他格式，可以编辑`o3de/Registry/sceneassetimporter.setreg`设置文件，并将格式扩展名添加到 `“SupportedFileTypeExtensions”:`列表中。

## 坐标系

了解 O3DE 的坐标系和数字内容创建 (DCC) 应用程序的不同坐标系非常重要。如果源资产使用的坐标系与 O3DE 不同，则在加载到 **O3DE 编辑器**时，资产的方向可能不正确。

O3DE 使用**右手**坐标系，**Z**为上坐标轴。DCC 应用程序通常使用右旋坐标系，**Y** 为上坐标轴。在右手坐标系中，从正**X**轴到正**Y**轴的正向旋转是逆时针方向。

在 O3DE 的坐标系中，如果正**X**轴向右，则前轴指向远离观察者的方向。而在大多数 DCC 应用程序坐标系中，正**X**轴向右，前进轴指向观察者。O3DE 坐标系与常见 DCC 应用程序坐标系的比较见下图。

{{< image-width "/images/user-guide/assets/scene-settings/o3de-coordinate-system.svg" "650" "O3DE 坐标系与普通内容创建应用程序坐标系的对比示例图片。" >}}

大多数 DCC 应用程序都有用户设置，您可以使用这些设置来更改坐标系，或者在导出场景时使用这些选项来确定场景的方向，从而确保资产在 O3DE 中的方向正确。否则，**场景设置**提供了一个**Coordinate system change**修改器，您可以在处理源资产时将三维场景的方向调整为 O3DE 的坐标系。

## 世界测量单位

O3DE 的基本测量单位是每世界单位一立方米。大多数 DCC 应用程序使用的基础测量单位要小得多，例如每个世界单位为一立方厘米。这种差异可能会导致产品资产在 O3DE 中显得微小，这就要求源资产在 O3DE 的基本测量单位中统一按 `100` 的系数缩放。

大多数 DCC 应用程序都有用户设置，可用于更改 DCC 的基本测量单位，或在导出场景时用于缩放场景的选项，以确保资产在 O3DE 中的比例正确。否则，场景设置坐标系更改修改器具有**高级设置**，您可以使用它来缩放源资产，以考虑 DCC 应用程序和 O3DE 之间的差异。

## 支持的3D场景数据

处理三维场景文件源资产时，会创建运行时优化的产品资产，并将其存储在项目的**资产缓存**中。在大多数情况下，源资产和生成的产品资产数量之间并不是一一对应的关系。例如，一个简单的网格可能会生成一个 `.azmodel`、几个 `.azlod` 和 `.pxmesh`产品资产，甚至更多的 `.azbuffer`产品资产。每个产品资产都包含该资产所需的场景数据子集。

O3DE 支持多种三维场景数据，但是，由于各种中间场景格式的规范以及实时应用硬件资源的限制，可用于产品资产处理的数据有一定的局限性。

下表概述了 O3DE 支持的三维场景数据及其限制。

{{< important >}}
下表中的数据代表 O3DE 支持的所有 3D 场景数据。某些源资产文件类型仅支持这些数据的子集。

例如，流行的三维场景文件格式（如`.stl`和`.obj`）对某些类型的网格数据的支持有限，而且不支持动画。其他三维场景文件格式（如 `.bvh`）只支持骨架层次和关键帧运动数据。O3DE 中的各种**资产生成器**只为资产生成器和源资产文件格式都支持的数据生成产品资产。
{{< /important >}}

| 数据 | 限制 |
| - | - |
| **Meshes** | 无限制。每个源资产可指定任意数量的网格组，其中包含任意数量的网格。 |
| **Vertices per mesh** [<sup>**1**</sup>](#vertex-limit) | 4,294,967,295 |
| **Vertex precision** | 32位精度。  |
| **Normals** | 从源资产中自定义法线，或自动生成（平均）法线。 |
| **Tangents** | 源资产中的自定义切线，或 MikkT 算法自动生成的切线。 |
| **Bitangents** | 来自源资产的自定义位切线，或使用 MikkT 算法自动生成的位切线。 |
| **UV Sets** [<sup>**2**</sup>](#buffer-limit)  | 默认着色器支持 2 个 UV 集（`UV0` 和 `UV1`）。 |
| **Vertex Colors** [<sup>**2**</sup>](#buffer-limit)  | 默认着色器支持 1 个顶点颜色流。额外的顶点颜色流可为布料模拟存储约束和反质量数据。 |
| **Materials** | 每个多边形 1 个。 |
| **Physics materials** | 每个材质1个。 |
| **Level of detail (LOD)** | 5 个网格 LOD 和 5 个骨骼 LOD，编号为 **[0]** 至 **[4]**（不包括基本网格和骨骼）。 |
| **Bone influences per vertex** [<sup>**3**</sup>](#skin-weights) | 每个顶点最多 32 个，默认限制为每个顶点 8 个。 |
| **Bones per skeleton** | 无限制。骨架可以有任意数量的骨头。  |
| **Motions** | 无限制。一个运动可以有任意数量的关键帧，可以从源资产中指定多个运动，分段设置开始和结束关键帧。 |
| **Cloth meshes** [<sup>**4**</sup>](#cloth-meshes) | 无限制。 |
| **PhysX colliders** [<sup>**5**</sup>](#physx-colliders) | 无限制。PhysX 碰撞体可以是通过源资产处理的三角形网格，也可以是自动生成的形状或凸壳碰撞体。 |

{{< note >}}
<a name="vertex-limit"></a>
<sup>**1**</sup> **Vertices per mesh limit**

每个网格的顶点限制是理论值。实际顶点总数取决于目标平台的硬件资源和性能。

<a name="buffer-limit"></a>
<sup>**2**</sup> **UV set and vertex color limits**

虽然默认着色器只支持 2 个 UV 集和 1 个顶点颜色流，但可以添加对其他 UV 集和顶点颜色流的支持。每个 `.azmodel`最多可以有 12 个 `.azbuffer`产品资产，用于存储额外的模型数据，包括 UV 集和顶点颜色流。UV 集和顶点颜色流的总数受可用`.azbuffer`产品资产的限制。例如，假设一个 `.azmodel` 产品资产有以下缓冲区：

* index
* position
* normals
* tangents
* bitangents
* skin weights
* joint indices

在上述资产示例中，12 个可用缓冲区中有 7 个包含数据，剩下的 5 个缓冲区可用于 UV 集和顶点颜色流。

<a name="skin-weights"></a>
<sup>**3**</sup> **Bone influences per vertex limit**

虽然每个顶点的最大骨骼影响数（皮肤权重）为 32，但在[设置注册表](../../../settings)中为项目指定了一个较低的默认值。可以修改以下设置注册表值来更改默认皮肤权重设置。在某些目标平台上，允许减少每个顶点的骨骼影响可以提供更好的动画性能。

| 设置 | 说明 | 默认值 |
| :-- | :-- | :-: |
| `/O3DE/SceneAPI/SkinRule/DefaultMaxSkinInfluencesPerVertex` | 可以影响Actor网格中任何顶点的骨骼的最大数量。Actor网格中的每个顶点都可以拥有任意数量的皮肤权重，这些皮肤权重的数量介于 1 和此注册值中指定的数量之间。 | 8 |
| `/O3DE/SceneAPI/SkinRule/DefaultWeightThreshold` | 皮肤重量的最小值。Skin权重值低于此限值时将被忽略。 | 0.001 |

通过在[场景设置](../scene-settings)的[皮肤修改器](../meshes-tab#skin) 中为Actor的网格添加[皮肤修改器](../meshes-tab#skin) ，可以覆盖每个Actor的最大骨骼影响数和权重阈值。

<a name="cloth-meshes"></a>
<sup>**4**</sup> **Cloth mesh limitations**

虽然可以处理任意数量的网格布，但 O3DE 支持的网格布数量和分辨率取决于目标平台的能力。

<a name="physx-colliders"></a>
<sup>**5**</sup> **Physics colliders limitations**

PhysX 碰撞体资产生成器可以从三角形网格、基元或凸壳生成碰撞体。生成的碰撞体可自动与网格资产相匹配，复杂网格也可分解为凸形部分。由于源资产网格可以组合或分解成凸形部分来生成碰撞体，因此网格产品资产的数量与 PhysX 碰撞体产品资产的数量之间可能不存在一一对应的关系。

虽然可以处理任意数量的 PhysX 碰撞体，但 O3DE 可支持的 PhysX 碰撞体数量和分辨率取决于目标平台的能力。一般来说，原始碰撞体可提供最佳性能。
{{< /note >}}

## `.fbx` 文件支持

`.fbx`格式便于携带，支持广泛。它能存储包含多个资产的完整 3D 场景，每个资产由多个网格、材质、骨骼、动画、顶点属性等组成。虽然可以在单个`.fbx`文件中包含任意数量的资产，但建议您只包含角色或道具等单个资产所需的数据。确保`.fbx`源资产只生成单个角色或道具的产品资产，可以防止出现依赖和资产再处理问题。

`.fbx`源资产可以包含上表中列出的任何数据，包括网格、骨骼、动画和 PhysX 网格。

{{< note >}}
`.fbx`文件可以是 ASCII 或二进制格式。如果您打算使用自动化和脚本来处理`.fbx`源资产，您可能更喜欢使用 ASCII 格式的`.fbx`文件，因为它们可以很容易地被 Python 等脚本语言读取和修改。
{{< /note >}}

## `.stl` 文件支持

`.stl`格式只支持由三角形组成的封闭流形网格。也就是说，网格不能有开放的边。每条边必须由两个三角形共享。`.stl`格式不支持 UV、材质、法线和皮肤权重等其他网格数据。支持 `.stl`是为了方便 CAD 和 3D 打印应用程序的用户。

## glTF 文件支持

{{< feature-in-progress "glTF support" "https://github.com/o3de/o3de/issues?q=is%3Aissue+is%3Aopen+in%3Atitle+gltf" "https://github.com/o3de/o3de/pulls?q=is%3Apr+is%3Aopen+in%3Atitle+gltf">}}

glTF 是一种开源三维场景传输格式，在实时应用中越来越受欢迎。glTF 支持包含多个资产的完整 3D 场景，每个资产由多个网格、材料、骨骼、动画、顶点属性等组成。

glTF 源资产可以三种不同的方式存储：

* **Binary** (`.glb`). 包括纹理资产在内的整个场景都嵌入在一个扩展名为`.glb`的二进制文件中。这种格式提供了最小且最易于传输的源资产。
* **ASCII embedded** (`.gltf`). 整个场景存储在一个单一的 JSON 格式 ASCII 文件中，扩展名为 `.gltf`。场景数据和图像在`.gltf`文件中以 Base64 编码。这种格式会生成一个大文件，但好处是可以提供完全以文本文件编码的源资产。
* **ASCII and binary** (`.gltf` 场景描述, `.bin` 数据, 和贴图文件). 场景描述存储在一个 JSON 格式的 ASCII 文件中，扩展名为`.gltf`。场景数据存储在扩展名为`.bin` 的二进制文件中。纹理以指定的图像格式单独存储。这种格式提供了合理的小文件，以及随时调试源资产和更新纹理的独特机会，但代价是源资产要存储在多个文件中。 

{{< note  >}}
在编写本文档时，Blender 为 O3DE 提供了近乎完整的 glTF 支持，但也存在一些可以缓解的问题。使用 ASCII 和二进制（`.gltf`场景描述、`.bin`数据和纹理文件）源资产以及 **Visual Studio Code** 和 **glTF Tools** 扩展提供了调试 glTF 源资产的其他方法。

有关 glTF 支持的当前状态，请参阅[glTF 支持](https://github.com/o3de/o3de/issues/6007)功能请求。
{{< /note  >}}

有关 `.glTF` 的更多信息，请访问 [glTF 规范主页](https://www.khronos.org/gltf/)。

