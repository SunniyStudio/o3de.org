---
linkTitle: 创建地形资产
title: 创建地形资产
description: 创建本教程所需的地形资源。
weight: 200
toc: true
---

在本教程小节中，您将创建在关卡中编写示例地形所需的地形资源。

## 创建曲面标签名称列表

[_Surface tag names_](/docs/user-guide/gems/reference/environment/surface-data)是可用于引用与特定渐变关联的地形部分的字符串。这些标签将用于描述 terrain 的哪些部分是草地表面，哪些部分是岩石表面。

1. 从**Tools**菜单，选择**Asset Editor**。

2. 在 Asset Editor 中，选择 **File → New → Surface Tag Name List**。

3. 在曲面标记名称列表中，单击{{< icon "add.svg" >}} **Add** 按钮以添加标签名称。

4. 为标签名称提供`tutorial_grass`。此标签将用于草地表面。

5. 点击 {{< icon "add.svg" >}} **Add** 按钮以添加第二个标签名称，并将其命名为`tutorial_rock`此标签将用于岩石表面。

6. 将表面标签名称列表保存到教程关卡目录中，名称为 `tutorial_tags.surfaceTagNameList` (快捷键 **Ctrl + S**).

您可以为本教程之外要创建的任何其他表面类型添加更多标签。

## 创建地形源图像资产

要完成本教程，您可以使用以下示例图像，也可以使用您自己的图像。要使用这些图像，请右键单击每个图像，选择 `Save Image As...`，然后将图像保存到包含教程关卡的文件夹中。

| 高度贴图 |宏颜色 |表面权重遮罩 |
| - | - | - |
| {{< image-width src="/images/learning-guide/tutorials/environments/terrain-from-images/tutorial_terrain_heightmap_gsi.png" width="256" alt="An example heightmap image." >}} | {{< image-width src="/images/learning-guide/tutorials/environments/terrain-from-images/tutorial_terrain_basecolor.png" width="256" alt="An example macro color image." >}} | {{< image-width src="/images/learning-guide/tutorials/environments/terrain-from-images/tutorial_grass_splatmap_gsi.png" width="256" alt="An example surface weight mask image." >}} |

高度贴图是一种图像，其中深色值表示低海拔区域，而浅色值表示高海拔区域。通过将高度贴图分配给 Image Gradients（图像渐变），可以将高度贴图用于地形。要将高度贴图与图像渐变一起使用，必须由[Asset Processor](/docs/user-guide/assets/asset-processor/)  将其处理为 *渐变信号图像 （GSI）*，以防止纹理处理应用颜色校正或有损压缩。要确保正确处理高度贴图，请牢记以下建议：

* 建议使用 16 位颜色深度。8 位图像可能无法提供足够的渐变步长来表示海拔高度，并且 32 位图像可能会不必要地大。16 位图像最多可提供 65,536 个高程步长。

* 对 16 位高度图图像使用`.png` 或 `.tif`格式。

* 对 32 位高度图图像使用`.tif`格式。

* 在高度贴图文件名后加上`_gsi`，以便 Asset Processor 将高度贴图作为 GSI（例如，`LevelOneTerrainHeight_gsi.png`）自动处理。如果不使用`_gsi`后缀，可以使用 [Texture Settings](/docs/user-guide/assets/texture-settings/) 将高度贴图源资源配置为作为 GSI 处理。

{{< tip >}}
O3DE 中地形的默认 **eight Query Resolution** 为 1.0 米。这意味着 1000 x 1000 高度贴图表示一平方公里的地形。可以在 [**Terrain World**](/docs/user-guide/components/reference/terrain/world) 关卡组件中更改 **Height Query Resolution**。
{{< /tip >}}

宏观颜色纹理是为地形提供低保真颜色变化的图像。可以使用任何纹理格式，但名称应具有 `_basecolor`后缀，以确保将其作为颜色纹理处理。

表面权重蒙版是深色值为低表面权重，而亮值为高表面权重的图像。表面权重蒙版图像也用于图像渐变，因此它们也应以`_gsi`为后缀。精度在表面权重蒙版中并不那么明显，因此 8 位图像几乎总是足够的。

## 地形材质

地形需要两种类型的材质，[*宏材质*](/docs/user-guide/components/reference/terrain/terrain-macro-material) 和 [*详细材质*](/docs/user-guide/components/reference/terrain/terrain-detail-material)。

宏材质是适用于整个地形的基本材质。它支持 simple color texture 和 normal texture。这些纹理在与细节材质混合的整个地形中提供低保真颜色和法线信息。宏材质是在距摄像机的距离大于 **Detail material render distance** 时显示的唯一材质。

