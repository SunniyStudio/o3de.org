---
title: Occlusion Culling Plane 组件
linktitle: Occlusion Culling Plane
description: 'Open 3D Engine (O3DE) Occlusion Culling Plane 组件参考。'
toc: true
---

**Occlusion Culling Plane** 组件添加了一个平面（称为**Occluder**），当遮挡器位于摄像机和网格之间时，它将阻止网格渲染。遮挡剔除是一种渲染技术，可减少发送到渲染器的非可见物体的数量。当一个物体在屏幕上，并且没有完全位于遮挡剔除平面之后时，它就是可见的。 


## 提供方 ##

[Atom Gem](/docs/user-guide/gems/reference/rendering/atom/atom/)


## 属性


![occlusion-culling-plane-component-base-properties](/images/user-guide/components/reference/atom/occlusion-culling-plane/occlusion-culling-plane-base-properties-ui.png)

| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **Show Visualization** | 将场景中的遮挡剔除平面显示为绿色平面，以便调试。  | Boolean | `Enabled` |
| **Transparent Visualization** | 将可视化显示为半透明平面。 | Boolean |  `Disabled` |
