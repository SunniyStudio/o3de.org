---
linktitle: Multiplayer Compression
title: Multiplayer Compression Gem
description: Open 3D Engine （O3DE） 提供了一个包含压缩实现的示例 Gem。
---

**Multiplayer Compression Gem** 提供 [`AzNetworking::ICompressor`](/docs/api/frameworks/aznetworking/class_az_networking_1_1_i_compressor.html) 和 [`AzNetworking::ICompressorFactory`](/docs/api/frameworks/aznetworking/class_az_networking_1_1_i_compressor_factory.html) 的基于 LZ4 的实现。Gem 将提供的压缩机工厂注册到 O3DE Networking System Component。要使用 Gem 及其相关压缩器，请将其添加到您的项目中，并将 cvars `net_UdpCompressor` 和 `net_TcpCompressor` 设置为`MultiplayerCompressor`。

{{< caution >}}
此 Gem 旨在作为如何实现 compressor 的示例。我们强烈建议您编写一个 （或 Compressor） 来满足您的流量需求。
{{< /caution >}}
