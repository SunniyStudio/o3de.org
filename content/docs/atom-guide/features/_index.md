---
title: "功能"
description: "了解 Atom Renderer（集成到 Open 3D Engine (O3DE) 中的图形引擎）的图形功能。"
toc: true
weight: 400
---

本节提供了**Atom 渲染器**支持的图形功能的高级概览。其中一些功能是在**Open 3D Engine (O3DE)** 中实现的，因此您可以在项目中添加渲染效果。


## Anti-aliasing抗锯齿
**锯齿**，即出现锯齿状边缘，是渲染图像中常见的问题。锯齿是将平滑曲线光栅化为像素的结果。有许多抗锯齿技术可以帮助缓解这一问题。Atom 实现了三种抗锯齿技术： 多采样抗锯齿（MSAA）、子像素形态抗锯齿（SMAA）和时态抗锯齿（TAA）。

Multisampling Anti-aliasing (MSAA)多采样抗锯齿 (MSAA)
: 这是一种基于超采样抗锯齿技术（SSAA）的抗锯齿技术，另一种技术是先以高分辨率渲染样本，然后再将样本降采样到低分辨率。MSAA 是 SSAA 的优化版本。SSAA 对每个像素进行采样，而 MSAA 则对像素组进行采样，从而有效减少了计算次数。MSAA 可提供最佳效果，但仍有较高的性能成本。在 Atom 中，MSAA 默认已启用。

Subpixel Morphological Anti-aliasing (SMAA)  子像素形态抗锯齿（SMAA）
: 一种基于图像的后处理抗锯齿技术，结合了形态学抗锯齿和多采样/超采样抗锯齿策略。它比 MSAA 更有效，但质量较低。您可以在文件 `SMAAConfiguration.azasset`中启用和配置 SMAA。

Temporal Anti-aliasing (TAA)时间抗锯齿 (TAA)
: 超采样的一种，每帧从像素的不同位置采集样本，然后与前一帧的样本相结合，生成超采样图像。您还可以使用对比度自适应锐化来减少 TAA 可能带来的模糊。更多信息，请参阅 [时域抗锯齿](taa/) 和 [对比度自适应锐化](cas/)。

## 光照

Punctual Lights 准时照明
: 准时光是一种高效、计算简单的光源。Atom 使用特征处理器接口将准时光集成到 O3DE 中。支持以下准时光：
+ **Point Light** - 从一个无限小的点向半径范围内的所有方向投射光线，类似于灯泡。
+ **Directional Light** - 从无限远的一点向单一方向投射光线。定向光常用来模拟太阳等远处的大光源，并投射阴影。

*关联：[O3DE Light component](/docs/user-guide/components/reference/atom/light/), [O3DE Directional Light component](/docs/user-guide/components/reference/atom/directional-light/), [Point Light Feature Processor API](/docs/api/gems/Atom/class_a_z_1_1_render_1_1_point_light_feature_processor_interface.html), [Directional Light Feature Processor API](/docs/api/gems/Atom/class_a_z_1_1_render_1_1_directional_light_feature_processor_interface.html)*


Area Lights
: 模拟从一个区域而不是从一个点或方向发出光线的光源。由于现实世界中的光源都是区域光源，因此这种光源对光线的描绘更为逼真。与点光源相比，区域光源需要更复杂的计算，渲染成本也更高。 

*关联: [O3DE Light component](/docs/user-guide/components/reference/atom/light/), [Disk Light Feature Processor API](/docs/api/gems/Atom/class_a_z_1_1_render_1_1_disk_light_feature_processor_interface.html),  [Capsule Light Feature Processor API](/docs/api/gems/Atom/class_a_z_1_1_render_1_1_capsule_light_feature_processor_interface.html), [Quad Light Feature Processor API](/docs/api/gems/Atom/class_a_z_1_1_render_1_1_quad_light_feature_processor_interface.html), [Polygon Light Feature Processor API](/docs/api/gems/Atom/class_a_z_1_1_render_1_1_polygon_light_feature_processor_interface.html)*


Clustered Forward Shading聚类正向阴影
: 一种光线剔除技术，通过剔除每个样本中的无效光源，优化光线计算量。聚类着色技术包括将场景中的样本分组，然后确定影响这些分组的光源。Atom 在前向着色过程中实现了聚类着色。


