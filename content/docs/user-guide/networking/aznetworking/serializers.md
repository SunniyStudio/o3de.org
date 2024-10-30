---
linktitle: 序列化器
title: 网络序列化器
description: Open 3D Engine (O3DE)网络协议栈使用的数据序列化器概述和参考。
weight: 400
---

**Open 3D Engine (O3DE)** 支持各种序列化器，可访问对象层次结构并对该层次结构执行操作。这些操作通常包括从对象层次结构中读取数据或将数据写入对象层次结构，以便进行持久化或网络传输。

`AzNetworking`和 **Multiplayer Gem** 都使用序列化技术通过数据包和 RPC 进行高性能网络通信。因此，序列化直接关系到网络通信的带宽利用率。在需要定制序列化器的情况下，`AzNetworking` 和 Multiplayer Gem 为如何使用序列化器提供了很好的示例。

本节将介绍`AzNetworking`中包含的各种序列化器实现，以及如何为对象模型编写序列化函数。本节还介绍了作为封装器的序列化器，这些封装器可用于补充其他序列化器类型。

## ISerializer

[AzNetworking::ISerializer](https://github.com/o3de/o3de/blob/main/Code/Framework/AzNetworking/AzNetworking/Serialization/ISerializer.h) 描述了所有 `AzNetworking` 序列化器都要实现的接口。除了处理对象类型序列化的模板机制外，它还描述了基础数据类型的序列化方法。

## 已包含的序列化器

`AzNetworking` 提供了 `ISerializer` 的一些实现。

### NetworkInputSerializer 

`NetworkInputSerializer`  将对象模型数据写入字节流。它通常用于序列化数据，以便在 `AzNetworking` 中使用数据包进行传输。 

### NetworkOutputSerializer 

`NetworkOutputSerializer` 从字节流中读取对象模型数据。它通常用于将数据反序列化为对象，而这些数据是在之前由 `NetworkInputSerializer` 序列化的数据包中接收的。

### DeltaSerializer 

`DeltaSerializer` 对 `DeltaSerializer` 用于创建和应用序列化三角的信息进行编码。一个使用实例是网络输入的序列化。网络输入的值通常比较接近，因此使用 `DeltaSerializer` 对它们进行相对序列化。

### HashSerializer 

`HashSerializer` 为可序列化对象生成 32 位整数哈希值。这些哈希值可用于比较序列化结果，从而有助于检测不同步情况。

### StringifySerializer

`StringifySerializer` 将对象模型数据写入由字符串键和字符串值组成的映射。它可用于生成人可读的对象模型映射。

### TrackChangedSerializer 

TrackChangedSerializer 是一个输出序列化器，可跟踪是否有任何 delta 被实际序列化。它可以封装其他 `Iserializer`类型，以补充封装类型序列化器的跟踪功能。它所执行的跟踪功能会带来轻微的内存和性能代价。

### TypeValidatingSerializer 

`TypeValidatingSerializer`是一个调试序列化器，它封装了其他 `ISerializer` 类型，以便为封装的类型序列化器补充序列化值的类型和名称信息。然后可以对这些值进行检查，以确保数据的一致性。当检测到不匹配时，`TypeValidatingSerializer`会断言，以帮助调试。其功能受 `net_validateSerializedTypes` cvar 的限制，如 [设置](../settings).所述。启用 `net_validateSerializedTypes`时，`TypeValidatingSerializer`会增加带宽成本，以便序列化类型和名称信息。

Multiplayer Gem在非发布版本中使用了 `TypeValidatingSerializer` 。要查看实现，请参阅 [IMultiplayer.h](https://github.com/o3de/o3de/blob/main/Gems/Multiplayer/Code/Include/Multiplayer/IMultiplayer.h)。

## 为对象模型创建序列化

由于序列化器实现了 `Iserializer`接口，因此在编写新的序列化函数时可以使用该接口，这样它们就可以接受任何该类型的序列化器。

例如，请看下面的结构及其 `Serialize` 方法：
```cpp
    struct PlayerState
    {
        PlayerNameString m_playerName;
        uint32_t m_score = 0;          // coins collected
        uint8_t m_remainingShield = 0; // % of shield left, max of ~200% allowed for buffs
        bool operator!=(const PlayerState& rhs) const;
        bool Serialize(AzNetworking::ISerializer& serializer);
    };

    inline bool PlayerState::Serialize(AzNetworking::ISerializer& serializer)
    {
        return serializer.Serialize(m_playerName, "playerName")
            && serializer.Serialize(m_score, "score")
            && serializer.Serialize(m_remainingShield, "remainingShield");
    }
```

`PlayerState` 的 `Serialize` 函数既可用于使用 `NetworkInputSerializer` 为网络传输进行数据序列化，也可用于使用 `NetworkOutputSerializer` 将数据反序列化回 `PlayerState` 中。事实上，任何实现了 `Ierializer` 的类型都可以用作 `PlayerState` 的 `Serialize` 方法的参数。
