---
linktitle: 自动数据包
title: 网络自动数据包
description: 了解如何通过自动数据包定义 Open 3D Engine (O3DE) 网络数据包。
weight: 200
---

**自动数据包**提供了一种定义网络数据包的便捷方法，用于在 `AzNetworking` 会话中在端点之间进行数据通信。通过使用 [AzAutoGen](/docs/user-guide/programming/autogen) 系统，在构建过程中会对项目中的自动数据包文件进行处理，从而为定义了有效载荷的数据包创建 C++ 类和处理程序。

要启用项目的自动数据包编译功能，必须将 AZ::AzNetworking 添加为项目的编译依赖项。

## 自动数据包文件结构

自动数据包在 XML 文件中定义，放置在相关项目或 Gem 的 `Code\Source\Autogen` 目录中。

### PacketGroup 属性

自动数据包定义的顶级元素是`PacketGroup`。数据包组将不同类型的单个数据包定义联系在一起。作为数据包组一部分定义的数据包被置于同一命名空间。

| 属性 | 说明 | 类型 |
|---|---|---|
| Name | 生成的自动数据包的命名空间。 | 一个有效的 C++ 命名空间标识符。 |
| PacketStart | 对此数据包组的数据包类型标识符开始排序的值。为避免与核心 `AzNetworking` 框架冲突，请使用 `CorePackets::PacketType::Max`。  | `const int32` |

### Packet 属性

`Packet` 标签定义了一个新的数据包。

| 属性 | 说明 | 类型 |
|---|---|---|
| Name | 生成的自动数据包的类名。 | 一个有效的 C++ 类标识符。 |
| Desc | 对生成的自动数据包的描述。 | `string` |
| HandshakePacket | 如果启用，则数据包是连接握手的一部分。 | `bool` |

{{< note >}}
只有当用户为数据包执行的 `IsHandshakeComplete` 函数返回`false`时，才会处理将`HandshakePacket`设置为 true 的数据包。
{{< /note >}}

### Member 属性

`Member` 标签定义数据包上的数据。这是定义数据包有效载荷的主要机制。

| 属性 | 说明 | 类型 |
|---|---|---|
| Name | 数据包成员的名称。 | 一个有效的 C++ 变量名。 |
| Type | 数据包成员的类型。| 有效的 C++ 类型。 |
| Init | 数据包成员的初始值。 | `<Type>` |
| Container | 如果提供，则该成员是一个 `<Type>`. | `Vector`, `Array`|
| | **Vector**: 可调整大小的vector。|
| | **Array**: 大小固定的数组。| |
| Count | 如果提供了容器，则指容器的大小。 | `uint` |

### Include

AzAutoGen 使用 `Include`标记来生成 C++ 代码的`#includes`。为生成的类所使用的每个头文件使用一个 `Include` 标记。

| 属性 | 说明 | 类型 |
|---|---|---|
| File | 要添加到生成的源代码中作为 `#include` 的头文件的路径。 | `string` |

### 示例

