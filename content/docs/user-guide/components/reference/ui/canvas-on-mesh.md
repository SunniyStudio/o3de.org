---
linkTitle: UI Canvas on Mesh
title: UI Canvas on Mesh 组件
description: 使用 UI Canvas on Mesh 组件将 UI 画布放置在 3D 世界中的组件实体上，用户可以在 Open 3D Engine （O3DE） 中与之交互。
---

使用 **UI Canvas on Mesh** 组件，您可以将 UI 画布放置在 3D 世界中的组件实体上，用户可以使用光标与之交互。

## 用法

当您想要加载同一 UI 画布的两个唯一实例时，您可以使用 **Render target override** 属性，用户可以将其设置为不同的状态。将此属性分配给`.attimage`附件图像资源会覆盖 UI 画布的 [**Render Target** 属性](/docs/user-guide/interactivity/user-interface/canvases/canvas-properties/#rendering-properties)值。您必须使用在 **Render target override** 中选择的相同 Attachment Image 资源作为网格或角色材质的漫反射纹理。

有关如何使用 **UI Canvas Asset Ref** 组件的更多信息，请参阅 [在 3D 世界中放置 UI Canvas](/docs/user-guide/interactivity/user-interface/canvases/placing-canvases-3d/)。

## UI Canvas on Mesh 组件属性 

UI Canvas on Mesh组件具有以下属性：

**Render target override**
对于简单情况，您可以将此属性留空。UI 画布指定渲染目标，该渲染目标可用作 3D 网格上材质的纹理。
当您想要加载同一 UI 画布的两个唯一实例时，您可以使用 **Render target override** 属性，用户可以将其设置为不同的状态。将此属性分配给附件图像资源会覆盖已加载的 UI 画布实例的 **Render to Texture** 值。

![Two entities load a unique instance of the same canvas](/images/user-guide/component/ui_canvas/component-ui-canvas-on-mesh-properties2.png)

有关如何使用 UI Canvas on Mesh 组件的更多信息，请参阅 [在 3D 世界中放置 UI Canvas](/docs/user-guide/interactivity/user-interface/canvases/placing-canvases-3d)。

## 提供方

[LyShine Gem](/docs/user-guide/gems/reference/ui/lyshine/)

## 依赖

选择以下必需组件之一以显示 UI 画布：
- [Actor 组件](/docs/user-guide/components/reference/animation/actor)
- [Mesh 组件](/docs/user-guide/components/reference/atom/mesh)

选择以下必需组件之一，以提供对 UI 画布的引用：
- [UI Canvas Asset Ref 组件](./canvas-asset-ref)
- [UI Canvas Proxy Ref 组件](./canvas-proxy-ref)

## UI Canvas on Mesh 属性 

![UI Canvas Asset Ref properties](/images/user-guide/components/reference/ui/ui-canvas-on-mesh-component.png)

| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **Render target override** | 选择将覆盖 UI 画布的 **Render Target** 属性的 '.attimage' `.attimage` [Attachment Image 资产](/docs/user-guide/interactivity/user-interface/canvases/canvas-properties/#attachment-image-assets)。 | Attachment Image asset | None |
