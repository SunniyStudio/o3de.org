---
linkTitle: Remote Tools
title: Remote Tools Gem
description: 远程工具 Gem 为 O3DE 应用程序提供了一个 API，用于相互连接以进行调试。
toc: true
---

**远程工具 Gem** 提供基于 AzNetworking 的连接功能，允许 O3DE 应用程序相互连接以启用调试。这方面的一个例子是 O3DE 编辑器连接到 Lua IDE 以调试 Lua 脚本。

**远程工具 Gem** 通过在 AzFramework 中实现 IRemoteTools 接口来工作。由于 Gem 用于调试目的，因此此注册是为发布版本编译的。

{{< note >}}
**Remote Tools Gem**行为（包括 Lua IDE 调试）在发布版本中被禁用。
{{< /note >}}

## IRemoteTools API

IRemoteTools API 处理远程工具端点和相关实用程序函数之间的通信。主要功能包括：

```cpp
//! 使用预定义的密钥、名称和目标端口将应用程序注册为 Remote Tools 服务的客户端
//! @param key 用于标识此服务的 Crc32 密钥
//! @param name 此服务的字符串名称
//! @param port 此服务连接的端口
virtual void RegisterToolingServiceClient(AZ::Crc32 key, AZ::Name name, uint16_t port) = 0;
```

```cpp
//! 使用预定义的密钥、名称和目标端口将应用程序注册为 Remote Tools 服务的主机
//! @param key 用于标识此服务的 Crc32 密钥
//! @param name 此服务的字符串名称
//! @param port 此服务开始侦听注册的端口
virtual void RegisterToolingServiceHost(AZ::Crc32 key, AZ::Name name, uint16_t port) = 0;
```

```cpp
//! Gets pending received messages for a given Remote Tools Service
//! @param key The key of the service to retrieve messages for
//! @return A vector of received messages pending processing for the given service
virtual const ReceivedRemoteToolsMessages* GetReceivedMessages(AZ::Crc32 key) const = 0;
```

```cpp
//! 清除给定 Remote Tools 服务的待接收消息。
//! 在必须带外处理消息的情况下很有用。
//! @param key 用于清除消息的服务密钥
virtual void ClearReceivedMessages(AZ::Crc32 key) = 0;
```

```cpp
//! 向远程终端节点发送消息
//! @param target 要向其发送消息的终端节点
//! @param msg 要发送的消息
virtual void SendRemoteToolsMessage(const RemoteToolsEndpointInfo& target, const RemoteToolsMessage& msg) = 0;
```
