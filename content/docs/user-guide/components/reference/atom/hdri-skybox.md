---
linkTitle: HDRi Skybox
title: HDRi Skybox 组件
description: 在 Open 3D Engine (O3DE) 项目中使用 HDRi Skybox 组件创建天空盒。 
toc: true
---

**HDRi Skybox** 组件通过使用立方体贴图纹理在场景中创建天空盒。立方体贴图纹理或天空盒是用于场景照明的图像源资产。图像由每个像素每个通道的数据组成，这些数据代表曝光值 (EV) 或 HDR **stops** 的数量。


## 提供方

[Atom Gem](/docs/user-guide/gems/reference/rendering/atom/atom/)


## 限制

HDRi Skybox 和 **Physical Sky** 组件是可互换的天空盒解决方案。如果同时拥有两个组件或多个组件，则只会渲染第一个激活的天空盒。


## 属性

![HDRi Skybox interface](/images/user-guide/components/reference/atom/hdri-skybox-component-ui.png)


### HDRi Skybox 属性

| 属性 | 说明 | 值 | 默认值 |
| - | - | - | - |
| **Cubemap Texture** | HDRi Skybox 支持的立方图纹理。  | `.streamingimage` 产品资产。 |  |
| **Exposure** |  标度曝光强度。曝光值单位为 EV100（一个 EV 表示 100 个国际标准单位（ISO））. | Float: -20.0 - 20.0 | `0.0` |


## 旋转天空盒

要在 **O3DE Editor**中旋转天空和，在实体的Transform中调整 **Rotation** 属性。

![Rotate the skybox](/images/user-guide/components/reference/atom/hdri-skybox/hdri-skybox-rotate.png)
