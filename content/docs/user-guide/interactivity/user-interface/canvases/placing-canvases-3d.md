---
linkTitle: 在 3D 世界中放置 UI 画布
title: 在 3D 世界中放置 UI 画布
description: 在 Open 3D Engine （O3DE） 的 3D 世界中将 UI 画布放置在对象上。
weight: 500
---

您可以将 UI 画布直接放置在 3D 世界中的对象上，而不是在屏幕空间中显示它。为此，您需要将 UI 画布渲染到纹理，然后在网格或角色的材质中使用该纹理。

您可以使用实体上的任何材质来显示由 UI 画布渲染的纹理。要与网格或角色上的画布交互，该实体需要 **UI Canvas on Mesh** 组件。

## 在世界中放置画布

1. 在[**UI Editor**](/docs/user-guide/interactivity/user-interface/editor/)中[创建一个 UI 画布](/docs/user-guide/interactivity/user-interface/canvases)

1. 在[画布属性](/docs/user-guide/interactivity/user-interface/canvases/canvas-properties)中，启用 **Render to texture** 属性。  

1. 在**Render Target**属性中，点击 {{< icon "picker.svg" >}} 按钮，然后选择一个`.attimage` Attachment Image 资产。 有关`.attimage`源资源的示例，请参阅 [附件图像资产](/docs/user-guide/interactivity/user-interface/canvases/canvas-properties/#attachment-image-assets) 。

1. 在关卡中，创建一个实体。

1. 选择实体后，在 **Entity Inspector** 中，将 **UI Canvas Asset Ref** 组件添加到实体，然后选择您创建的 UI 画布。启用 **自动加载**。

1. 向实体添加 **Mesh** 组件，然后选择 **Model Asset**。

1. 将 **Material** 组件添加到实体，然后选择要显示画布的材料。通过单击 **Model Materials** 属性旁边的箭头来编辑材质实例。选择 **打开材质实例编辑器...**。在 Material Instance Editor 中，将 **Base Color** 纹理更新为与 UI 画布的 **Render Target** 相同的 Attachment Image 资源。

1. （对于可交互画布）将 **UI Canvas on Mesh** 组件添加到实体。如果要在不同的实体上加载 UI 画布的多个实例并让它们显示不同的状态，请在 **Render target override** 属性中选择 Attachment Image 资产。否则，请将此属性留空。

    {{< note >}}
您必须为每个实体创建唯一的 Attachment Image 资产，为该资产分配实体的材质，然后在 **Render target override** 属性中分配 Attachment Image 资产。
{{</ note >}}

## 实体组件配置示例

下图显示了一个实体，该实体配置为在其网格上显示不可交互的画布。

![Entity Inspector with components that have been configured](/images/user-guide/interactivity/user-interface/canvases/ui-editor-placing-canvases-3d.png)
