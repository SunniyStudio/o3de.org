---
linkTitle: 调试多人游戏不同步
title: 使用审核跟踪调试多人游戏不同步
description: 使用 Open 3D Engine （O3DE） 中的 Multiplayer Desync Audit Trail 工具分析和调试多人游戏不同步。
weight: 800
---

当客户端和服务器对网络变量的值不一致时，将发生不同步。不同步可能非常难以调试，但 *Multiplayer Audit Trail* 工具可以提供帮助。

**Open 3D Engine （O3DE）** 中的 Multiplayer Audit Trail 工具与 **Multiplayer Gem** 一起提供，用于`debug`配置版本。该工具详细说明了发生的所有网络不同步，并对导致不同步的网络活动进行分类，从而允许您找出不同步本身的根本原因。

![Audit Trail Overlay](/images/user-guide/networking/multiplayer/audit_trail_default.png)

Audit Trail （审计跟踪） 工具捕获不同步，包括输入 ID 和发生不同步的主机帧。对于每次不同步，UI 都会列出导致不同步的所有捕获活动。您可以通过 cvar 控制此历史记录的深度。

## 审计类别

Audit Trail 工具旨在捕获 desync 事件，但也可以配置为捕获 *input events* 和 *custom events*。

### 不同步

不同步是 Audit Trail 捕获的主要事件。因此，Audit Trail （审计跟踪） 组织了围绕不同步的所有活动。在客户端上，不同步通常包括变量的增量图以及客户端和服务器不同意的值。

### 输入事件

Network inputs 详细说明了在模拟的 networked 状态中创建增量的操作。Audit Trail （审计跟踪） 工具列出了已发送的所有网络输入。此外，它还列出了每个网络输入的每个成员的非默认值。这允许将 inputs 关联到不同步的数据。通过同时跟踪客户端输入和取消同步，您可以确定玩家操作是否影响特定的不同步。

![Audit Trail Inputs](/images/user-guide/networking/multiplayer/audit_trail_input.png)

前面的屏幕截图显示了客户端的玩家实体上发生的相对于不同步的输入，包括在本地处理的输入。对于有问题的主机帧，动作完全是通过 `NetworkPlayerMovementComponent`移动的玩家。

### 捕获自定义事件

自定义事件是开发人员可以通过其 C++ 源中的宏指定的自定义审核事件。

| 属性 | 说明 |
|---|---|
| `AZ_MPAUDIT_INPUT_REWINDABLE` | 允许开发人员审计 `RewindableObject` 对象的当前本地值与其上一个已知服务器值的比较。 |
| `AZ_MPAUDIT_INPUT_VALUE` | 允许 developer 使用 input 的 timing data 审计非网络化值。计时数据包括 input ID 和 host frame。 |
| `AZ_MPAUDIT_VALUE` | 允许开发人员在没有任何 timing 数据的情况下审计非 networked 值。 |

自定义事件允许开发人员指定他们想要跟踪的其他信息。为了使用这些宏，需要包含`MultiplayerDebug.h`。宏需要的参数是：

* 有问题的变量
* 变量的基础类型。这应该始终是`T`，即使对于被捕获的`RewindableObject<T>`值也是如此。
* `NetworkInput`，用于以`AZ_MPAUDIT_INPUT`开头的命令。这会将宏用法绑定到特定的主机帧和输入 ID，从而能够按事件时间进行分类。

```cpp
Multiplayer::RewindableObject<AZ::Vector3> NetworkExampleComponent::GetVelocity()
{
    return m_velocity;
}

void NetworkExampleComponent::OnExampleEvent(Multiplayer::NetworkInput& input)
{
    // Pass the input, the RewindableObject and its templated type of Vector3
    AZ_MPAUDIT_INPUT_REWINDABLE(input, GetVelocity(), AZ::Vector3);
}
```

![Audit Trail Custom Events](/images/user-guide/networking/multiplayer/audit_trail_event.png)

## 配置

可以修改各种 cvar 以启用其他数据捕获。启用其他捕获会增加 Audit Trail 的性能成本和密度。如果需要捕获更长的事件历史记录，请考虑增加`net_DebutAuditTrail_HistorySize`的值。

| 属性 | 说明 | 类型 |
|---|---|---|
| `cl_EnableDesyncDebugging` | 如果为 true，则启用对标准日志和审核跟踪捕获的不同步输出。 | `bool` |
| `cl_DesyncDebugging_AuditInputs` |如果为 true，则向 Audit Trail 添加输入。 | `bool` |
| `net_DebutAuditTrail_HistorySize` | 审核跟踪将聚合的最大事件数。如果启用了 input 或 event auditing 以补偿增加的事件量，则建议提高此值。 | `int` |
