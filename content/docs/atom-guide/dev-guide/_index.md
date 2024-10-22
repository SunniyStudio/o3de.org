---
title: "开发者指南"
description: "了解 Atom 渲染器背后的系统和接口"
toc: true
weight: 600
---

本节将从技术角度深入解读**Atom 渲染器**的基础系统和接口。
1
| 内容                        | 详情 |
|--------------------------------------|---------|
| [帧渲染](frame-rendering.md) | 帧渲染流程描述了场景如何从渲染组件（Render Components）通过 RPI 中的渲染通道（Render Passes）到 RHI 的处理过程。 |
| [渲染管道](render-pipelines.md) | 概述 Atom 预建的渲染管道及其支持的功能。  |
| [材质系统](materials/_index.md) | 材质系统可处理材质和材质类型，并将其构建为资产，以便在模拟中使用。 |
| [着色器系统](shaders/_index.md) | 着色器系统会处理以 AZSL（亚马逊着色语言）编写的着色器，并将其构建为可在项目中使用的着色器资产。 |
| [渲染管道接口 (RPI)](rpi/_index.md) | 渲染管道接口（RPI）包含开发人员对渲染管道进行编程的主要接口。 |
| [通道](passes/_index.md) | 通道系统将场景中的数据转换为最终的渲染输出。 |
| [渲染硬件接口 (RHI)](rhi/_index.md) | 渲染硬件接口（RHI）提供了一个抽象了特定平台代码的底层接口。 |
| [故障排除](troubleshoot.md) | Atom 渲染器图形处理单元 (GPU) 崩溃故障排除指南。 |
