---
title: "渲染"
toc:
---

Open 3D Engine (O3DE)的图形功能通过 Atom 渲染器集成，这是一个基于物理的高性能渲染引擎。Atom 提供以下功能：

* 光线追踪，可生成详细、真实的环境。
* 基于物理的渲染，实现真实世界的照明。
* 现代后期处理效果，增强游戏体验。
* 前向+渲染，未来支持延迟渲染。


Atom 具有统一的界面，支持多种平台和图形应用程序接口：
* Windows 上的 DirectX 12 或 Vulkan
* MacOS 和 iOS 上的 Metal
* Linux 上的 Vulkan

Atom 指南中的以下章节将详细介绍 O3DE 的图形功能。

| 功能 | 说明 |
| - | - |
| [材质](/docs/atom-guide/dev-guide/materials/) | 使用 Atom 的材质编辑器创建支持全局照明的 PBR 材质。 |
| [着色器](/docs/atom-guide/dev-guide/shaders/) | 使用 Atom 的着色语言 AZSL 创建着色器。 |
| [Atom Gem 组件](/docs/user-guide/components/reference/#atom) | 使用 Atom Gem 提供的组件添加网格、灯光和各种后期处理效果。|
| 粒子 | 使用集成到 Atom 中的 [PopcornFX](https://www.popcornfx.com/o3de/) 制作粒子特效。 |
