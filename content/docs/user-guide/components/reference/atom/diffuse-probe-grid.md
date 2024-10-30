---
title: Diffuse Probe Grid组件
linktitle: Diffuse Probe Grid
description: "Open 3D Engine (O3DE) Diffuse Probe Grid组件参考。"
toc: true
---

**Diffuse Probe Grid** 组件创建一个光探头体积，在指定区域内提供漫反射全局照明（GI）。体积中的每个探针都使用实时光线追踪来捕捉**辐照度**或周围的漫射光环境。实时光线追踪会在每个探针周围的不同方向投射数百条光线。每条光线的最大长度为 30 米。在光线与周围几何体的每个交叉点，探头都会存储照明信息。然后，利用光线追踪数据创建辐照度纹理，并将纹理应用到每个网格上。


{{< note >}}
要在**Atom 渲染器**中使用光线追踪功能，您的 GPU 必须支持 DirectX Shader Model 6.3 或更高版本。
{{< /note >}}


## 提供方

[Atom Gem](/docs/user-guide/gems/reference/rendering/atom/atom/)


## 依赖

[Box Shape 组件](/docs/user-guide/components/reference/shape/box-shape/)


## 属性

![Diffuse Probe Grid component properties](/images/user-guide/components/reference/atom/diffuse-probe-grid-component-ui.png)


### Bake Textures

| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **Bake Textures** | 将周围的漫反射光环境烘焙成纹理，并在**Editor Mode**或**Runtime Mode**设置为 `Baked`时使用。只有当编辑器模式或运行时模式设置为 `Real Time (Ray-Traced)` 时，才能烘焙纹理。 | - | - |


### Probe Spacing

探针间距是指沿体积内各轴探针之间的距离（以米为单位）。间距越小，探针数量越多，从而提高 GI 的逼真度，但会牺牲性能和内存。空间的最大范围取决于 **Box Shape** 组件的尺寸。

| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **X-Axis** | 探针之间沿 x 轴的空间大小。该空间必须在 Box Shape 组件沿 x 轴的尺寸范围内。 | `0.0` 到 Box Shape 组件的 `Dimensions`-`X` 属性。 | `2.0` |
| **Y-Axis** | 探针之间沿 Y 轴的空间大小。该空间必须在 Box Shape 组件沿 Y 轴的尺寸范围内。 | `0.0` 到 Box Shape 组件的 `Dimensions`-`Y` 属性， | `2.0` |
| **Z-Axis** | 探针之间沿 Z 轴的空间大小。该空间必须在Box Shape组件沿 Z 轴的尺寸范围内。 | `0.0` 到 Box Shape 组件的 `Dimensions`-`Z` 属性， | `2.0` |


### Grid Settings

| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **Ambient Multiplier** | 增加体积内 GI 的强度。它可用于微调场景的光照，或使 GI 更明显以便调试。 | `0.0` 到 `10.0` | `1.0` |
| **View Bias** | 微调功能可消除因探头位置而可能出现的视觉伪影。 | `0.0` 到 `1.0` | `0.2` |
| **Normal Bias** | 表面到光线二次射线投射的微调，用于确定表面点是否受直射光影响。增加法线偏置会使光线投射的起点离表面更远，从而更有可能受到直射光的影响。 | `0.0` 到 `1.0` | `0.1` |
| **Editor Mode** | 控制**Editor**使用实时还是烘焙漫反射 GI。`Real Time (Ray-Traced)` 要求 GPU 具有光线追踪功能。如果没有光线追踪功能，`Auto Select`将使用`Baked`作为备用。请参阅[全局光照模式](#global-illumination-modes)。 | `Real Time (Ray-Traced)`, `Baked`, `Auto Select` |`Real Time (Ray-Traced)` |
| **Runtime Mode** | 控制单机运行时使用实时还是烘焙漫反射 GI。`Real Time (Ray-Traced)`要求 GPU 具有光线追踪功能。如果没有光线追踪功能，`Auto Select`将使用`Baked`作为备用。请参阅[全局光照模式](#global-illumination-modes)。 | `Real Time (Ray-Traced)`, `Baked`, `Auto Select` |`Real Time (Ray-Traced)` |


## 全局光照模式

您可以通过调整 `Editor Mode` 和 `Runtime Mode`属性来调整编辑器和独立运行时处理 GI 的方式。共有三种模式：

- **Real Time (Ray-Traced)**: 根据周围几何体和光线的变化不断更新。它要求 GPU 能够使用 DXR 或 Vulkan-RT 进行实时光线追踪。该模式最适合编辑场景，但会降低性能，具体取决于 GPU 和场景中网格的复杂程度。 

- **Baked**: 使用之前通过`Bake Textures`按钮捕捉并存储在纹理中的光照信息。在没有能进行实时光线追踪的 GPU 的机器上，它可以大大提高性能，并允许使用漫反射 GI。

- **Auto Select**: 根据机器 GPU 的性能决定使用哪种模式。这样就可以在支持该模式的机器上提供实时光线追踪漫反射 GI，同时也可以将烘焙漫反射 GI 作为备用模式。

编辑器和运行时有多种配置设置：

- **Editor Real-Time and Runtime Baked**: 编辑器使用实时漫反射 GI 更新，而运行时则使用烘焙漫反射 GI 以获得最佳性能。请注意，必须明确烘焙纹理，运行时才能以`Baked`模式运行。
  
- **Editor Real-Time and Runtime Auto Select**: 编辑器使用实时光线追踪技术以获得最佳编辑体验，而运行时则根据 GPU 的能力选择实时光线追踪或烘焙。

- **Editor Baked and Runtime Baked**: 编辑器和运行时均使用烘焙漫反射 GI 以获得最佳性能。在适当的时候，可以将编辑器模式临时切换为`Real Time (Ray-Traced)`并烘焙纹理，从而显式烘焙纹理。
