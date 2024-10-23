---
linkTitle: Simple LOD Distance
title: Simple LOD Distance 组件
description: 在Open 3D Engine (O3DE)中，使用Simple LOD Distance组件来设置每个Actor LOD 的距离。
---

通过**Simple LOD Distance**组件，您可以为Actor的每个细节级别（LOD）设置距离和运动采样率。它需要一个**Actor**组件，该组件引用了一个Actor资产，除基本Actor网格群组外，至少还有一个 LOD 网格群组。

简单 LOD 距离组件会自动为Actor资产中的每个 LOD 在**LOD distance (Max)**和**Anim graph sample rates**列表中创建一个元素。包括基本网格组在内，最多支持六个 LOD。您可以为每个 LOD 网格组设置以米为单位的显示距离和运动采样率（每秒）。

有关Actor LOD 的更多信息，请参阅主题 [使用Actor LOD 优化性能](/docs/user-guide/visualization/animation/using-actor-lods-optimize-game-performance/)。

## 提供者

[EMotionFX](/docs/user-guide/gems/reference/animation/emotionfx)

## 依赖

* [Actor 组件](actor)
* Actor资产包含一个基础网格组和至少一个 LOD 网格组。

## Simple LOD Distance 属性

![Add the Simple LOD Distance component to an entity to set display distances for each LOD mesh group.](/images/user-guide/components/reference/animation/simple-lod-distance-component.png)
1
| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **LOD distance (Max)** | 设置Actor每个 LOD 的最大显示距离。距离值以米为单位（世界单位）。索引 0 是角色的基本网格组。 | 0 到 无限 | 取决于 LOD 序号。 |
| **Enable LOD anim graph sampling** | 启用每个 LOD 的动画采样率。启用后，**Anim graph sample rate**列表将显示每个 LOD 的一个元素。| Boolean | `Disabled` |
| **Anim graph sample rate** | 设置每个 LOD 的动画采样率（每秒）。索引 0 是Actor的基本网格组。将一个 LOD 的采样率设置为 `0`可以最大化采样率，这样每秒就可以对该 LOD 进行尽可能多的动画采样。任何非零值都指定了 LOD 的最大动画采样率。大于 `0` 的较低值可提供更好的性能。较高的值（或 `0`）可能会降低性能，但会产生更流畅的动画。 | 0 到 无限 | 取决于 LOD 序号。 |