Diffuse Probe Grid扩散探头网格
: 在指定区域提供漫反射全局照明的光探头体积。该区域中的每个探针都使用光线追踪来捕捉其漫反射照明环境，即辐照度。  

*关联: [Diffuse Probe Grid component](/docs/user-guide/components/reference/atom/diffuse-probe-grid/), [Diffuse Probe Grid Feature Processor API](/docs/api/gems/diffuseprobegrid/class_a_z_1_1_render_1_1_diffuse_probe_grid_feature_processor_interface.html)*


Reflection Probes反射探头
: 为捕捉点（即探针）周围环境提供镜面反射的系统。 探针将其环境存储为一个立方体地图，并将立方体地图应用于位于探针体积内的网格。有了反射探针系统，网格可以显示与其所在位置周围环境相匹配的环境反射。  

*关联: [Reflection Probe component](/docs/user-guide/components/reference/atom/reflection-probe/), [Reflection Probe Feature Processor API](/docs/api/gems/Atom/class_a_z_1_1_render_1_1_reflection_probe_feature_processor_interface.html)*


 Subsurface Scattering 次表面散射
 : 描述进入半透明物体的光线如何通过材质发生散射。次表面散射是在材料属性中实现的。   

关联: [Material System](/docs/atom-guide/dev-guide/materials)


Exposure and Eye Adaptation 曝光与眼睛适应
: 描述场景亮度并与之互动的照明机制。

**Exposure control** 提供了一组调整场景曝光或光量的选项。您可以手动设置曝光，也可以使用Eye Adaptation模式自动设置曝光。曝光控制通过曝光控制组件集成到 O3DE 中，并由 Atom 通过一系列通道进行处理。

**Eye adaptation** 是一种自动调整场景光照强度的技术，模拟人眼从亮到暗或从暗到亮的调节能力。眼睛适应是在曝光控制功能中实现的。  

*关联: [Exposure Control component](/docs/user-guide/components/reference/atom/exposure-control/)*


Global Skylight (IBL) 全球天光 (IBL)
: 基于图像的全局照明效果，可使用 HDR 天光箱图像为场景计算照明。

*关联: [Global Skylight (IBL) component](/docs/user-guide/components/reference/atom/global-skylight-ibl)*


 HDRi Skybox   
: 使用 HDR 图像在场景中创建天空盒。

*关联: [HDRi Skybox component](/docs/user-guide/components/reference/atom/hdri-skybox/)*


 Decal   
: 投射到物体表面的非重复材料。贴花可用于涂抹各种独特的表面细节，包括油漆徽章、碎裂、污垢、锈迹等。

*关联: [Decal component](/docs/user-guide/components/reference/atom/decal/), [Decal Feature Processor API](/docs/api/gems/Atom/class_a_z_1_1_render_1_1_decal_feature_processor_interface.html)*


## Meshes 网格
Atom 具有统一的网格格式，支持多种网格类型：
+ **Static静态**网格是静止的。静态网格可以包含在光照贴图和反射贴图中。
+ 通过脚本或模拟动画的**Dynamic动态**网格。动态网格不能包含在光照贴图或反射贴图中。
+ **Skinned蒙皮**网格与骨架绑定，每个顶点都有骨骼权重，并通过应用于骨架骨骼的变换来实现动画效果。蒙皮网格不能包含在光贴图或反射贴图中。
+ **Cloth布**网格，可动态模拟类布物体的物理特性。布网格不能包含在光照贴图或反射贴图中。

*关联: [Mesh component](/docs/user-guide/components/reference/atom/mesh), [Mesh Feature Processor API](/docs/api/gems/Atom/class_a_z_1_1_render_1_1_mesh_feature_processor.html), [Skinned Mesh Feature Processor API](/docs/api/gems/Atom/class_a_z_1_1_render_1_1_skinned_mesh_feature_processor_interface.html)*  


## Post-processing effects (PostFX) 后期处理效果


Tonemapping  
: 将图像的色调值从高动态范围（HDR）转换为较低动态范围的过程。Atom 采用 16 位 HDR 帧缓冲区进行渲染，并应用色调映射，使渲染后的图像适合在各种数字屏幕技术上观看，从标准的 48 尼特 LDR 显示器（sRGB/rec.709）到各种 1000 尼特或更亮的 HDR 显示器类型。在 O3DE 中，**Display Mapper**组件可配置渲染器如何执行色调映射。

