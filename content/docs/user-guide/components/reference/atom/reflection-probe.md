---
title: Reflection Probe 组件
linktitle: Reflection Probe
description: 'Open 3D Engine (O3DE) Reflection Probe 组件参考。'
toc: true
---

**Reflection Probe** 将镜面反射应用于探针体积内的网格。 反射以立方图的形式存储，并在烘焙时使用反射探针的实体位置作为捕捉点。 

每个Reflection Probe由两个体积组成：一个外体积（使用Box形状组件指定）和一个内体积。 外部体积定义了受探针影响的总面积，并与其他重叠的探针体积和[Global IBL](/docs/user-guide/components/reference/atom/global-skylight-ibl)镜面立方体地图进行**混合**。 内体积定义了仅使用探针立方体贴图渲染的区域，这意味着没有来自其他探针或全局 IBL 的混合。 请注意，如果多个探针内部体积重叠，则使用最小的体积。


## 提供方

[Atom Gem](/docs/user-guide/gems/reference/rendering/atom/atom)


## 依赖

[Box Shape component](/docs/user-guide/components/reference/shape/box-shape/)


## 属性

![Reflection Probe component properties](/images/user-guide/components/reference/atom/reflection-probe-component-ui.png)
 

### Inner Extents 属性

| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **Height** | 反射探头内部空间的高度。高度取决于**Box Shape**组件沿 Z 轴的尺寸。  | `0` 到 Box Shape 组件的 `Dimensions`-`Z` 属性 | Box Shape 组件的 `Dimensions`-`Z` 属性的值。 |
| **Length** | 反射探头内部空间的长度。长度取决于Box Shape组件沿 Y 轴的尺寸。 | `0` 到 Box Shape 组件的 `Dimensions`-`Y` 属性 | Box Shape 组件的 `Dimensions`-`Y` 属性的值。 |
| **Width** | 反射探头内部空间的宽度。宽度取决于Box Shape组件沿 x 轴的尺寸。 | `0` 到 Box Shape 组件的 `Dimensions`-`X` 属性| Box Shape 组件的 `Dimensions`-`X` 属性的值。 |

### Settings 属性

| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **Parallax Correction** | 通过调整捕捉位置的偏移来修正反射。| Boolean | `Enabled` |
| **Show Visualization** |  显示一个可视化探针的球体。 |  Boolean | `Enabled` |
| **Exposure** |  在场景中应用反射立方体贴图时使用的曝光量。 | `-5.0` to `5.0` | `0.0` |

### Cubemap Bake 属性

| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **Bake Reflection Probe** | 将周围环境烘焙成立方图。 | - | - |
| **Bake Exposure** |  烘焙反射立方图时使用的曝光量。 | `-16.0` to `16.0` | `0.0` |

### Cubemap 属性

| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **Use Baked Cubemap** | 如果启用，将使用烘焙的立方图创建反射效果。 | Boolean | `Enabled` |
| **Baked Cubemap Quality** | 烘焙立方图的分辨率。 | `Very Low`, `Low`, `Medium`, `High`, `Very High` | `Medium` |
| **Baked Cubemap path** |  烘焙后的 cubemap 文件的路径。烘焙 cubemap 时，此属性会自动生成 cubemap 文件的路径。 | - | - |
