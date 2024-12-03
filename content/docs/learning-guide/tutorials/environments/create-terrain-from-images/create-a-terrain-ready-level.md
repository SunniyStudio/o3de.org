---
linkTitle: 创建地形就绪关卡
title: 创建地形就绪关卡
description: 创建一个已准备好编写地形的新关卡。
weight: 100
toc: true
---

在本教程部分中，您将创建一个准备好编写地形的新关卡。

## 创建新关卡

1. 在 **O3DE 编辑器 ** 中，创建一个新关卡。（请参阅 [创建新关卡](/docs/learning-guide/tutorials/reference/environments/create-a-level)教程了解更多详情）

2. 删除 **Atom Default Environment** 下的 **Grid**、**Ground** 和 **Shader Ball** 实体，以便您将构建的地形完全可见。

    {{< image-width src="/images/learning-guide/tutorials/environments/terrain-from-images/delete-default-entities.png" alt="Delete several default entities." >}}

3. 在 **Global Sky** 实体上，删除 [**HDRi Skybox**](/docs/user-guide/components/reference/atom/hdri-skybox/) 组件。默认天空盒图像中的地形将与您将在下面创建的地形发生冲突。相反，您将使用 **Sky Atmosphere** 组件，该组件提供空的天空背景。

    {{< image-width src="/images/learning-guide/tutorials/environments/terrain-from-images/delete-hdri-skybox.png" alt="Delete the HDRi Skybox component." >}}

4. 在 **Entity Outliner** 中，选择 **Sun** 实体。

5. 在 **Entity Inspector** 中，选择 **Add Component** 并添加 **Sky Atmosphere** 组件。通过将其添加到 **Sun** 实体，天空光照将自动与太阳在天空中的方向对齐。

    {{< image-width src="/images/learning-guide/tutorials/environments/terrain-from-images/add-sky-atmosphere.png" alt="Add the Sky Atmosphere component." >}}

现在，除了太阳和蓝天之外，您有一个空的关卡。

{{< image-width src="/images/learning-guide/tutorials/environments/terrain-from-images/terrain-empty-level.png" alt="Illustration of the new empty level with the sky component configured." >}}

## 启用地形系统

要启用地形系统，必须向 **Level** 实体添加两个级别组件。添加这些组件后，将启用地形系统，但尚不可见地形。

1. In **Entity Outliner**, select the **Level** entity.

    {{< image-width src="/images/learning-guide/tutorials/environments/terrain-from-images/enable-terrain-select.png" alt="Select the Level entity." >}}

2. 在 **Entity Inspector**中，选择**Add Component**，并将 [**Terrain World**](/docs/user-guide/components/reference/terrain/world) 和 [**Terrain World Renderer**](/docs/user-guide/components/reference/terrain/world-renderer) 关卡组件添加到 **Level** 实体。

    {{< image-width src="/images/learning-guide/tutorials/environments/terrain-from-images/enable-terrain-components.png" width="600" alt="Add terrain level components." >}}

地形关卡组件中有许多属性可用于配置关卡的地形系统设置。现在，将设置保留为默认值。一旦关卡中存在地形，它们就更容易调整。
