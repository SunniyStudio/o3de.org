---
linkTitle: Meshes 标签页
title: Scene Settings Meshes 标签页
description: 场景设置Meshes 选项卡中可用的修改器和选项，用于处理Open 3D Engine (O3DE)的网格源资产。
weight: 300
toc: true
---

在**Meshes**选项卡中，您可以创建网格组作为网格产品资产进行处理。默认情况下，所有未指定细节级别（以 `_lod<n>`后缀命名的网格）或未指定为 PhysX 网格（以 `_phys`后缀命名的网格）的网格都会在一个网格组中处理。您可以创建任意数量的网格组，其中包含源资产中的特定网格。每个网格组都会生成自己的产品资产。运行时产品资产作为源资产的子资产出现在**资产浏览器**中。

如果源资产至少包含一个网格，则网格选项卡可用。

## Meshes 标签页属性

![The Scene Settings Meshes tab.](/images/user-guide/assets/scene-settings/meshes-tab.png)


| 属性 | 说明 |
| - | - |
| **Add another mesh** | 添加要处理的网格组。每个网格组可包含源资产文件中的一个或多个网格。每个网格组都会生成一个`.azmodel`，以及所需数量的`.azlod` 和 `.azbuffer`产品资产。|
| **Name mesh**  | 网格组的名称。该网格组的所有产品资产都使用该名称作为前缀。该名称会显示在资产浏览器中该网格组的产品资产上。 |
| **Select meshes** | 选择 {{< icon browse-edit-select-files.svg >}} **Selection list** 按钮。选择**Selection list**按钮，显示源资产中的网格列表。从列表中选择网格，将其包含在网格组中。如果源资产中包含用于 PhysX 碰撞体资产的网格，则应在此列表中取消选择，将其排除在网格组之外。 |
| **Add Modifier** | 修改器为处理源资产添加了专门的选项。选择**Add Modifier**按钮可显示可用修改器列表。除非在项目中启用了提供修改器的 gem，否则某些修改器可能不可用。 |

## Cloth

![The Scene Settings Meshes tab Cloth modifier.](/images/user-guide/assets/scene-settings/cloth-modifier.png)

为选定的网格添加 NVIDIA 布料数据，以模拟布料物理特性。

{{< note >}}
网格组中每个要模拟为布料的网格都需要有自己的布料修改器。在网格组中添加布料修改器后，***Mesh (Advanced)** 修改器中的**Merge Meshes**属性将被视为禁用。布网格需要独立处理，不能与其他网格合并。更多信息，请参阅 [使用英伟达布料模拟布料](/docs/user-guide/interactivity/physics/nvidia-cloth/).

关于下面的**Inverse Masses**, **Motion Constraints**, 和 **Backstop**属性，请参阅 [布的每个顶点属性](/docs/user-guide/interactivity/physics/nvidia-cloth/vertex-data)。
{{< /note >}}

| 属性 | 说明 |
| - | - |
| **Select Cloth Mesh** | 选择要应用布料数据的网格，并将其模拟为布料对象。 |
| **Inverse Masses** | 选择顶点颜色流，为布料模拟应用每个顶点的反质量数据。如果没有选择顶点颜色流，则会将`1.0`的反质量值分配给布料网格中的所有顶点。 |
| **Inverse Masses Channel** | 选择顶点颜色流中包含反质量数据的通道。 |
| **Motion Constraints** | 选择顶点颜色流，为布料模拟应用每个顶点的动作约束数据。如果没有选择顶点颜色流，则会为布料网格中的所有顶点分配`1.0`动作约束值。 |
| **Motion Constraints Channel** | 选择顶点颜色流中包含动作限制数据的通道。 |
| **Backstop** | 选择顶点颜色流，为布料模拟应用每个顶点的逆止数据。如果没有选择顶点颜色流，布网格的逆止将被禁用。 |
| **Backstop Offset Channel** | 选择顶点颜色流中包含逆止偏移数据的通道。 |
| **Backstop Radius Channel** | 选择顶点颜色流中包含逆止半径数据的通道。 |

## Comment

![The Scene Settings Meshes tab Comment modifier.](/images/user-guide/assets/scene-settings/comment-modifier.png)

为网格组的文件添加注释。您可以添加对源资产文件所做更改的注释，以便跟踪或注释导出选项等。注释不会影响文件的处理方式。一个网格组可以添加多个注释修改器。

## Coordinate system change

