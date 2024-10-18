---
title: 对比度自适应锐化
description: "了解 Atom 渲染器中的对比度自适应锐化 (CAS)，该渲染器是集成在 Open 3D Engine (O3DE) 中的图形引擎。"
toc: true
---

**对比度自适应锐化**（Contrast adaptive sharpening，CAS）是一种图像锐化技术，在决定锐化程度时会考虑局部 3x3 邻域的对比度。高对比度样本的锐化程度远远低于低对比度样本。这有助于避免在均匀锐化的标准锐化滤镜中出现的过度锐化现象。您可以将 CAS 与时间抗锯齿 (TAA) 结合使用，以减少 TAA 给图像带来的柔和感。更多信息，请参阅 [时间抗锯齿](taa/)。

**Atom 渲染器**中的 CAS 基于 AMD 的实现。更多信息，请参阅 [AMD FidelityFX CAS](https://gpuopen.com/fidelityfx-cas/)。


## 参数

| 参数 | 说明 | 值 | 默认值 |
| - | - | - | - |
| **m_strength** | 控制锐化效果的强度。 | `0.0` 到 `1.0` | `0.25` |


## 使用 CAS

在主渲染管道中，您可以通过父通道程序`PostProcessParent.pass`启用  `ContrastAdaptiveSharpening.pass`。默认情况下，CAS 是禁用的。要启用 CAS，请在父传递中将 `Enabled` 设置为 `true`。

您也可以将 `ContrastAdaptiveSharpening.pass` 添加到自定义管道中。

要将 CAS 与 TAA 结合使用以提高图像质量，请在`Taa.pass`之后运行`ContrastAdaptiveSharpening.pass`。
