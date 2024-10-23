---
linkTitle: 资产 
title: 资产管道和资产处理 
description: Open 3D Engine (O3DE)中资产、资产处理和资产管道的完整指南。
weight: 700
toc: true
---

本节的主题涵盖**资产管道**和资产处理，包括什么是源资产、如何发现和跟踪源资产以及如何将源资产处理为运行时优化的产品资产。此外，还有管理资产管道的**Asset Processor资产处理器**以及自定义源资产处理的**Scene Settings场景设置**和**Texture Settings纹理设置**工具指南。最后的指南介绍了如何使用**Python Asset Builder**创建自定义**Asset Builders**。

| 主题区域 | 说明 |
| --- | --- |
| [资产管道](pipeline) | 资产管道是将源资产转化为运行时优化产品资产的端到端流程。 |
| [场景管道](scene-pipeline) | 场景管道是一种专门的资产生成器，可导入源场景文件，并允许场景生成器导出模型和动画等场景产品资产。  |
| [资产处理器Asset Processor](asset-processor) | 资产处理器发现源资产、管理资产处理任务并维护 [资产缓存](pipeline/asset-cache)。 |
| [场景源资产](scene-settings) | 创建资产的信息和最佳实践。这些主题包括如何使用[场景设置](scene-settings/scene-settings) 自定义处理网格、演员、动作和 PhysX 碰撞体的详细信息。 |
| [纹理设置](texture-settings) | 通过 “纹理设置”，您可以自定义处理源图像资产的方式。 |
| [Python Asset Builder](builder) | 使用 Python Asset Builder，您可以为已知文件类型创建自定义 Asset Builders。 |
| [资产类型](asset-types) | 本表列出了 O3DE 支持的源资产类型及其生成的产品资产。 |
| [运行时资产系统](runtime) | 有关使用和操作运行时资产系统的信息。 |