坐标系更改修改器用于将三维场景源重新定向为开放三维引擎（O3DE）坐标系。大多数数字内容创建 (DCC) 应用程序使用右手坐标系，**Y** 为向上轴。O3DE 的坐标系是左手坐标系，**Z** 为向上轴。

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
| **Relative Origin Node** | 选择作为网格组原点的源资产节点。网格组将相对于所选节点进行变换。该节点不一定是网格组的一部分。一般情况下，这里会选择源资产的`RootNode`，因为它代表源资产的场景原点。不过，在某些情况下，最好是相对于源文件中不是`RootNode`的骨骼或网格进行变换。 |
| **Translation** | 设置产品资产的位置偏移。该属性最常用于使产品资产居中或与地面对齐。  |
| **Rotation** | 设置产品资产的方向偏移（单位：度）。该属性最常用于根据 O3DE 坐标系调整产品资产的方向。 |
| **Scale** | 设置产品资产的比例偏移。O3DE 的基本单位是一米。该属性最常用于对以厘米或英寸为基本单位的 DCC 应用程序中的产品资产进行统一缩放。 |

## Level of Detail (LOD)

![The Scene Settings Meshes tab Level of Detail modifier.](/images/user-guide/assets/scene-settings/mesh-lod-modifier.png)

LOD 是经过优化的网格，其多边形数量逐渐减少，纹理数量越来越少，尺寸越来越小，材质也越来越简化。实体离摄像机越远，组成实体的网格所需的细节就越少。当实体离摄像机越远，它就会切换到逐渐简化的 LOD。

通过[场景设置](/docs/user-guide/assets/scene-settings/scene-settings/)，您最多可以指定五个 LOD（不包括基础网格），它们的编号从\[`0`\]到\[`4`\]，其中\[`0`\]是**最高**的细节级别。LOD 不是必需的，但建议使用，因为它们有助于在不同硬件能力的平台上获得最佳性能和视觉保真度。

* 点击 {{< icon add.svg >}} **Add**按钮添加一个 LOD。

* 点击 {{< icon delete.svg >}} **Delete** 按钮移除一个 LOD。

* 点击 {{< icon browse-edit-select-files.svg >}} **Selection list** 按钮来指定要包含在 LOD 中的网格。

您还可以使用 “软命名约定 ”来定义优化网格名称，该约定定义了自动添加详细程度修改器并将网格分配到相应 LOD 插槽的命名规则。LOD 从 0（**最高**详细程度）开始排序，然后是逐渐**低**的详细程度，以满足 O3DE 应用程序的需要。

例如，您可以添加 `_lod0`、`_lod1`、`_lod2`、`_lod3`、`_lod4` 和 `_lod5`，作为网格名称的后缀。`_lod0` 是具有最高分辨率几何图形、纹理和材质的基本网格，会被分配到基本网格组。`_lod1` 分配到 LOD 插槽 **/[0\]**，`_lod2`分配到 LOD 插槽 **/[1\]**，以此类推。

