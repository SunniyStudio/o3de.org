---
title: Global Skylight (IBL) 组件
linktitle: Global Skylight (IBL)
description: 'Open 3D Engine (O3DE) Global Skylight (IBL) 组件参考。'
toc: true
---

**Global Skylight (IBL)** 组件提供了使用立方图的基于Specular 和 Diffuse 的光照。这也被称为**Global IBL**。


## 提供方 ##

[Atom Gem](/docs/user-guide/gems/reference/rendering/atom/atom/)


## 属性

![global-skylight-ibl-component-base-properties](/images/user-guide/components/reference/atom/global-skylight-ibl/global-skylight-ibl-base-properties-ui.png)

| 属性 | 说明 | 值              | 默认值 |
|-|-|----------------|-|
| **Diffuse Image** | 指定场景中用于Diffuse IBL 的 cubemap 图像。 |                |  |
| **Specular Image** | 指定场景中用于Specular IBL 的 cubemap 图像。 |                |  |
| **Exposure** | 指定将 IBL cubemap应用到场景时使用的曝光量。 | `-5.0` 到 `5.0` | `0.0` |
