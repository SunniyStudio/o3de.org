---
title: CubeMap Capture 组件
linktitle: CubeMap Capture
description: 捕捉实体位置的Specular IBL 或Diffuse IBL 立方图。
toc: true
---

**CubeMap Capture**组件可捕捉场景中特定位置的镜面 IBL 或漫反射 IBL 立方体贴图。您可以将生成的立方体地图与**Global Skylight (IBL)**和**Reflection Probe**组件一起使用。

## 提供方

[Atom Gem](/docs/user-guide/gems/reference/rendering/atom/atom/)


## 属性

![cubemap-capture-component-base-properties](/images/user-guide/components/reference/atom/cubemap-capture/cubemap-capture-base-properties-ui.png)

| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **Capture CubeMap** | 开始捕捉过程的按钮，并在实体位置生成立方图。 |  |  |
| **Capture Type** | 指定生成的 cubemap 类型。 | `Specular IBL`, `Diffuse IBL` | `Specular IBL` |
| **Specular IBL CubeMap Quality** | 指定Specular IBL 立方体贴图的质量级别。 仅当**Capture Type**设置为 `Specular IBL`。 | `Very Low`, `Low`, `Medium`, `High`, `Very High` | `Medium` |
| **CubeMap Path** | 显示输出的 cubemap 路径和文件名。 这是一个只读字段，仅供参考。| | |
| **Exposure** | 指定生成立方图时使用的曝光量。 | `-16.0` to `16.0` | `0.0` |