以下是用于Multiplayer Gem的[自动数据包组](https://github.com/o3de/o3de/blob/main/Gems/Multiplayer/Code/Source/AutoGen/Multiplayer.AutoPackets.xml)示例。

```xml
<?xml version="1.0" encoding="utf-8"?>

<PacketGroup Name="MultiplayerPackets" PacketStart="CorePackets::PacketType::MAX">
    <Include File="AzNetworking/AutoGen/CorePackets.AutoPackets.h" />
    <Include File="Multiplayer/MultiplayerTypes.h" />
    <Include File="Multiplayer/NetworkTime/INetworkTime.h" />
    <Include File="Multiplayer/NetworkEntity/NetworkEntityRpcMessage.h" />
    <Include File="Multiplayer/NetworkEntity/NetworkEntityUpdateMessage.h" />

    <Packet Name="Connect" HandshakePacket="true" Desc="Client connection packet, on success the server will reply with an Accept">
        <Member Type="uint16_t" Name="networkProtocolVersion" Init="0" />
        <Member Type="Multiplayer::LongNetworkString" Name="ticket" />
    </Packet>

    <Packet Name="Accept" HandshakePacket="true" Desc="Server accept packet">
        <Member Type="Multiplayer::HostId" Name="hostId" Init="Multiplayer::InvalidHostId" />
        <Member Type="Multiplayer::LongNetworkString" Name="map" />
    </Packet>
  
    <Packet Name="ReadyForEntityUpdates" Desc="Client confirming it is ready to receive entity updates">
      <Member Type="bool" Name="readyForEntityUpdates" />
    </Packet>

    <Packet Name="SyncConsole" Desc="Packet for synchornizing cvars between hosts">
        <Member Type="Multiplayer::LongNetworkString" Name="commandSet" Container="Vector" Count="32" />
    </Packet>

    <Packet Name="ConsoleCommand" Desc="Packet for executing a server command from the client">
        <Member Type="Multiplayer::LongNetworkString" Name="command" />
    </Packet>

    <Packet Name="EntityUpdates" Desc="A packet that contains multiple entity updates">
        <Member Type="AZ::TimeMs" Name="hostTimeMs" Init="AZ::TimeMs{ 0 }" />
        <Member Type="Multiplayer::HostFrameId" Name="hostFrameId" Init="Multiplayer::InvalidHostFrameId" />
        <Member Type="Multiplayer::NetworkEntityUpdateMessage" Name="entityMessages" Container="Vector" Count="Multiplayer::MaxAggregateEntityMessages" />
    </Packet>

    <Packet Name="EntityRpcs" Desc="A packet that contains multiple entity rpcs">
        <Member Type="Multiplayer::NetworkEntityRpcMessage" Name="entityRpcs" Container="Vector" Count="Multiplayer::MaxAggregateRpcMessages" />
    </Packet>

    <Packet Name="ClientMigration" Desc="Tell a client to migrate to a new server">
        <Member Type="uint64_t" Name="temporaryUserIdentifier" Init="0" />
        <Member Type="AzNetworking::IpAddress" Name="remoteServerAddress" Init="AzNetworking::IpAddress()" />
        <Member Type="AZ::TimeMs" Name="lastInputGameTimeMs" Init="AZ::TimeMs{ 0 }" />
    </Packet>
</PacketGroup>
```

### 数据包处理

除 AzNetworking 中定义的 CorePackets 外，数据包均通过 IConnectionListener 接口处理。该接口的实现者通常会定义以下内容：

```C++
    //! IConnectionListener interface
    //! @{
    AzNetworking::ConnectResult ValidateConnect(const AzNetworking::IpAddress& remoteAddress, const AzNetworking::IPacketHeader& packetHeader, AzNetworking::ISerializer& serializer) override;
    void OnConnect(AzNetworking::IConnection* connection) override;
    AzNetworking::PacketDispatchResult OnPacketReceived(AzNetworking::IConnection* connection, const AzNetworking::IPacketHeader& packetHeader, AzNetworking::ISerializer& serializer) override;
    void OnPacketLost(AzNetworking::IConnection* connection, AzNetworking::PacketId packetId) override;
    void OnDisconnect(AzNetworking::IConnection* connection, AzNetworking::DisconnectReason reason, AzNetworking::TerminationEndpoint endpoint) override;
    //! @}
```

这里值得注意的是 OnPacketReceived，因为它将按以下方式转发给 PacketGroup 的调度程序：

```C++
    AzNetworking::PacketDispatchResult MultiplayerSystemComponent::OnPacketReceived(AzNetworking::IConnection* connection, const IPacketHeader& packetHeader, ISerializer& serializer)
    {
        return MultiplayerPackets::DispatchPacket(connection, packetHeader, serializer, *this);
    }
```

调度器希望 IConnectionListener 实现者实现一个 IsHandshakeComplete 函数，并为每个数据包实现一个 HandleRequest 函数。上面的 MultiplayerPackets 示例是在 [MultiplayerSystemComponent](https://github.com/o3de/o3de/blob/main/Gems/Multiplayer/Code/Source/MultiplayerSystemComponent.h) 中处理的。

#### IsHandshakeComplete

该函数检查 IConnectionListener 的握手逻辑是否完成。这对于指定 AzNetworking 以外的任何额外握手逻辑非常有用。例如，Multiplayer 在开始处理游戏流量前会使用此函数与 ISessionProvider 进行检查。当 IsHandshakeComplete 返回 false 时，只有指定为 HandshakePackets 的数据包才会被处理。所有其他类型的数据包都将被跳过。

#### HandleRequest

HandleRequest 定义了每种数据包类型的回调。

| 属性 | 说明 | 类型 |
|---|---|---|
| connection | 数据包发送的连接。 | `AzNetworking::IConnection*` |
| packetHeader | 数据包的报头。 | `const IPacketHeader&` |
| packet | AzCodeGen 所定义的数据包本身。 | `<Type>`: 必须是相关数据包组的有效数据包类型。 |
