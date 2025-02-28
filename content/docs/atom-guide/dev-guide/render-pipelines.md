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

| 特性                                          | 说明                                                                                                                   | 主管线 | 低端管线 |
|---------------------------------------------|----------------------------------------------------------------------------------------------------------------------|-----|------|
| Skinning                                    | 基于计算着色器的网格蒙皮，适用于骨骼加权动画和变形目标。                                                                                         | Yes | Yes  |
| Directional shadows                         | 级联阴影贴图，用于模拟太阳光。                                                                                                      | Yes | Yes  |
| Punctual shadows                            | 精准光源类型的阴影：点光源和聚光灯。                                                                                                   | Yes | Yes  |
| Area lights                                 | 球形、圆盘、胶囊、四边形和多边形光。                                                                                                   | Yes | Yes  |
| Light culling                               | 基于平铺或基于群集的光剔除，可在 Forward+ 通道中实现高效的光迭代。                                                                               | Yes | Yes  |
| RTX global illumination                     | 使用 NVIDIA RTXGI （RTX 全局照明） 的实时全局照明。                                                                                  | Yes | No   |
| Reflection probe                            | 生成并使用反射探针进行基于图像的镜面反射照明 （IBL）。                                                                                        | Yes | Yes  |
| Physically based sky                        | 使用基于物理的大气建模的基本天空渲染。                                                                                                  | Yes | No   |
| HDR skybox                                  | Uses an HDR image as a skybox.将 HDR 图像用作天空盒。                                                                         | Yes | Yes  |
| Screen space ambient occlusion (SSAO)       | 将 SSAO 应用于场景，以在对象相交处创建接触阴影。                                                                                          | Yes | No   |
| Subsurface scattering                       | 使用屏幕空间技术计算材质（如蒙皮）的次表面散射。                                                                                             | Yes | No   |
| Deferred fog                                | 将基于高度的雾应用于场景。                                                                                                        | Yes | No   |
| Screen space reflections (SSR)              | 通过使用渲染的光照缓冲区来近似实时反射来实现 SSR。                                                                                          | Yes | No   |
| Depth of field                              | 将基于场景的深度和摄像机焦点参数的散景形状景深效果应用于场景。                                                                                      | Yes | No   |
| Bloom                                       | 将泛光效果应用于渲染的图像，使亮像素渗出到相邻像素中。                                                                                          | Yes | No   |
| Subpixel morphological anti-aliasing (SMAA) | 将 SMAA 应用为抗锯齿技术。                                                                                                     | Yes | No   |
| Temporal anti-aliasing (TAA)                | TAA 使用运动矢量和时间超级采样来减少最终图像中的锯齿。                                                                                        | Yes | No   |
| Light adaptation                            | 根据场景的平均明亮度调整最终图像的亮度，使图像看起来既不太暗也不太亮。                                                                                  | Yes | Yes  |
| Look modification and color grading         | 允许艺术家使用查找表 （LUT） 调整图像的最终外观。                                                                                          | Yes | Yes  |
| Display mapping                             | 根据屏幕预期的颜色范围调整最终图像的颜色值。                                                                                               | Yes | Yes  |
| User interface (UI)                         | 渲染 2D UI 组件。                                                                                                         | Yes | Yes  |
| Auxilary geometry (AuxGeom)                 | 渲染各种几何体和文本，用于编辑器内工具和调试，例如 Gizmo、网格、调试行和形状。 | Yes | Yes  |
| Terrain system                              | 支持任意大小的关卡的地形创作、模拟和可视化                                                                                                | Yes | No   |



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