Atom 的默认色调映射是 [ACES](https://acescentral.com/),，它通过保持艺术完整性和保真度来简化和标准化色彩管理。它允许从渲染器到显示屏以及在许多支持 ACES 的现代 DCC 应用程序中以最佳方式再现色彩。

*关联: [Display Mapper component](/docs/user-guide/components/reference/atom/display-mapper/)*


Color Grading 颜色分级
: 改变图像视觉外观的过程。这种技术可以在不同设备上呈现不同环境下的图像，或增强艺术风格。调色可以增强图像的各种属性，如对比度、色彩、饱和度、细节、黑度和白点。调色和色调映射通常配合使用，以控制在不同设备上显示的图像的色彩和色调。在 O3DE 中，您可以通过 PostFX 系统在渲染管道中执行 HDR 调色，通过 **Look Modification** 组件使用色彩查找表（LUT），或通过 Display Mapper 组件在最终显示输出上应用单一的全局 LDR 调色。

*关联: [Look Modification component](docs/user-guide/components/reference/atom/look-modification.md), [Display Mapper component](/docs/user-guide/components/reference/atom/display-mapper/)*

Post-processing Volumes 后期处理体积
: O3DE 中的各种机制通过形状、梯度信号或半径对体积或区域内的每个图层进行加权，从而控制 PostFX 图层的混合。

- **PostFX Radius Weight Modifier**组件根据距离设置简单的混合。
  
- **PostFX Shape Weight Modifier** 组件和 Shape 组件根据音量和衰减设置混合效果。

- **PostFX Gradient Weight Modifier** 组件通过使用渐变信号作为遮蔽操作，设置了一种更复杂的混合效果。
 
*关联: PostFX 组件 ([PostFX Layer](/docs/user-guide/components/reference/atom/postfx-layer/), [PostFX Gradient Weight](/docs/user-guide/components/reference/atom/postfx-gradient-weight-modifier/), [PostFX Shape Weight Modifier](/docs/user-guide/components/reference/atom/postfx-shape-weight-modifier/), [PostFX Radius Weight Modifier](/docs/user-guide/components/reference/atom/postfx-radius-weight-modifier/)), [Post Process Feature Processor API](/docs/api/gems/Atom/class_a_z_1_1_render_1_1_post_process_feature_processor_interface.html)*


Bloom  
: 一种后期处理效果，可模拟真实世界中因光线过强而产生的发光或光斑。

*关联: [Bloom component](/docs/user-guide/components/reference/atom/bloom/)* 

Chromatic Aberration   
: 一种后期处理效果，模拟镜头将不同波长的光线聚焦在不同点上，在图像边缘产生色彩条纹。

*关联: [Chromatic Aberration component](/docs/user-guide/components/reference/atom/chromatic-aberration/)* 

Deferred Fog   
: 一种在场景中创建体积雾的后期处理效果。延迟雾是利用动态八度噪音计算出的雾，并通过光线行进来渲染雾。

*关联: [Deferred Fog component](/docs/user-guide/components/reference/atom/deferred-fog/)* 


Depth of Field   
: 这是一种后期处理效果，它使用焦点和距离来模拟现实世界中相机聚焦于特定区域的镜头效果。Atom 通过着色器处理景深效果。

*关联: [Depth of Field component](/docs/user-guide/components/reference/atom/depth-of-field/)* 


Screen Space Ambient Occlusion (SSAO)   
: 实时估算环境遮挡作为屏幕空间的后处理效果。SSAO 可遮挡被附近表面遮挡的区域，使其无法接收环境光。这种效果在表面的裂缝、缝隙和折痕中产生的阴影中最为明显。

*关联: [SSAO component](/docs/user-guide/components/reference/atom/ssao/)* 


## Rendering
Motion Vectors   
: 描述物体在特定时间运动的计算。运动矢量是通过计算运动的传递来实现的，分为两个部分：摄像机运动和物体运动。摄像机运动通道记录由摄像机运动引起的剪辑空间运动。物体运动通路记录由摄像机移动和物体变换引起的剪辑空间运动。运动矢量是计算运动模糊和时间抗锯齿 (TAA) 等效果所必需的。


Multi-scene   
: 支持多场景渲染。在 Atom 示例查看器中可以找到实现该功能的示例。

Multi-render Pipeline   
: 支持在一个场景中使用多个渲染管道。多重渲染管道允许你在运行时切换渲染管道。在 Atom 示例查看器中可以找到这一实现的示例。