细节材质是您可以使用 [材质编辑器](/docs/atom-guide/look-dev/tools/material-editor/) 创建的标准材质。它们通过表面标签名称分配给地形，其权重值由坡度生成。这允许您在大型地形表面分配许多材质并在它们之间进行混合。细节材质显示在距摄像机的 **Detail material render distance** 内的地形上。

这种细节材质与宏材质的混合使您能够通过在整个地形中创建颜色和光照的变化来使用小型、高保真、平铺的细节材质。在以下示例中，左侧的草地是未与宏材质混合的细节材质。右侧的草地是相同的细节材质，但它已混合在低保真微距材质上。请注意颜色和光照的变化，这些变化会产生不太均匀但更自然的外观：

{{< image-width src="/images/learning-guide/tutorials/environments/terrain-from-images/macro-material-blending.png" width="800" alt="An example of the difference in variation created by macro material blending." >}}

{{< tip >}}
在 **Terrain World Renderer** 关卡组件中，有一个名为 **Detail material configuration** 的属性组，可用于配置从摄像机停止将细节材质混合到宏材质中的距离，以及淡出混合细节材质的距离。
{{< /tip >}}

以下示例图像演示了各种材质分配以及它们在 terrain 表面上的交互方式：

{{< image-width src="/images/learning-guide/tutorials/environments/terrain-from-images/terrain-materials-example.png" width="800" alt="An example of macro and detail materials applied to a terrain." >}}

在此示例中，有四种材质：

* 洋红色 - 这是宏材质。它显示在距离摄像机 512 米的地形上。此微距材质与细节材质之间的混合距离为 64 米。

* 绿色 - 这是默认的细节材质。细节材质是分层的，默认材质是基础层。默认材质显示在未通过曲面标记名称指定细节材质但靠近摄像机的区域上。

* 红色 - 这是细节材质。它通过表面标签名称应用于地形。表面标签名称由平铺图像渐变生成，该渐变在黑色背景上包含一个白色三角形。

* 蓝色 - 这是一种细节材质。它通过表面标签名称应用于地形。表面标签名称由平铺图像渐变生成，该渐变在黑色背景上包含一个白色圆圈。

{{< note >}}
请注意，红色和蓝色细节材质在三角形和圆形重叠的区域混合为紫色。这是因为图像渐变正在为这些重叠区域中的两种材质生成表面标签。
{{< /note >}}

### 创建地形细节材质

以下步骤是 [创建 StandardPBR 材质](/docs/learning-guide/tutorials/rendering/create-standardpbr-material)教程的略微修改版本，因此最终结果是 [地形细节材质](/docs/user-guide/components/reference/terrain/terrain-detail-material)  而不是 StandardPBR 材质。您可能希望先熟悉该教程，因为此处的步骤较短且包含的解释较少。

