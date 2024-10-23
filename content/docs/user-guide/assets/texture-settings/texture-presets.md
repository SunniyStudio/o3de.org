---
linkTitle: 纹理预设
title: 纹理设置预设
description: 指定如何使用纹理预设为 Open 3D Engine (O3DE) 自动处理纹理源资产。
weight: 400
toc: true
---

下表提供了可用纹理处理预设的信息。您可以在纹理源资产名称后附加**Name Conventions名称约定**列中的字符串，以自动使用相应预置处理纹理。

| 预设 | 说明 | 源文件要求 | 像素格式 | 名称约定 |
|-|-|-|-|-|
| Albedo | 颜色纹理，包含未发光的表面颜色。 | RGB 图像。如果图像有 Alpha 通道，则在处理图像时将其丢弃。| 桌面电脑: BC1<br>iOS: ASTC_6x6<br>Android: ASTC_4x4 | `_alb`<br>`_albedo`<br>`_basecolor`<br>`_bc`<br>`_col`<br>`_color`<br>`_diff`<br>`_diffuse`<br> |
| AlbedoWithCoverage | 带有 1 位 alpha 通道的彩色纹理，包含非光照表面颜色。 | RGBA 图像。无论纹理源资产位深度如何，alpha 通道都是以 1 位处理的。 | 桌面电脑: BC1a<br>iOS: ASTC_6x6<br>Android: ASTC_4x4 | `_albc`<br>`_albedoc`<br>`_basecolorc`<br>`_bcc`<br>`_colc`<br>`_colorc`<br>`_diffc`<br>`_diffusec`<br> |
| AlbedoWithGenericAlpha | 带有 8 位 alpha 通道的彩色纹理，包含非光照表面颜色。 | RGBA 图像。 | 桌面电脑: BC3<br>iOS: ASTC_6x6<br>Android: ASTC_4x4 | `_alba`<br>`_albedoa`<br>`_basecolora`<br>`_bca`<br>`_cola`<br>`_colora`<br>`_diffa`<br>`_diffusea`<br> |
| AmbientOcclusion | 线性灰度纹理，包含环境光照被遮挡的表面区域（如褶皱和折痕）的阴影值。 | 单通道灰度图像。 | 桌面电脑: BC4<br>移动平台: ASTC_4x4 | `_amb`<br>`_ambientocclusion`<br>`_ambocc`<br>`_ao`<br> |
| ConvolvedCubemap | 用于快速照明计算的立方体贴图纹理。该预设的分辨率为 256 x 256。 | HDR 球形环境地图或立方体地图。 | R9G9B9E5 | `_ccm`<br>`_convolvedcubemap`<br> |
| Decal_AlbedoWithOpacity | 高质量彩色纹理，带有 Alpha 通道，可投射到表面上。 | RGBA 图像。 | 桌面电脑: BC7t<br>移动平台: ASTC_4x4 | `_decal` |
| Displacement | 包含表面位移或高度值的线性灰度纹理。 | 单通道灰度图像。 | 桌面电脑: BC4<br>移动平台: ASTC_4x4 | `_d`<br>`_disp`<br>`_displ`<br>`_displacement`<br>`_dm`<br>`_dsp`<br>`_h`<br>`_height`<br>`_hm`<br>`_ht`<br> |
| Emissive | 一种高品质的彩色纹理，用于界定表面发光或发亮的区域。 | RGB 图像。 | 桌面电脑: BC7<br>iOS: ASTC_6x6<br>Android: ASTC_4x4 | `_e`<br>`_em`<br>`_emissive`<br>`_emit`<br>`_glow`<br> |
| GSI (Gradient Signal Image) | 带有 alpha 的颜色纹理，包含数值渐变的梯度。 | RGBA 图像。 | 与输入像素格式相同。 | `_gsi` |
| GSI8 | 纹理只有一个 8 位通道（红色），包含数值渐变的梯度。 | 单通道图像或 RGB 图像。 | R8 | `_gsi8` |
| GSI16 | 纹理只有一个 16 位通道（红色），包含数值渐变的梯度。 | 单通道图像或 RGB 图像。| R16 | `_gsi16` |
| GSI32 | 纹理只有一个 32 位通道（红色），包含一个渐变的渐变值。 | 单通道图像或 RGB 图像。 | R32 | `_gsi32` |
| Gradient | 带有 alpha 的颜色纹理，包含数值渐变的梯度。 | RGBA 图像。 | R8G8B8A8 | `_grad`<br>`_gradient` |
| Greyscale | 线性灰度纹理。 | 单通道灰度图像。 | 桌面电脑: BC4<br>移动平台: ASTC_4x4 | `_mask`<br> |
| IBLDiffuse | 包含 IBL 色彩组件的 HDR 立方体地图。 | HDR 球形环境地图或立方体图。 | Windows: BC6UH<br>Linux: BC6UH<br>macOS: R9G9B9E5<br>iOS: R9G9B9E5<br>Android: R9G9B9E5 | `_ibldiffusecm` |
| IBLGlobal | 包含 IBL 的镜面反射和漫反射成分的 HDR 立方体贴图。| HDR 球形环境贴图或立方体贴图。 | R8G8B8A8 | `_cm`<br>`_cubemap`<br>`_iblglobalcm`<br> |
| IBLSkybox | HDR 立方体贴图，包含 IBL 的天空盒纹理、漫反射和镜面反射组件。| HDR 球形环境地图或立方体图。 | R9G9B9E5 | `_iblskyboxcm` |
| IBLSpecular | 256 x 256 像素的 HDR 立方地图，包含 IBL 的镜面反射成分。 | HDR 球形环境地图或立方体图。 | R9G9B9E5 | `_iblspecularcm`<br>`_iblspecularcm256`<br> |
| IBLSpecularHigh | 一个 512 x 512 像素的 HDR 立方地图，包含 IBL 的镜面反射成分。 | HDR 球形环境地图或立方体图。 | R9G9B9E5 | `_iblspecularcm512`<br> |
| IBLSpecularLow | 128 x 128 像素的 HDR 立方地图，包含 IBL 的镜面反射成分。| HDR 球形环境地图或立方体图。 | R9G9B9E5 | `_iblspecularcm128`<br> |
| IBLSpecularVeryHigh | 一个 1024 x 1024 像素的 HDR 立方地图，包含 IBL 的镜面反射成分。 | HDR 球形环境地图或立方体图。 | R9G9B9E5 | `_iblspecularcm1024`<br> |
| IBLSpecularVeryLow | 一个 64 x 64 像素的 HDR 立方地图，包含 IBL 的镜面反射成分。 | HDR 球形环境地图或立方体图。 | R9G9B9E5 | `_iblspecularcm64`<br> |
| LUT_R32F | 具有 32 位浮点红色通道的查找表（LUT）纹理。 | 具有 32 位浮点红色通道的 RGB 图像。 | R32F | `_lutr32f` |
| LUT_RG16 | 带有 16 位红色和绿色通道的 LUT 纹理。| 具有 16 位红色和绿色通道的 RGB 图像。 | R16G16F | `_lutrg16` |
| LUT_RG32F | 带有 32 位浮点红色和绿色通道的 LUT 纹理。 | 具有 32 位浮点红色和绿色通道的 RGB 图像。 | R32G32F | `_lutrg32f` |
| LUT_RG8 | 带有 8 位红色和绿色通道的 LUT 纹理。| 具有 8 位红色和绿色通道的 RGB 图像。 | R8G8 | `_lut` |
| LUT_RGBA16 | 具有 16 位通道的 LUT 纹理。 | 具有 16 位通道的 RGBA 图像。 | R16G16B16A16 | `_lutrgba16` |
| LUT_RGBA16F | 具有 16 位浮点通道的 LUT 纹理。| 具有 16 位浮点通道的 RGBA 图像。 | R16G16B16A16F | `_lutrgba16f` |
| LUT_RGBA32F | 具有 32 位浮点通道的 LUT 纹理。| 具有 32 位浮点通道的 RGBA 图像。 | R32G32B32A32F | `_lutrgba32f` |
| LUT_RGBA8 | 具有 8 位通道的 LUT 纹理。 | 具有 8 位通道的 RGBA 图像。 | R8G8B8A8 | `_lutrgba8` |
| LayerMask | 颜色遮罩纹理，有 8 位颜色通道和额外的 8 位预留通道。 | RGB 图像。如果图像有 Alpha 通道，则在处理图像时将其丢弃。 | R8G8B8X8 | `layers_rgbmask` |
| Normals | 包含切线空间表面法线的线性彩色纹理。 | RGB 图像，切线空间表面法线在 RGB 通道中编码。  | 桌面电脑: BC5s<br>移动平台: ASTC_4x4 | `_ddn`<br>`_n`<br>`_nm`<br>`_nor`<br>`_norm`<br>`_normal`<br>`_normalmap`<br>`_normals`<br>`_nrm`<br> |
| NormalsWithSmoothness | 线性彩色纹理，在颜色通道中包含切线空间表面法线，在 Alpha 通道中包含平滑或光泽值。 | RGBA 图像，切线空间表面法线在 RGB 通道中编码，光泽度或平滑值在 alpha 通道中编码。 | 桌面电脑: BC5s<br>移动平台: ASTC_4x4 | `_ddna`<br>`_na`<br>`_nma`<br>`_normala`<br>`_nrma`<br> |
| Opacity | 包含不透明度值的线性灰度图像。 | 单通道灰度图像。 | 桌面电脑: BC4<br>移动平台: ASTC_4x4 | `_blend`<br>`_mask`<br>`_msk`<br>`_o`<br>`_op`<br>`_opac`<br>`_opacity`<br>`_sss`<br>`_trans`<br> |
| ReferenceImage | 带 Alpha 的未压缩色彩参考纹理。 | RGBA 图像。 | R8G8B8A8 | `_ref` |
| ReferenceImage HDRLinear | HDR 线性色彩参考纹理。 | 16 位通道的 RGB 图像. | Windows, Linux: BC6UH<br>macOS, iOS, Android: R9G9B9E5 | `_refhdr`  |
| ReferenceImage HDRLinearUncompressed | 未经压缩的 HDR 线性色彩参考纹理。 | 具有 16 位浮点通道的 RGB 图像。 | R16G16B16A16F | `_refhdru` |
| ReferenceImage Linear | 带 Alpha 的未压缩线性色彩参考纹理。 | RGBA 图像。 | R8G8B8A8 | `_reflinear` |
| Reflectance | 包含反射率值的线性灰度图像。| 单通道灰度图像。 | 桌面电脑: BC4<br>iOS: ASTC_6x6<br>Android: ASTC_4x4 | `_f0`<br>`_g`<br>`_gloss`<br>`_m`<br>`_metal`<br>`_metallic`<br>`_metalness`<br>`_mt`<br>`_mtl`<br>`_ref`<br> |
| Skybox | 最小尺寸为 256 x 256 像素的 HDR 天空方块图。 | HDR 球形环境地图或立方体图。 如果图像有 Alpha 通道，则在处理图像时将其丢弃。 | R9G9B9E5 | `_skyboxcm` |
| UserInterface Compressed | 压缩的彩色纹理，包含带有 8 位 alpha 通道的用户界面元素。 | RGBA 图像。 | Windows, Linux: R8G8B8A8<br>macOS: BC1<br>iOS: ASTC_6x6<br>Android: ASTC_4x4  | `_ui` |
| UserInterface Lossless | 一种未压缩的线性彩色纹理，包含一个带有 8 位 alpha 通道的用户界面元素。 | RGBA 图像。 | R8G8B8A8 | `_uil` |
