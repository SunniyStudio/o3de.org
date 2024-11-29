---
linkTitle: 版本不匹配
title: 多人游戏版本不匹配
description: 了解如何在 Open 3D Engine （O3DE） 中检测多人游戏版本不匹配。
weight: 900
---

为了使多人游戏模拟保持同步，所有连接的多人游戏终端节点都必须运行相同的多人游戏版本。

例如，假设某个服务器正在运行多人游戏的特定版本。如果更新的客户端连接时对其网络组件进行了更改，则服务器可能不知道如何处理更新的网络属性，或者如何正确序列化网络数据包。

服务器和客户端必须运行所有多人游戏组件（通过网络相互通信的组件）的相同版本;任何差异都可能导致意外行为。

**Open 3D Engine (O3DE)**联网提供多人游戏版本检查来识别和防范这种意外行为。

## 如何启用 Multiplayer Version Mismatch 功能：
多人游戏版本不匹配检测在 [Multiplayer Gem](/docs/user-guide/gems/reference/multiplayer/) 中自动启用。如果两个多人游戏端点连接不同的版本，将触发`Multiplayer::VersionMismatchEvent` [AZ::Event](/docs/user-guide/programming/az-event/)。此外，有关哪些 auto-components 不匹配的信息将打印到控制台并写入日志以帮助调试。

## 相关控制台变量 (CVARs)

| 设置                            | 说明                                                 | 默认值 |                    | 
|------------------------------------|-------------------------------------------------------------|---------|--------------------------|
| sv_versionMismatch_autoDisconnect    | 确定不匹配的连接是否将自动终止。建议保持此状态;即使多人游戏组件版本之间的微小差异也可能导致意外行为。 | True |
| sv_versionMismatch_sendManifestToClient | 确定当出现不匹配时，服务器是否会将其所有单独的多人游戏组件版本信息发送到客户端。收到信息后，客户端将打印哪些组件与控制台不同。建议将发布版本设置为 false。这是为了防止客户端了解任何应保持私有的多人游戏组件信息（组件名称和版本哈希）。 | True |
| bg_viewportConnectionStatus|  如果为 true，则每当发生重要的多人游戏事件时，视区连接状态系统将在屏幕上显示警告;这包括版本不匹配。 | True ||

## 幕后工作原理：
1. AzAutoGen 为其摘要的每个自动组件 XML 文件创建一个唯一的 64 位哈希值。
    1. 64 位哈希值将存储在传递给全局`MultiplayerComponentRegistry`的`ComponentData`类中。
2. 在应用程序启动期间，由于所有 Gem 都向`MultiplayerComponentRegistry`注册其组件，因此`MultiplayerComponentRegistry`将合并每个组件的哈希值，以创建自己的 64 位系统版本哈希值。
3. 在连接事件中，`MultiplayerComponentRegistry`版本哈希作为`MultiplayerPackets::Connect` 数据包的一部分从连接器（通常是客户端）发送到接受器（服务器）。
4. Server 会将客户端的版本哈希值与自己的版本哈希值进行比较，以确保它匹配。
    1. 如果存在多人游戏系统版本不匹配，则：
        1. 交换一个版本不匹配数据包，其中包含每个单独的自动组件的版本哈希，以便服务器和客户端确切知道哪些组件已过期。
        2. 报告错误日志。
        3. 使用`AZ::Interface<IMultiplayer>::Get()->AddVersionMismatchHandler`广播[AZ::Event](/docs/user-guide/programming/az-event/)。
        4. 连接已终止 (如果启用了 `sv_versionMismatch_autoDisconnect`)。 
