---
title: "创建 StandardPBR 材质"
description: "本教程将指导您了解如何在 Atom 中创建 StandardPBR 材质。"
toc: false
---

本教程将指导您了解如何在 Atom 中创建 StandardPBR 材质。StandardPBR 材质类型允许您创建对艺术家友好、功能齐全的基于物理的渲染 （PBR） 材质。

本教程涵盖以下概念：
- StandardPBR 材质的元素
- 使用 Material Editor 创建材质
- 使用纹理文件
- 使用纹理文件蒙版

## 开始之前
材质可以具有多个纹理，例如基础颜色、法线贴图和粗糙度。这些纹理包含不同的数据，这些数据组合在一起以创建材质的整体外观。您可以使用外部工具（如 [Substance Designer](https://www.adobe.com/products/substance3d-designer.html) 和 [Materialize](http://boundingboxsoftware.com/materialize/)设计材质的纹理。在本教程中，我们使用来自 [Poly Haven](https://polyhaven.com/)的现成材质。

### 下载纹理文件
从 Poly Haven 下载任何分辨率的 [城堡砖材料](https://polyhaven.com/a/castle_brick_02_red)。要下载所需的贴图，请单击 **下载** 按钮旁边的汉堡按钮，然后选择要使用的扩展，然后单击 AO、漫反射、置换、法线和粗糙贴图旁边的该扩展按钮。 如果文件未自动下载，请将文件保存到磁盘。

下载后，将地图移动到 `<project-folder>/Materials/PolyHaven/`文件夹中。

我们使用以下文件作为输入纹理:
-  castle_brick_02_red_ao_1k (ambient occlusion)
-  castle_brick_02_red_diff_1k (diffuse, base color)
-  castle_brick_02_red_disp_1k (displacement)
-  castle_brick_02_red_nor_gl_1k (normal)
-  castle_brick_02_red_rough_1k (roughness)

### 将文件掩码应用于纹理文件
要确保这些纹理文件正常工作，您必须指示 Asset Processor 应使用哪个纹理预设来处理这些文件。最简单的方法是重命名文件以使用纹理文件蒙版。**纹理文件蒙版** 是文件名末尾的后缀，指示要使用的纹理预设。这允许 Asset Processor 将此图像类型正确转换为其运行时格式。

纹理文件蒙版有多种变体可用于单个纹理。例如，“_ao”或“_ambientocclusion”都有效，并且 Asset Processor 知道将纹理作为环境光遮蔽纹理处理。这些纹理文件蒙版在配置 Atom 图像处理器的预设 （`*.preset`） 文件中定义。预设文件可以在文件夹中找到  `/Gems/Atom/Asset/ImageProcessingAtom/Assets/Config/`. 

我们重命名以下文件以正确应用文件掩码。（为清楚起见，我们使用描述性纹理文件蒙版名称。
- castle_brick_02_red_**ao**\_1k &rarr; castle_brick_02_red\_**ambientocclusion**
- castle_brick_02_red_**diff**\_1k &rarr; castle_brick_02_red\_**basecolor**
- castle_brick_02_red_**disp**\_1k &rarr; castle_brick_02_red\_**displ**
- castle_brick_02_red_**nor**\_gl_1k &rarr; castle_brick_02_red\_**normal**
- castle_brick_02_red_**rough**\_1k &rarr; castle_brick_02_red\_**roughness**
  

## 使用材质编辑器创建材质
要使用 Material Editor 创建材质，请执行以下操作：
1. 打开 Material Editor。如果您在 Open 3D Engine Editor 中，请转到 *Tools > Material Editor*，或按 *M*。否则，您可以将 Material Editor 作为独立应用程序打开，从 `<build_folder>/bin/profile/MaterialEditor.exe` 或 `<install>/bin/<platform>/profile/Default/MaterialEditor.exe`.

2. 通过选择 **File** > **New** > **Standard PBR** 创建新的 StandardPBR 材质。这将打开文件浏览器并提示您保存新文件。在 **Inspector** 选项卡中，您可以通过检查 `Details` 属性组中的 `Material Type` 属性来验证材质类型是否为 `StandardPBR`。

   {{< note >}} 
文件浏览器在项目文件夹、任何 Gem 的 *Assets* 文件夹或 AssetProcessorPlatformConfig.ini 中包含的任何其他文件夹中查找材质。
    {{< /note >}}

1. 浏览每个纹理文件并将其加载到纹理的关联属性组下的 `Texture Map` 属性中。根据属性组，可能会显示其他属性，以便您可以进一步配置属性组。

    纹理将按以下方式加载到其关联的属性组中。
    
   - **Base Color**: castle_brick_02_red_basecolor
   - **Roughness**: castle_brick_02_red_roughness
   - **Normal**: castle_brick_02_red_normal
   - **Occlusion**: castle_brick_02_red_ambientocclusion
   - **Displacement**: castle_brick_02_red_displ 
    
    {{< note >}} 
下载的法线贴图被翻转。要将其反转，请启用 `Flip Y Channel` 属性。
    {{< /note >}}

您已成功创建新的 StandardPBR 材质！下图显示了属性设置和预期的材料。

![Creating a StandardPBR material using the Material Editor](/images/learning-guide/tutorials/rendering/create-standardpbr-material.png)
