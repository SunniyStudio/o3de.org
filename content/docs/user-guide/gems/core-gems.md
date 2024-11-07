---
linktitle: 核心 Gems
title: 提供核心 Open 3D Engine功能的 Gem
description: Open 3D Engine Gem 概述，它们提供游戏和模拟的核心通用功能。
weight: 300
---

虽然 Open 3D Engine 的架构和核心框架提供了支持游戏和模拟开发的支柱和基础设施，但创建项目通常需要的大多数功能都以通过 Gem 系统扩展到核心的形式出现。并非所有这些 Gem 都由使用 O3DE 运行时的已发布项目使用，有些特定于 Editor、Asset Processor 或其他 O3DE 工具。

即使 Gem 仅添加 Editor 或 Asset Processor 功能，它也应作为项目 Gem 的一部分包含在内。

{{< note >}}
所有项目都需要 O3DE 核心 （LmbrCentral） Gem 来提供核心编辑器功能。 O3DE Core （LmbrCentral） 还提供了许多用于游戏的组件。 阅读 [O3DE Core (LmbrCentral)文档](../reference/o3de-core)了解更多信息。
{{< /note >}}

这些 Gem 仅为需要它们的产品构建和加载：

* **Atom** - O3DE 使用的基于物理的实时渲染器。除了在运行时渲染之外，Atom 还提供 Editor 组件和资源处理工具。有关 Atom 的更多信息，请阅读 [Atom 指南](/docs/atom-guide)。

* **CameraFramework** - 用于放置和控制摄像机的组件。阅读 [Cameras 文档](/docs/user-guide/components/reference/camera/camera) 了解更多信息。

* **EMotionFX** - EMotionFX Gem 包括一套面向艺术家和动画师的编辑器和资产处理工具。它还支持处理来自网络流量的动画，以及通过 C++ API 控制动画状态的功能。

* **AudioSystem** - O3DE 使用的核心音频抽象集。这提供了 O3DE 的音频实现使用的接口集，例如 Wwise Audio Gem。阅读 [Audio 文档](/docs/user-guide/interactivity/audio/) 了解在O3DE中音效工作原理的更多信息。

* **PhysX** - 支持使用 NVIDIA PhysX 进行物理模拟。阅读 [O3DE中的NVIDIA PhysX文档](/docs/user-guide/interactivity/physics/nvidia-physx/) 了解更多信息。

* **ScriptCanvas** 和 **GraphCanvas** - Script Canvas 是 O3DE 使用的图形脚本语言，其编辑器 UX 工具由 GraphCanvas Gem 提供支持。ScriptCanvas Gem 本身提供 Script Canvas 工具和运行时。阅读 [Script Canvas documentation](/docs/user-guide/scripting/script-canvas/)  了解更多信息。

* **EditorPythonBindings** - 支持 Editor 的动态 Python 脚本。阅读 [Python Bindings Examples](/docs/user-guide/editor/editor-automation/#python-editor-bindings-gem-examples)  了解更多信息。

* **PythonAssetBuilder** - 添加了对通过 Python 脚本构建资产的 Asset Processor 支持。阅读 [Python Asset Builder Gem documentation](/docs/user-guide/assets/builder/)  了解更多信息。

* **ImageProcessing** - 添加了向 Asset Processor 处理常见图像格式的功能。
