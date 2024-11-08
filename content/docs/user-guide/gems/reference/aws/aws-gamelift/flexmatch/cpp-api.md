---
linkTitle: FlexMatch C++ API
title: FlexMatch C++ API 
description: 了解如何将 FlexMatch C++ API 与 Open 3D Engine （O3DE） 中的 AWS GameLift Gem 结合使用。
toc: true
weight: 300
---


## 客户端

**AWS GameLift** Gem 为 Amazon GameLift 实施对战接口。*对战接口*（`IMatchmakingRequests` 和 `IMatchmakingAsyncRequests`）提供公共 API，以将玩家对战功能添加到您的 GameLift 托管游戏。

matchmaking interface 表示在游戏中处理 matchmaking 所需的操作。Gem 为此接口提供了一个实现，该接口利用 GameLift FlexMatch 在游戏的会话中查找和分组玩家。

每个对战解决方案只能有一个对战接口的实现。要添加对其他对战解决方案的支持，您必须创建对战接口的另一个实现。


### Matchmaking APIs

### `StartMatchmaking`

为一组玩家查找对战游戏，并创建会话以托管对战游戏。请查看[StartMatchmaking](https://docs.aws.amazon.com/gamelift/latest/apireference/API_StartMatchmaking.html)了解更多详情。

要开始匹配，请调用`AWSGameLiftClientManager::StartMatchmaking()` 或 `AWSGameLiftClientManager::StartMatchmakingAsync()`，并传入对 StartMatchmaking 请求的引用，其中包含匹配条件。发送请求后，您可能希望继续轮询对战票证状态，以检查对战是否完成。

```cpp
// Make synchronous call to start a game match for the current player
AWSGameLift::AWSGameLiftStartMatchmakingRequest request;
request.m_ticketId = "YourMatchmakingTicketId";
request.m_configurationName = "YourMatchmakingConfigurationName";

AWSGameLift::AWSGameLiftPlayer player;
player.m_playerAttributes["skill"] = "{\"N\": 23}";
player.m_playerAttributes["gameMode"] = "{\"S\": \"deathmatch\"}";
player.m_playerId = "CurrentPlayerId";
request.m_players = { player };

AZStd::string result = "";
AWSGameLift::AWSGameLiftMatchmakingRequestBus::Broadcast(&AWSGameLift::AWSGameLiftMatchmakingRequestBus::Events::StartMatchmaking, request, result);

// Make asynchronous call to start a game match for the current player
AWSGameLift::AWSGameLiftStartMatchmakingRequest request;
request.m_ticketId = "YourMatchmakingTicketId";
request.m_configurationName = "YourMatchmakingConfigurationName";

AWSGameLift::AWSGameLiftPlayer player;
player.m_playerAttributes["skill"] = "{\"N\": 23}";
player.m_playerAttributes["gameMode"] = "{\"S\": \"deathmatch\"}";
player.m_playerId = "CurrentPlayerId";
request.m_players = { player };

AWSGameLift::AWSGameLiftMatchmakingAsyncRequestBus::Broadcast(&AWSGameLift::AWSGameLiftMatchmakingAsyncRequestBus::Events::StartMatchmakingAsync, request);
 
void OnStartMatchmakingAsyncComplete(const AZStd::string& matchmakingTicketId)
{
    ...
}
```

### `StopMatchmaking`

取消当前正在处理的对战票证。查看 [StopMatchmaking](https://docs.aws.amazon.com/gamelift/latest/apireference/API_StopMatchmaking.html) 了解更多详情。

要停止匹配请求，请调用`AWSGameLiftClientManager::StopMatchmaking()` 或 `AWSGameLiftClientManager::StopMatchmakingAsync()`，并传入对 StopMatchmaking 请求的引用，其中包含匹配票证 ID。

```cpp
// Make synchronous call to stop a specific matchmaking request
AWSGameLift::AWSGameLiftStopMatchmakingRequest request;
request.m_ticketId = "YourMatchmakingTicketId";
AWSGameLift::AWSGameLiftMatchmakingRequestBus::BroadcastResult(result, &AWSGameLift::AWSGameLiftMatchmakingRequestBus::Events::StopMatchmaking, request);

// Make asynchronous call to stop a specific matchmaking request
AWSGameLift::AWSGameLiftStopMatchmakingRequest request;
request.m_ticketId = "YourMatchmakingTicketId";
AWSGameLift::AWSGameLiftMatchmakingAsyncRequestBus::Broadcast(&AWSGameLift::AWSGameLiftMatchmakingAsyncRequestBus::Events::StopMatchmakingAsync, request);

void OnStopMatchmakingAsyncComplete()
{
    ...
}
```

### `AcceptMatch`

启用对战接受时，注册玩家对建议的对战游戏的接受或拒绝。查看 [AcceptMatch](https://docs.aws.amazon.com/gamelift/latest/apireference/API_AcceptMatch.html) 了解更多详情。

要接受或拒绝匹配，请调用`AWSGameLiftClientManager::AcceptMatch()` 或 `AWSGameLiftClientManager::AcceptMatchAsync()`，并传入对 AcceptMatch 请求的引用，其中包含玩家决策。
如果任何玩家拒绝对战，或者在指定的超时之前未收到接受，则建议的对战将被取消。

```cpp
// Make synchronous call to accept the proposed matchmaking
AWSGameLift::AWSGameLiftAcceptMatchRequest request;
request.m_acceptMatch = true;
request.m_ticketId = "YourMatchmakingTicketId";
request.m_playerIds = { "CurrentPlayerId" };

AWSGameLift::AWSGameLiftMatchmakingRequestBus::BroadcastResult(result, &AWSGameLift::AWSGameLiftMatchmakingRequestBus::Events::AcceptMatch, request);

// Make asynchronous call to accept the proposed matchmaking
AWSGameLift::AWSGameLiftAcceptMatchRequest request;
request.m_acceptMatch = true;
request.m_ticketId = "YourMatchmakingTicketId";
request.m_playerIds = { "CurrentPlayerId" };
AWSGameLift::AWSGameLiftMatchmakingAsyncRequestBus::Broadcast(&AWSGameLift::AWSGameLiftMatchmakingAsyncRequestBus::Events::AcceptMatchAsync, request);

void OnAcceptMatchAsyncComplete()
{
    ...
}
```

### `StartPolling`

请求启动基于给定票证 ID 和玩家 ID 轮询对战票证的进程。

要在匹配开始后开始轮询匹配票证状态，请调用`AWSGameLiftClientLocalTicketTracker::StartPolling()`并传入匹配票证 ID 和玩家 ID。

```cpp
// Make synchronous call to start polling
AZStd::string ticketId = "YourMatchmakingTicketId";
AZStd::string playerId = "CurrentPlayerId";
AWSGameLift::AWSGameLiftMatchmakingEventRequestBus::Broadcast(&AWSGameLift::AWSGameLiftMatchmakingEventRequestBus::Events::StartPolling, ticketId, playerId);
```

### `StopPolling`

请求停止轮询对战票证的进程。

要在匹配完成或取消后停止轮询匹配票证状态，请调用 `AWSGameLiftClientLocalTicketTracker::StopPolling()`。

```cpp
// Make synchronous call to stop polling
AWSGameLift::AWSGameLiftMatchmakingEventRequestBus::Broadcast(&AWSGameLift::AWSGameLiftMatchmakingEventRequestBus::Events::StopPolling);
```

{{< caution >}}  
AWS GameLift Gem 提供的本地票证跟踪器仅用于开发。O3DE 开发人员应该用公开发布游戏的可扩展解决方案来取代它。
{{< /caution >}}


### 客户端通知

当票证状态更新时，将从对战票证跟踪系统发送通知。

### `OnMatchComplete`

匹配请求完成后，将从匹配票证跟踪系统广播`Multiplayer::MatchmakingNotificationBus::Events::OnMatchComplete()`通知。完成对战票证后，玩家可以加入托管对战的游戏会话。

```cpp
bool OnMatchComplete()
{
    ...
}
```

### `OnMatchError`

当匹配请求处理时出错时，将从匹配票证跟踪系统广播`Multiplayer::MatchmakingNotificationBus::Events::OnMatchError()`通知。

```cpp
bool OnMatchError()
{
    ...
}
```

### `OnMatchFailure`

当匹配请求未能完成时，将从匹配票证跟踪系统广播`Multiplayer::MatchmakingNotificationBus::Events::OnMatchFailure()`通知。

```cpp
bool OnMatchFailure()
{
    ...
}
```

### `OnMatchAcceptance`

当找到匹配项并等待接受时，将从匹配票证跟踪系统广播`Multiplayer::MatchmakingNotificationBus::Events::OnMatchAcceptance()` 通知。

```cpp
bool OnMatchAcceptance()
{
    ...
}
```

## 服务端

### Matchmaking APIs

以下 API 仅用于手动回填。您可以在对战配置中设置回填模式（自动或手动）。
查看 [使用 FlexMatch 回填现有游戏](https://docs.aws.amazon.com/gamelift/latest/flexmatchguide/match-backfill.html) 了解更多详情。

### `StartMatchBackfill`
发送请求，为现有游戏会话中的空位查找新玩家。有关更多详细信息，请查看 [C++的 GameLift 服务器 API 参考：操作](https://docs.aws.amazon.com/gamelift/latest/developerguide/integration-server-sdk-cpp-ref-actions.html#integration-server-sdk-cpp-ref-startmatchbackfill)。

要开始手动回填过程，请调用`AWSGameLiftServerManager::StartMatchBackfill()`，并传入对票证 ID 和玩家列表的引用。

当现有会话开始更新时，将在服务器端广播`OnUpdateSessionBegin`通知以执行任何配置或初始化。
在更新结束时，将在服务器端广播`OnUpdateSessionEnd`通知以执行任何后续操作。

```cpp
AZStd::string matchmakingTicketId = "YourMatchmakingTicketId";
// This list should contain the current players in the match, and each player should have expected attributes. Otherwise gamelift gem will use lazy loaded data which is not guaranteed to be accurate.
AZStd::vector<AWSGameLift::AWSGameLiftPlayer> players;
bool result;
AWSGameLift::AWSGameLiftServerRequestBus::BroadcastResult(result, &AWSGameLift::AWSGameLiftServerRequestBus::Events::StartMatchBackfill, matchmakingTicketId, players);
```

### `StopMatchBackfill`
取消使用 StartMatchBackfill 创建的活动匹配回填请求。有关更多详细信息，请查看 [C++的 GameLift 服务器 API 参考：操作](https://docs.aws.amazon.com/gamelift/latest/developerguide/integration-server-sdk-cpp-ref-actions.html#integration-server-sdk-cpp-ref-stopmatchbackfill)。

要停止手动回填过程，请调用`AWSGameLiftServerManager::StopMatchBackfill()`，并传入对票证 ID 的引用。

```cpp
AZStd::string matchmakingTicketId = "YourMatchmakingTicketId";
bool result;
AWSGameLift::AWSGameLiftServerRequestBus::BroadcastResult(result, &AWSGameLift::AWSGameLiftServerRequestBus::Events::StopMatchBackfill, matchmakingTicketId);
```


### Session notification

### `OnUpdateSessionBegin`

在会话更新过程开始时，将调用`Multiplayer::SessionNotificationBus::Events::OnUpdateSessionBegin`来执行任何配置或初始化，以处理会话设置更改。

```cpp
bool OnUpdateSessionBegin(const SessionConfig& sessionConfig, const AZStd::string& updateReason)
{
    ...
}
```

### `OnUpdateSessionEnd`

在会话更新过程结束时，将调用`Multiplayer::SessionNotificationBus::Events::OnUpdateSessionEnd`以在会话更新后执行任何后续操作。

```cpp
bool OnUpdateSessionEnd()
{
    ...
}
```

{{< note >}}  
服务器端 API 和通知仅适用于手动回填模式。您无需为自动回填模式调用它们。
{{< /note >}}
