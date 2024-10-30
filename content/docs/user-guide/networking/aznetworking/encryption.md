---
linktitle: UDP 加密
title: 带数据报传输层安全（DTLS）的 UDP
description: 了解Open 3D Engine (O3DE) `AzNetworking` 框架的加密功能以及如何在您的项目中使用这些功能。
weight: 300
---

为了提供加密以确保 UDP 数据包的安全，**Open 3D Engine (O3DE)** 使用  [OpenSSL](https://www.openssl.org/) 来实现*数据报传输层安全 (DTLS)*。O3DE 的`AzNetworking` 框架提供了[DtlsSocket](/docs/api/frameworks/aznetworking/class_az_networking_1_1_dtls_socket.html) 和 [DtlsEndpoint](/docs/api/frameworks/aznetworking/class_az_networking_1_1_dtls_endpoint.html)类，用于在 UDP 上启用 DTLS。在 O3DE 网络系统中启用和配置加密后，传输层会自动使用 `DtlsSocket` 对象，而不是 `UdpSocket`。`DtlsEndpoint` 的作用是捕获连接目标并处理 SSL 操作。

## 握手

谨慎使用 DTLS 的一个原因是，建立连接的握手过程是一项昂贵的操作。下图[^1]显示了客户端和服务器在数据传输前进行协商的过程。

![DTLS handshake diagram. 'Hello' is exchanged and verified, certificates are sent to the client, the server verifies the request, the server establishes the cipher, and data transmission begins.](/images/user-guide/networking/dtls-handshake.png)

O3DE 网络传输层采用的方法是简单地重复握手过程，直到底层 OpenSSL 库表示连接已建立。通过使用这种方法，O3DE 可以启用和存储 cookie，而无需在握手后进行任何额外配置。

用于完成握手操作的数据包类型有

* `InitiateConnectionPacket` - 发出执行未加密连接的信号。建立连接后，发送启用 SSL 的请求，启动 DTLS 握手。
* `ConnectionHandshakePacket` - 从底层 OpenSSL 握手调用中传输握手数据。
* `FragmentedPacket` - 由 `ConnectionHandshakePacket` 使用的碎片包，允许握手过程中交换的 SSL 数据超出 MTU。

为确保只传输这些类型的数据包，所有其他数据包都会在验证后排队发送。这发生在任何数据包分片之前，因此生成的唯一`FragmentedPackets`是`ConnectionHandshakePacket`。

{{< note >}}
这些类没有 API 引用，因为它们是在[AzAutoGen](/docs/user-guide/programming/autogen/)的 `Code/Framework/AzNetworking/AzNetworking/AutoGen`内容中生成的。
{{< /note >}}

一旦握手的一方确认 SSL 初始化，就会立即开始传输加密流量。如果握手过程的另一方尚未完成 SSL 初始化，就会出现在完成握手过程时需要阻止加密流量的问题。为了解决这个问题，加密套接字会**在连接时**丢弃它认为是垃圾的数据。一旦握手完成，收到垃圾数据将导致断开连接。

## 应用数据传输

一旦通过身份验证，加密就是发送数据包时执行的最后一步。这主要是因为假定已通过身份验证的端点收到的任何数据包都已加密。只要 `DtlsEndpoint` 被视为已连接，所有数据包都会被加密。

加密通常会增加数据包的有效载荷大小。为了确保这种税收不会使数据包的大小超过 MTU，O3DE 会为应用程序减去一定量的可用数据，以适应加密大小。其结果是，在加密前接近 MTU 的数据包最终会在启用加密后被抢先分片。

## 参考

[^1]: [负载循环网络中的 DTLS 性能 - ResearchGate 上的科学图谱](https://www.researchgate.net/figure/Message-exchange-during-a-DTLS-handshake-Messages-in-parentheses-are-not-sent-for_fig1_308815075) \[accessed 31 May, 2021\]