1. 下载纹理文件。本教程使用 [Poly Haven](https://polyhaven.com/) 中的两个现成材质：[Forest Ground 01](https://polyhaven.com/a/forrest_ground_01)和 [Rock Boulder Dry](https://polyhaven.com/a/rock_boulder_dry)。您可以以任何分辨率下载它们;本教程中的示例图像使用 4K 分辨率。要下载所需的图像，请单击 下载 按钮旁边的汉堡按钮，然后选择要使用的扩展，然后单击 AO、漫反射、置换、法线和粗糙贴图旁边的该扩展的按钮。将文件保存到教程关卡文件夹中，并按如下方式重命名它们：

    * forrest_ground_01_**ao**\_4k &rarr; forrest_ground_01\_**ao**
    * forrest_ground_01_**diff**\_4k &rarr; forrest_ground_01\_**basecolor**
    * forrest_ground_01_**disp**\_4k &rarr; forrest_ground_01\_**displ**
    * forrest_ground_01_**nor**\_gl_4k &rarr; forrest_ground_01\_**normal**
    * forrest_ground_01_**rough**\_4k &rarr; forrest_ground_01\_**roughness**
    * rock_boulder_dry_**ao**\_4k &rarr; rock_boulder_dry\_**ao**
    * rock_boulder_dry_**diff**\_4k &rarr; rock_boulder_dry\_**basecolor**
    * rock_boulder_dry_**disp**\_4k &rarr; rock_boulder_dry\_**displ**
    * rock_boulder_dry_**nor**\_gl_4k &rarr; rock_boulder_dry\_**normal**
    * rock_boulder_dry_**rough**\_4k &rarr; rock_boulder_dry\_**roughness**

2. 使用 Material Editor 创建 Terrain Detail 材质。打开材质编辑器，选择 File -> New Material Document...，然后创建一个 `Terrain Detail Material`。注意，您在这里没有选择`Standard PBR`，因为您将需要`Terrain Detail Material`上可用的其他地形纹理控件。将材质命名为 `forrest_ground_01.material`并将其保存在教程关卡文件夹中。

3. 为 **Base Color**下的**Texture**选择`forrest_ground_01_basecolor`，为**Roughness**下的**Texture**选择`forrest_ground_01_roughness`，为**Normal**下的**Texture**选择`forrest_ground_01_normal`，为**Occlusion**下的**Diffuse AO**选择`forrest_ground_01_ao`，为**Displacement**下的**Height Map**选择`forrest_ground_01_displ`。

4. 默认情况下，Poly Haven 中的法线贴图需要翻转其法线，因此在**Normal**下启用 **Flip Y Channel**。

5. 在 **Base Color** 下，将 **Texture Blend Mode** 设置为 `Lerp`，将 **Factor** 设置为`0.5`。对于地形细节材质，您通常使用`LinearLight`的 **Texture Blend Mode** 来混合高频细节材质和低频宏材质。有关更多详细信息，请参阅[Terrain Detail Material](/docs/user-guide/components/reference/terrain/terrain-detail-material)和 [了解频率分离](/docs/learning-guide/tutorials/environments/understanding-frequency-separation) 教程。不太常见的情况是，您可以选择使用`Lerp`的 **Texture Blend Mode** 和 **Factor** `0.0`，以便仅从远距离的宏材质混合到仅近距离的细节材质。这用于细节材质包含全色信息且宏材质是细节材质的低分辨率合成，而不是用于颜色变化的单独纹理的工作流程。为简单起见，本教程使用第三种技术通过混合实现颜色变化。将 **Texture Blend Mode** 设置为 `Lerp` 且 **Factor** 为 `0.5` 将在宏材质和细节材质之间执行 50% 的 alpha 混合。这将产生具有一些颜色变化的混合结果。与使用 **Factor** 为 `0.0` 的 `Lerp` 相比，Alpha 混合足够明显，可以产生更多变化，但也不如使用频率分离工作流程那么生动和详细。下图显示了不同 `Lerp` 因子的效果，以帮助可视化混合如何改变输出。

    | Base Macro Color | Detail Lerp 1.0 | Detail Lerp 0.5 | Detail Lerp 0.0 |
    | - | - | - | - |
    | {{< image-width src="/images/learning-guide/tutorials/environments/terrain-from-images/terrain-macro-only.png" alt="Comparison image showing only the macro material." >}} | {{< image-width src="/images/learning-guide/tutorials/environments/terrain-from-images/terrain-macro-lerp-1.png" alt="Comparison image showing the macro material plus a detail material with Lerp 1.0." >}} | {{< image-width src="/images/learning-guide/tutorials/environments/terrain-from-images/terrain-macro-lerp-0.5.png" alt="Comparison image showing the macro material plus a detail material with Lerp 0.5." >}} | {{< image-width src="/images/learning-guide/tutorials/environments/terrain-from-images/terrain-macro-lerp-0.png" alt="Comparison image showing the macro material plus a detail material with Lerp 0.0." >}} |

6. 放大纹理。这纯粹是一种审美选择，它有助于纹理中的细节变得更加明显。将 **UVs** 下的 **Tile U** 和 **Tile V** 设置为 0.25，使纹理比默认值大 4 倍。

7. 设置地形高度混合参数。在 **Terrain** 下，启用 **Override parallax settings**，将 **Height offset** 设置为 0.25，将 **Height scale** 设置为 0.5，将 **Blend factor** 设置为 0.5，并将 **Weight clamp factor** 设置为 0.2。这些值根据美学进行调整，它们控制在启用基于高度的混合时细节材质如何与其他细节材质混合。

8. 再次按照步骤 2-7 操作，使用下载的`rock_boulder_dry`纹理创建名为 `rock_boulder_dry.material`的材质。对于此材质，在执行步骤 7 时，将 **Tile U** 和 **Tile V** 设置为 0.125，使此材质比默认值大 8 倍，并将 **Height offset** 设置为 0.0，将 **Height scale** 设置为 1.0，将 **Blend factor** 设置为 0.2，将 **Weight clamp factor** 设置为 0.2。再一次，这些值是根据这种材料的美学来选择的。