场景设置流水线依靠[SoftNameSettings.setreg](https://github.com/o3de/o3de/blob/development/Gems/SceneProcessing/Registry/SoftNameSettings.setreg) 中定义的软命名约定设置规则来识别 LOD 网格。这些规则允许使用各种格式的名称后缀，包括 _LOD0、_LOD_0、Lod0、_Lod_0、_lod0 和 _lod_0（大写、小写、驼峰字体等）。

要覆盖这些默认设置，请使用项目用户注册表（`<project-root>/Registry`）或全局机器注册表（通常在用户主目录下的`.o3de/Registry/`），而不是直接修改 [SoftNameSettings.setreg](https://github.com/o3de/o3de/blob/development/Gems/SceneProcessing/Registry/SoftNameSettings.setreg)。有关为 O3DE 应用程序和工具提供设置和配置的更多信息，请查阅[设置注册表](/docs/user-guide/settings/) 。例如，您可以在`<project-root>/Registry` 下添加类似下面的新设置注册表文件，以**排除** LOD0 节点的子节点：
```
{
    "O3DE": {
        "AssetProcessor": {
            "SceneBuilder": {
                "NodeSoftNameSettings": [
                    {
                        "pattern": {
                            "pattern": "^.*_[Ll][Oo][Dd]_?1(_optimized)?$",
                            "matcher": 2
                        },
                        "virtualType": "LODMesh1",
                        "includeChildren": false
                    },
                    ...
                },
                "FileSoftNameSettings": [
                    ...
                ]
            }
        }
    }
}

```

{{< note >}}
您可以通过覆盖默认设置注册表来添加任意数量的软命名约定设置，但每个 O3DE 系统都有自己的最大 LOD 限制： [Atom 渲染器](/docs/atom-guide/) 最多支持 10 个 LOD，而每个角色最多可以有 [6](/docs/user-guide/visualization/animation/using-actor-lods-optimize-game-performance/#using-actor-lods-in-o3de)个 LOD。
{{< /note >}}

## Material

![The Scene Settings Meshes tab Material modifier.](/images/user-guide/assets/scene-settings/material-modifier.png)

材质修改器有助于在更新网格资产时自动管理与网格组相对应的`.material`文件内容。

材质是着色器和属性的组合，用于定义网格的表面。材质包含着色器和纹理分配，以及光滑度、不透明度或发射色等着色器属性的设置。

| 属性 | 说明 |
| - | - |
| **Update materials** | 激活后，`.material`文件中的纹理贴图文件名会更新，以匹配源资产文件中定义的纹理贴图名称。 |
| **Remove unused materials** | 激活后，将删除源资产文件中未定义的`.material`文件中的材质。 |

## Mesh (Advanced)

![The Scene Settings Meshes tab Mesh (Advanced) modifier.](/images/user-guide/assets/scene-settings/mesh-advanced-modifier.png)

网格（高级）修改器增加了网格合并和顶点颜色流选择等高级网格处理功能。

| 属性 | 说明 |
| - | - |
| **Vertex Precision** | 这是一个传统属性。默认情况下，所有网格都使用 32 位精度。 |
| **Merge Meshes** | 激活后，网格组中的所有网格会合并为一个网格进行优化。|
| **Use Custom Normals** | 激活时，将处理源资产中包含的自定义法线。否则，将生成平均法线。法线是定义网格每个顶点表面方向的顶点属性。可以自定义法线，使网格看起来有切面，在曲面之间创建硬边，或使曲面看起来更平滑。如果激活此选项，且源资产不包含自定义法线，则会自动生成平均法线。 |
| **Vertex color stream** | 如果此网格组的网格包含顶点颜色流，则可从此列表中选择要处理的顶点颜色流。顶点颜色流包含可被材质引用的每个顶点颜色数据。顶点颜色流还经常用于为网格标记任意数据，例如布料模拟中使用的反质量值。因此，一个网格可能有多个顶点颜色流。如果存在多个顶点颜色流，请务必选择一个可被材质引用的顶点颜色流作为颜色数据。 |

## Skin

![The Scene Settings Meshes tab Skin modifier.](/images/user-guide/assets/scene-settings/skin-modifier.png)

皮肤修改器会为绑定到骨架的网格设置处理皮肤权重的限制。

| 属性 | 说明 | 值 |
| - | - | - |
| **Max weights per vertex** | 每个顶点影响骨骼的最大数量。虽然该属性的上限允许每个顶点有 32 个骨骼，但在编写本文档时，每个顶点最多支持 4 个骨骼。如果一个顶点有 4 个以上的关节影响，且该属性的值大于 4，则产品资产只处理 4 个最大的顶点权重。 | `1` 到 `32` |
| **Weight threshold** | 产品资产中包含的骨骼影响的最小值。如果顶点蒙皮权重小于此值，则产品资产中不包含该顶点的蒙皮信息（顶点权重和相关骨骼索引）。例如，如果一个顶点有五个骨骼影响因素，其中三个骨骼影响因素的权重低于该权重阈值，则只有两个高于该权重阈值的骨骼影响因素才会包含在产品资产中。 | `0.0` 到 `0.01` |

## Tangents

![The Scene Settings Meshes tab Tangents modifier.](/images/user-guide/assets/scene-settings/tangents-modifier.png)

切线修改器可以从源资产中导入切线和位切线，也可以在资产处理过程中生成它们。切线和位切线是顶点属性，用于各种着色计算。切线和位切线对于蒙皮网格以及法线和浮雕贴图尤为重要。

| 属性 | 说明 |
| - | - |
| **Generation Method** | 切线和位切线可以从源资产(`From Source Scene`)导入，也可以使用`MikkT`算法自动生成。  |
| **TSpace Method** | `TSpaceBasic`方法可在顶点或像素级别生成与法线垂直的切线和位切线，其长度大小为单位。`TSpaceBasic` 方法适用于法线映射。`TSpace`方法以真实大小计算切线和位切线。计算出的切线和位切线可能不垂直于法线，但相互垂直。`TSpace`方法适用于浮雕贴图。 |

## UVs

![The Scene Settings Meshes tab UVs modifier.](/images/user-guide/assets/scene-settings/uvs-modifier.png)

UV 修改器可以从源资产中导入 UV 贴图，也可以在资产处理过程中生成 UV 贴图。UV 是顶点属性，用于控制纹理贴图在网格中特定区域的采样位置。UV 是切线和位切线生成正常运行所必需的。

需要注意的是，大多数 DCC 工具生成的 UV 设置都比使用此设置生成的 UV 设置要好得多，但在源数据中没有定义 UV，而您又希望无需在 DCC 工具中打开数据进行修改并提供 UV 即可渲染的情况下，此设置可能会很有用。

如果没有 UV 修改器，无论网格是否有 UV，默认情况下都不会对源场景做任何处理。

| 属性 | 说明 |
| - | - |
| **Generation Method** | UV 可以从源资产导入（“从源场景”），也可以使用球面定位投影(`Spherical Projection`)自动生成。|
| **Replace Existing UVs** | 如果关闭此选项，UVs 修改器将只为实际缺少 UVs 的网格生成 UVs，而不做其他操作。 如果打开此选项，即使场景中的网格已经有 DCC 工具生成的法线， **Generation Method** 也会应用。 |

