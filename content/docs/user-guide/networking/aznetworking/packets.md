---
linktitle: 数据包结构
title: 网络数据包结构
description: 关于Open 3D Engine (O3DE)网络协议栈使用的 UDP 和 TCP 数据包结构的概述和参考。
weight: 100
---

**Open 3D Engine (O3DE)** 在其网络协议栈中支持 TCP 和 UDP 协议。这样，您就可以灵活选择如何将游戏连接到现有服务，以及在服务器通信中引入何种级别的开销。虽然 TCP 数据包本身包含的信息极少，但在这些连接的背后还有 TCP 协议的开销（和保证）。UDP 数据包包含较长的报头，以帮助解决一致性问题，即使数据包可能被丢弃。这样，就可以通过 O3DE Gems 自定义恢复机制，实现回滚等功能。

本节仅描述数据包的整体结构，不包括最佳实践或有效执行数据包有效载荷数据的信息。

## 数据包结构

| TCP 数据包结构 | UDP 数据包结构 |
|--|--|
| ![TCP packets: Flags 1 byte, type 2 bytes, size 2 bytes](/images/user-guide/networking/tcp-packet-structure.png) | ![UDP Packets: Flags 1 byte, type 2 bytes, local seq ID 2 bytes, remote seq ID 2 bytes, ACK replication 4 bytes, reliability flag 1 byte, reliable seq ID 2 bytes (if reliability flag set)](/images/user-guide/networking/udp-packet-structure.png) |

## 数据包标志和类型信息

数据包标志是一个 8 位值，存储在数据包的开头。

| 标签 | 位 | 说明 |
|--|--|--|
| Compressed | `0` | 数据包的数据传输部分是否通过 [`ICompressor`](/docs/api/frameworks/aznetworking/class_az_networking_1_1_i_compressor.html) 实现进行压缩。 |
| N/A | 1-7 | 留作将来使用。 |

数据包类型是一个 16 位值，可以设置为任何数字标识符，但范围 `CorePackets::PacketType::START..CorePackets::PacketType::MAX` 中的值是为 O3DE 内部使用的数据包类型保留的。当从 [`IPacketHeader`](/docs/api/frameworks/aznetworking/class_az_networking_1_1_i_packet_header.html) 派生时，这意味着您使用的任何数据包类型的第一个 **有效的** 值应该是`CorePackets::PacketType::MAX+1`。因此，建议您为自定义数据包类型编写如下 `enum` ：

```cpp
enum class MyPacketType {
    START = aznumeric_cast<int32_t>(CorePackets::PacketType::MAX),
    PacketType1,
    PacketType2,
    ...,
    PacketTypeN,
    MAX
}
```

建议使用 `MAX` 作为哨兵值，这样，如果其他开发者在您的数据包类型之上编写扩展，他们就可以为自己的数据包类型从适当的 `MyPacketType::MAX+1` 值开始，避免冲突。

{{< note >}}
除了不与 O3DE 的内部数据包类型和超类使用的任何数据包类型冲突外，数据包头并不要求普遍唯一。{{< /note >}}

## TCP 数据包字段

TCP 数据包特有的字段如下：

| 字段 | 说明 |
|--|--|
| Packet size | 数据包数据的总大小。避免从数据包有效载荷中读取任何大于此大小的数据，以防访问不安全的内存。 |

## UDP 数据包字段

UDP 数据包特有的字段如下：

| 字段 | 说明 |
|--|--|
| Local sequence ID | 此数据包实例的序列 ID。 |
| Remote sequence ID | 远程端点接收到的最后一个序列 ID。用于 ACK 复制。 |
| ACK replication | 用于复制传输确认的其他信息。 |
| Reliability flag | 数据包是否启用了可靠性功能。 |
| Reliable sequence ID | 如果该数据包是可靠的，则这是可靠性的序列 ID。 |
