---
linkTitle: Fast Noise
title: Fast Noise Gem
description: Fast Noise Gradient Gem 使用第三方开源 FastNoise 库，在 Open 3D Engine （O3DE） 中提供多种高性能噪声生成算法。
toc: true
---

Fast Noise Gradient Gem 使用第三方开源 [FastNoise 库](https://github.com/Auburn/FastNoiseLite) 在Open 3D Engine （O3DE） 中提供多种高性能噪点生成算法。

Fast Noise Gem 提供 Fast Noise Gradient （快速噪声梯度） 组件，该组件将噪声生成算法表示为梯度信号。任何与 Gradient Signal Gem 兼容的系统（如 Vegetation）都可以使用杂色生成。

渐变信号是介于 0.0 到 1.0 之间的值，并自动映射到世界位置。它们通常以灰度图像或波形的形式显示。
