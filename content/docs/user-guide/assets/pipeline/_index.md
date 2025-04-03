---
linkTitle: 资产管道
title: 资产管道和资产处理
description: Open 3D Engine (O3DE)中资产、资产处理和资产管道的完整指南。
weight: 100
toc: true
---

**Open 3D Engine (O3DE)** 资产管道是端到端的流程，用于从源资产生成运行时优化的产品资产。这些主题提供了整个资产管道的概述，包括源资产、扫描目录、**资产构建器**、资产依赖性以及存储由资产管道生成的产品资产的**资产缓存**的说明。

| 主题 | 说明 |
| --- | --- |
| [资产处理](asset-processing) | 对如何分析源资产和如何生成产品资产进行高度概述。 |
| [ 源资产 Assets](source-assets) |  源资产可以有自定义处理选项，并在各种情况下自动处理。 |
| [Scan Directories](scan-directories) | 对扫描目录进行监控，以发现新的和更新的源资产。 |
| [Asset Builders](asset-builders) | 资产构建器为流程作业提供信息并生成产品资产。 |
| [Intermediate Assets](intermediate-assets) | 中间资产可将构建器串联起来并重复使用。 |
| [Metadata Asset Relocation (Experimental)](metadata) | 元数据资产重定位通过在侧载文件（.meta）中存储 UUID，允许文件自由移动和重命名，而不会破坏现有的引用。 |
| [Product Assets](product-assets) | 产品资产是 O3DE 项目内容的运行准备版本。 |
| [Asset Dependencies and Identifiers](asset-dependencies-and-identifiers) | 资产依赖性和标识符可确保在处理、加载和打包资产时满足资产引用要求。 |
| [资产缓存](asset-cache) | 资产缓存存储运行时优化的产品资产，以及 ** 资产处理器**跟踪资产和更新资产所需的信息。 |
