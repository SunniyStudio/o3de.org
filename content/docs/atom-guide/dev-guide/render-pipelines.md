---
linkTitle: 渲染管道
title: Atom 渲染器中的渲染管道
description: 您可以在 Atom Renderer（集成到 Open 3D Engine (O3DE) 中的图形引擎）中使用预置的渲染管道或创建自定义管道。
toc: false
weight: 150
---

**渲染管道**描述了在二维屏幕上渲染三维场景的一系列步骤。在**Atom Renderer**中，渲染管道是完全模块化的，由一系列通道驱动。这样，你就可以针对阴影、光照和后期特效等功能定制渲染管道。你还可以完全改变渲染技术，例如延迟、棋盘格或前向+。默认情况下，Atom 在其预置的渲染管道中实现了 *Forward+* 渲染。

Atom 的预建渲染管道可随时使用，并包含**Open 3D Engine (O3DE)** 的常见用例：
- **主渲染管道**： Windows、Linux 和 macOS 平台的默认渲染管道。
- **低端渲染管道**： iOS 和 Android 平台的默认渲染管道。、

渲染管道由一个根通行证组成，根通行证驱动一系列通行证，用于在渲染管道中处理不同的图形技术。更多信息，请参阅[通道系统](/docs/atom-guide/dev-guide/passes/pass-system/)部分。


## 预建渲染管道支持的功能

本表列出了主渲染管道和低端渲染管道默认支持的功能。

| 特性 | 说明 | 主渲染管道 | 低端渲染管道 |
| - | - | - | - |
| Skinning | 基于计算着色器的网格蒙皮，用于骨骼加权动画和变形目标。 | Yes | Yes |
| Directional shadows | 级联阴影贴图，模拟太阳光。 | Yes | Yes |
| Punctual shadows | 点光源和聚光灯的阴影。 | Yes | Yes |
| Area lights | 球形灯、圆盘灯、胶囊灯、四边形灯和多边形灯。 | Yes | Yes |
| Light culling | 基于瓦片或集群的光线剔除，以便在前向+通路中实现高效的光线迭代。 | Yes | Yes |
| RTX global illumination | 使用 NVIDIA RTXGI（RTX 全局照明）的实时全局照明。 | Yes | No |
| Reflection probe | 为镜面反射照明（IBL）生成和使用反射探针。| Yes | Yes |
| Physically based sky | 使用基于物理的大气建模进行基本的天空渲染。| Yes | No |
| HDR skybox | 使用 HDR 图像作为天空盒。| Yes | Yes |
| Screen space ambient occlusion (SSAO) | 在场景中应用 SSAO，在物体交汇处创建接触阴影。 | Yes | No |
| Subsurface scattering | 利用屏幕空间技术计算皮肤等材质的次表面散射。 | Yes | No |
| Deferred fog | 在场景中应用基于高度的雾化效果。 | Yes | No |
| Screen space reflections (SSR) | 通过使用渲染的照明缓冲区来近似实时反射，从而实现 SSR。 | Yes | No |
| Depth of field | 根据场景的深度和相机对焦参数，为场景应用虚化景深效果。| Yes | No |
| Bloom | 在渲染的图像上应用绽放效果，使明亮的像素向邻近像素渗出。 | Yes | No |
| Subpixel morphological anti-aliasing (SMAA) | 应用 SMAA 作为抗锯齿技术。 | Yes | No |
| Temporal anti-aliasing (TAA) | TAA 使用运动矢量和时间超采样来减少最终图像中的混叠现象。 | Yes | No |
| Light adaptation | 根据场景的平均亮度调整最终图像的亮度，使图像看起来既不会太暗也不会太亮。 | Yes | Yes |
| Look modification and color grading | 允许艺术家使用查找表（LUT）调整图像的最终效果。 | Yes | Yes |
| Display mapping | 根据屏幕期望的色彩范围调整最终图像的色彩值。 | Yes | Yes |
| User interface (UI) | 渲染二维用户界面组件 | Yes | Yes |
| Auxilary geometry (AuxGeom) | 为编辑器内工具和调试渲染各种几何图形和文本，例如小工具、网格、调试线和形状。 | Yes | Yes |



## 文件结构

呈现管道存储为一个 `.azasset` 文件，它是一个 JSON 文件，链接到呈现管道的根传递并包含其他配置。在该文件中，“RootPassTemplate ”参数包含管道根传递的名称。

下面的示例是 `MainRenderPipeline.azasset` 文件。根通行模板名为 `MainPipeline`，它链接到 `MainPipeline.pass` 文件。
```json
{
    "Type": "JsonSerialization",
    "Version": 1,
    "ClassName": "RenderPipelineDescriptor",
    "ClassData": {
        "Name": "MainPipeline",
        "MainViewTag": "MainCamera",
        "RootPassTemplate": "MainPipeline",
        "RenderSettings": {
            "MultisampleState": {
                "samples": 4
            }
        }
    }
}

```


您可以在 O3DE 引擎源代码中找到以下渲染管道文件：
- **主渲染管道e**: `Gems\Atom\Feature\Common\Assets\Passes\MainRenderPipeline.azasset`
- **低端渲染管道e**: `Gems\Atom\Feature\Common\Assets\Passes\LowEndRenderPipeline.azasset`


## 修改渲染管道

由于模块化通行证驱动着渲染管道，因此添加或删除通行证以及更新连接都更加方便。你可以修改主呈现管道或为团队创建自定义呈现管道。出于多种原因，你可能需要使用自定义渲染管道，例如：

- 开发影响渲染管道的新功能。
- 实施和使用其他渲染技术。
- 提高渲染性能。

修改渲染管道需要启用或禁用一个通道并更新连接。您可以使用文本编辑器手动编辑 `MainPipeline.pass` ，直接完成上述操作。

要启用或禁用渲染管道中的某一通道，请执行以下操作
1. 在渲染管道的 `.pass` 文件中，在 `PassRequests` 列表中找到要启用或禁用的通行证。
2. 将 `Enabled` 属性设置为 true 或 false。

启用或禁用通行证时，通行证系统会自动更新通行证层次结构中的连接。这就需要你设置后备连接。更多信息，请参阅 [管道模板文件规范](/docs/atom-guide/dev-guide/passes/pass-template-file-spec/)

例如，在`MainPipeline.pass`的`PassRequests`部分，将`Enabled`设置为 false，从而禁用呈现管道中的  `DeferredFogPass` 。
```JSON
    "PassRequests": [

        ...
        
        {
            "Name": "DeferredFogPass",
            "TemplateName": "DeferredFogPassTemplate",
            "Enabled": false,
            "Connections": [
                ...
            ],
            "PassData": { ... }
        },
        
        ...
    ]
```
