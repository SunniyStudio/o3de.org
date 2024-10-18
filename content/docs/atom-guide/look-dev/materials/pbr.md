---
linktitle: PBR 材质
title: "基于物理的材质渲染"
description: "了解基于物理的渲染及其如何应用于 Atom 中的材质。"
toc: true
weight: 200
---

**基于物理的渲染（Physically based rendering，PBR）** 是通过模拟真实世界照明模型的物理特性，以逼真的方式进行渲染。下文将介绍如何在 Atom 的核心材质类型中体现 PBR 渲染的概念。

{{< todo issue="https://github.com/o3de/o3de.org/issues/688" >}}
{{< /todo >}}

## 核心PBR材质类型

Atom 中包含以下核心材料类型：

- **StandardPBR**  
  一种全功能的 PBR 材质类型，可通过有限的渲染目标提供尽可能多的功能。
    
    此文件位于[`Gems/Atom/Feature/Common/Assets/Materials/Types/StandardPBR.materialtype`](https://github.com/o3de/o3de/blob/development/Gems/Atom/Feature/Common/Assets/Materials/Types/StandardPBR.materialtype)。

- **EnhancedPBR**  
  StandardPBR 的增强版本，包含更多功能，但性能代价更高。它支持需要额外渲染目标（g 缓冲区）的更高级功能。

  此文件位于[`Gems/Atom/Feature/Common/Assets/Materials/Types/EnhancedPBR.materialtype`](https://github.com/o3de/o3de/blob/development/Gems/Atom/Feature/Common/Assets/Materials/Types/EnhancedPBR.materialtype)。 

- **BasePBR**  
  StandardPBR 的简化替代方案，仅限于最常见的 PBR 功能，如*base color*, *metallic*, *roughness*, 和 *normal*。这对测试和调试特别有帮助，因为它消除了 StandardPBR 中一些增加复杂性的功能。

  此文件位于[`Gems/Atom/Feature/Common/Assets/Materials/Types/BasePBR.materialtype`](https://github.com/o3de/o3de/blob/development/Gems/Atom/Feature/Common/Assets/Materials/Types/BasePBR.materialtype). 
    
- **StandardMultilayerPBR**  
  最多可将三层 StandardPBR 混合在一起。这对于分解大面积表面上的重复图案特别有用。

  此文件位于[`Gems/Atom/Feature/Common/Assets/Materials/Types/StandardMultilayerPBR.materialtype`](https://github.com/o3de/o3de/blob/development/Gems/Atom/Feature/Common/Assets/Materials/Types/StandardMultilayerPBR.materialtype). 

除上述类型外，我们还提供或可能添加更多类型，尤其是针对皮肤和眼睛等特殊用途的类型。要查找可用材料类型的完整列表，请查看[`Gems/Atom/Feature/Common/Assets/Materials/Types`](https://github.com/o3de/o3de/tree/development/Gems/Atom/Feature/Common/Assets/Materials/Types) 文件夹。

## PBR 着色模型

PBR 着色模型由描述材料在物理世界中如何相互作用的属性组成。在基本层面上，PBR 着色模型需要以下属性： **base color**, **metallic**, **roughness**, 和 **specular reflectivity**。这些属性足以定义木材、金属、混凝土和其他原材料等材料。然而，材料的属性会变得复杂得多，如**clear coat**, **subsurface scattering**等等。例如，上过清漆的木材是由一种基本的木质材料制成的，上面还有一层透明涂层。

以下属性列表用于在 Atom 中定义 PBR 材质。[PBR 材质类型中的属性](#properties-in-material-types) 表格中列出了哪些属性包含在哪种材质类型中。
1
| 属性 | 定义 |
| - | - |
| [Base Color](#base-color) | 非金属（电介质）的表面反射颜色或金属（导体）的反射率值。 |
| [Metallic](#metallic) | 表面是否呈现金属光泽。 |
| [Roughness](#roughness) | 表面的表面光滑度或粗糙度。 |
| [Specular Reflectance f0](#specular-reflectance-f0) | 非金属表面的表面反射率。常数 f0 表示正常入射角（菲涅尔 0 角）下的镜面反射率。 |
| Emissive | 模拟从表面发射光线的机制。 |
| Occlusion | 使用烘焙 AO 和空腔纹理贴图描述环境光对表面上某一点的影响程度。 |
| Opacity | 表面的透明度。 |
| Normal | 一种模拟表面凹凸的纹理贴图技术。与凹凸贴图类似。 |
| UVs | 描述纹理如何映射到表面的纹理坐标。 |
| Clear Coat | 表面有一层半透明的薄层。 |
| Detail Layer | 与主表面混合的附加基色和法线纹理贴图，可提供小尺度细节。 |
| Subsurface Scattering | 一种模拟光线穿透半透明表面后在不同位置被散射和吸收的机制。 |
| Irradiance | 描述表面如何与全局照明（GI）系统的漫反射照明环境互动。它不会影响材质本身的外观。 |
| Displacement/Parallax | 使用位移贴图来描述表面的凹凸或偏移。视差闭塞映射等各种技术都会使用这些信息来呈现表面的深度。 |
| Anisotropic Response | 控制光照方向，使表面微小凹槽中的反射拉长。 |

### Base Color

 color for metals.
**base color**定义了非金属的漫反射和金属的镜面反射颜色。

在 PBR 材质类型中配置**Base Color**属性组时，可以在`Color`属性中设置线性 sRGB 颜色。通过在 `Texture Map`属性中指定图像，可以将该颜色与纹理相结合。`Texture Blend Mode`决定了`Color` 和 `Texture Map`的组合方式。`Factor` 值可用于调整混合强度。颜色以线性 sRGB 格式存储在磁盘上，随后转换为 ACEScg 格式，再传递给着色器和 GPU<!-- (for more information, see [Color Management](/docs/atom-guide/look-dev/color-management))DRAFT TOPIC-->. 

### Metallic 

**Metallic** 属性决定了材料是作为电介质（非金属）表面，还是作为导体（金属）表面。金属值通常为完全金属（1）或完全非金属（0），但在使用纹理贴图在两者之间转换时，会出现介于两者之间的值。

在 PBR 材质类型中配置**Metallic**属性组时，对于均匀的表面，金属值是通过`Factor`属性定义的。对于具有不同金属度的表面，可以在 `Texture Map`属性中指定一个图像，表示每个像素的值在 0 到 1 之间。

### Roughness

**Roughness** 属性决定了表面的粗糙度或光泽度。表面越粗糙，反射就越模糊。

在配置 PBR 材质的**Roughness**属性组时，`Factor`属性定义了统一表面的粗糙度（因子 = 1）或光滑度（因子 = 0）。对于粗糙度不同的表面，您可以在`Texture Map`属性中指定一个图像来表示每个像素的粗糙度值。您可以使用`Upper Bound` and `Lower Bound` ”属性调整纹理贴图的值如何转换为粗糙度级别。

### Specular Reflectance f0

The **Specular Reflectance f0** properties determine how much light reflects from dielectric (non-metal) surfaces. (Specular reflectivity on conductor (metal) surfaces are controlled by the [**Base Color**](#base-color)). Specular reflection is based on the Fresnel effect, a model which describes how the amount of light that reflects from a surface depends on the viewing angle and the index of refraction (IOR). When looking straight ahead at a surface, the view is at a 0-degree angle, also known as a normal incidence. From this viewing angle, the amount of light reflected is denoted by f0. The f0 values lie in the range 0 to 0.08, meaning the amount of light reflected can be between 0% to 8%. 

When configuring the **Specular Reflectivity f0** property group in a PBR material type, you can specify a specular reflectance value between 0 and 1 in the `Factor` property. This value is mapped to the [0,0.08] range, representing an f0 value between 0% and 8%. For surfaces with varying levels of specular reflectivity, you can assign an image to the `Texture Map` property to indicate values between 0 and 1 per pixel. A texture map is most useful for composite materials, or materials with a significant variation of material types (for example, material for a character with skin, a metal belt, and a leather watch). 

### Multi-scattering Compensation

By default, Atom uses a lighting model that assumes light only bounces once (single-scattering); but in reality, light may bounce multiple times (multi-scattering). With **multi-scattering compensation**, you can configure the lighting model to perform additional calculations to produce more accurate surface lighting. 

You can take multi-scattering into account in your materials by toggling `Multiscattering Compensation`. For a bit extra performance cost, this simulates the fact that light may bounce off a rough surface multiple times at the microscopic level. This feature makes the lighting on certain materials appear more realistic (brighter). The impact is most noticeable on rough metallic surfaces. For smooth surfaces and non-metal surfaces, the impact likely won't be noticed and should be disabled.

## Properties in Material Types

The following table lists which properties are included in which core PBR material types. 

| Property                  | StandardPBR | EnhancedPBR | BasePBR | StandardMultilayerPBR |
| --                        | --          | --          | --      | --                    |
| Base Color                | X           | X           | X       | X                     |  
| Metallic                  | X           | X           | X       | X                     |  
| Roughness                 | X           | X           | X       | X                     |  
| Specular Reflectivity F0  | X           | X           | X       | X                     |  
| Emissive                  | X           | X           |         | X                     |  
| Occlusion                 | X           | X           |         | X                     |  
| Opacity                   | X           | X           |         |                       |  
| Normal                    | X           | X           | X       | X                     |  
| UVs                       | X           | X           | X       | X                     |  
| Clear Coat                | X           | X           |         | X                     |  
| Subsurface Scattering     |             | X           |         |                       |  
| Irradiance                | X           | X           | X       | X                     |  
| Displacement/Parallax     | X           | X           |         | X                     |  
| Anistropic Response       |             | X           |         |                       |
| Detail Layer              |             | X           |         |                       |
| Detail Layer UV           |             | X           |         |                       |
