---
linkTitle: Session Management C++ API
title: Session Management C++ API 
description: 了解如何将多人游戏会话管理 C++ API 与 Open 3D Engine （O3DE） 中的 AWS GameLift Gem 结合使用。
toc: true
weight: 300
---

## 客户端

**AWS GameLift** Gem 为 Amazon GameLift 实现会话接口。*会话接口*（'`ISessionRequests`' 和 '`ISessionAsyncRequests`'）提供公共 API，允许您创建游戏会话并允许玩家搜索和加入在线游戏。session 接口抽象了 session 相关管理的实现细节。

会话接口执行所有会话处理。Gem 充当会话接口的游戏特定处理程序。游戏代码通过使用 Gem 的 C++ API 与会话交互来进行调用。GameLift 创建并拥有游戏会话，该会话仅在运行在线游戏时存在于服务器上。

每个专用服务器解决方案只能有一个会话接口的实现。要添加对其他专用服务器解决方案的支持，您必须创建会话接口的另一个实现。

建议按照 [GameLift 游戏会话队列的最佳实践](https://docs.aws.amazon.com/gamelift/latest/developerguide/queues-best-practices.html) 创建游戏会话，而不是直接在队组上创建它们。

### 客户端初始化

要向 GameLift 发出请求，您必须使用`AWSGameLiftClientManager::ConfigureGameLiftClient()`配置适当的 GameLift 客户端。

请注意，您必须以正确的格式指定 AWS 区域。例如，对于美国东部（弗吉尼亚北部）区域，请指定 **us-east-1**。有关受支持区域的列表，请参阅 AWS 一般参考中的 [Amazon GameLift 终端节点和配额](https://docs.aws.amazon.com/general/latest/gr/gamelift.html) 。

```cpp
AWSGameLift::AWSGameLiftRequestBus::Broadcast(
    & AWSGameLift::AWSGameLiftRequestBus::Events::ConfigureGameLiftClient,
    "us-east-1" // AWS region
);
```

### Session APIs

### `CreateSession`

创建多人游戏会话供玩家查找和加入。此 API 应仅用于开发期间的试验。更喜欢使用队列和异步游戏会话放置，最好从生产中的 FlexMatch 或游戏服务组件。有关详细信息，请参阅 [开始使用自定义服务器](https://docs.aws.amazon.com/gamelift/latest/developerguide/gamelift-integration.html) 。

要创建会话，请调用`AWSGameLiftClientManager::CreateSession()` 或 `AWSGameLiftClientManager::CreateSessionAsync()`。这将发出一个配置新会话的请求调用。

当会话创建开始时，将在服务器端广播`OnCreateSessionBegin`通知以执行设置操作，例如加载关卡。当会话创建完成且会话处于活动状态时，将在服务器端广播 `OnCreateSessionEnd` 通知以执行任何后续操作。

```cpp
void CreateSessionSync() {
    // Make synchronous call to create a game session with max 2 players
    AWSGameLift::AWSGameLiftCreateSessionRequest request;
    request.m_idempotencyToken = "YourGameLiftSessionId";
    request.m_fleetId = "YourGameLiftFleetId";
    request.m_maxPlayer = 2;
    AZStd::string result = "";
    AWSGameLift::AWSGameLiftSessionRequestBus::BroadcastResult(
        result,
        &AWSGameLift::AWSGameLiftSessionRequestBus::Events::CreateSession, 
        request
    );
}

void CreateSessionAsync() {    
    // Make asynchronous call to create a game session with max 2 players and get response from notification
    AWSGameLift::AWSGameLiftCreateSessionRequest request;
    request.m_idempotencyToken = "YourGameLiftSessionId";
    request.m_fleetId = "YourGameLiftFleetId";
    request.m_maxPlayer = 2;
    AWSGameLift::AWSGameLiftSessionAsyncRequestBus::Broadcast(
        &AWSGameLift::AWSGameLiftSessionAsyncRequestBus::Events::CreateSessionAsync,
        request
    );
} 
```

### `SearchSessions`

搜索并检索与提供的搜索条件匹配的所有活动会话。

要搜索会话，请调用`AWSGameLiftClientManager::SearchSessions()` 或 `AWSGameLiftClientManager::SearchSessionsAsync()`，并传入对包含搜索条件的搜索请求的引用。搜索完成后，您可以从 '`SearchSessionsResponse`' 迭代 '`SessionConfigs`'。

```cpp
void SearchSessionsSync() {
    // Make synchronous call to search active game sessions on a specific fleet
    AWSGameLift::AWSGameLiftSearchSessionsRequest request;
    request.m_fleetId = "YourGameLiftFleetId";
    Multiplayer::SearchSessionsResponse result;
    AWSGameLift::AWSGameLiftSessionRequestBus::BroadcastResult(
        result,
        &AWSGameLift::AWSGameLiftSessionRequestBus::Events::SearchSessions,
        request
    );
}

void SearchSessionsAsync() {
    // Make asynchronous call to search active game sessions on a specific fleet and get response from notification
    AWSGameLift::AWSGameLiftSearchSessionsRequest request;
    request.m_fleetId = "YourGameLiftFleetId";
    AWSGameLift::AWSGameLiftSessionAsyncRequestBus::Broadcast(
        &AWSGameLift::AWSGameLiftSessionAsyncRequestBus::Events::SearchSessionsAsync,
        request
    );
}
```

### `JoinSession`

在游戏会话中保留一个开放的玩家位置，并初始化从客户端到服务器的连接。

要开始允许玩家加入游戏的过程，请调用`AWSGameLiftClientManager::JoinSession()` 或 `AWSGameLiftClientManager::JoinSessionAsync()`，并传入游戏会话 ID 和将加入的玩家 ID。如果两个步骤（保留玩家位置和初始化连接）都成功，则该过程将返回`true`。如果任一步骤失败，则进程返回 `false`。

```cpp
void JoinSessionSync() {
    // Make synchronous call to join a specific session
    AWSGameLift::AWSGameLiftJoinSessionRequest request;
    request.m_sessionId = "YourGameSessionId";
    request.m_playerId= "YourPlayerId";
    bool result = false;
    AWSGameLift::AWSGameLiftSessionRequestBus::BroadcastResult(
        result,
        &AWSGameLift::AWSGameLiftSessionRequestBus::Events::JoinSession,
        request
    );
}

void JoinSessionAsync() {
    // Make asynchronous call to join a specific session and get response from notification
    AWSGameLift::AWSGameLiftJoinSessionRequest request;
    request.m_sessionId = "YourGameSessionId";
    request.m_playerId= "YourPlayerId";
    AWSGameLift::AWSGameLiftSessionAsyncRequestBus::Broadcast(
        &AWSGameLift::AWSGameLiftSessionAsyncRequestBus::Events::JoinSessionAsync,
        request
    );
}
```

### `LeaveSession`

断开玩家与游戏会话的连接。

要离开游戏会话，请调用`AWSGameLiftClientManager::LeaveSession()` 或 `AWSGameLiftClientManager::LeaveSessionAsync()`。

```cpp
void LeaveSessionSync()
{
    // Make synchronous call to leave the current session
    AWSGameLift::AWSGameLiftSessionRequestBus::Broadcast(&AWSGameLift::AWSGameLiftSessionRequestBus::Events::LeaveSession);
}

void LeaveSessionAsync() 
{
    // Make asynchronous call to leave the current session and get notification once the leaving session is completed
    AWSGameLift::AWSGameLiftSessionAsyncRequestBus::Broadcast(&AWSGameLift::AWSGameLiftSessionAsyncRequestBus::Events::LeaveSessionAsync);
}
```

### Destroy session (passively)

作为默认行为，当最后一名玩家离开游戏会话时，多人游戏 Gem 将自动开始终止游戏会话。

## 服务端

### 服务器初始化

您必须通知 Amazon GameLift 服务您的服务器进程已准备好托管游戏会话、处理请求和建立连接。

要发送服务器进程已准备就绪的通知，请完成任何相关的初始化，然后使用`AWSGameLiftServerRequestBus::Events::NotifyGameLiftProcessReady()`。
我们建议在连接到`YourProjectServerSystemComponent`激活步骤中的`Multiplayer::SessionNotificationBus`后进行调用。

```cpp
AWSGameLift::AWSGameLiftServerRequestBus::Broadcast(&AWSGameLift::AWSGameLiftServerRequestBus::Events::NotifyGameLiftProcessReady);
```


### 服务器通知APIs

创建游戏会话后，将通过`Multiplayer::SessionNotificationBus`广播通知。您可以对会话如何响应这些通知进行编程。


### `OnCreateSessionBegin`

当会话开始在服务器上创建时，`Multiplayer::SessionNotificationBus::Events::OnCreateSessionBegin()`通知在服务器端广播。当 GameLift 请求游戏会话时，它将提供相关的运行时配置，其中包括关键游戏会话信息，例如最大玩家数。它还可以包括游戏数据和玩家数据。 在此步骤中，设置特定于游戏会话的属性并在服务器端加载适当的关卡。

```cpp
bool OnCreateSessionBegin(const Multiplayer::SessionConfig& sessionConfig)
{
    ...
}
```


### `OnCreateSessionEnd`

在会话创建过程结束时，`Multiplayer::SessionNotificationBus::Events::OnCreateSessionEnd()` 通知在服务器端广播，以便在创建并激活会话后执行任何后续操作。

```cpp
bool OnCreateSessionEnd()
{
    ...
}
```


### `OnSessionHealthCheck`

当您的服务器进程准备就绪并运行时， `Multiplayer::SessionNotificationBus::Events::OnSessionHealthCheck` 定期调用以报告 Server 进程的运行状况。

您可以在 `OnSessionHealthCheck` 中自定义运行状况检查逻辑。有关更多信息，请参阅 Amazon GameLift 文档中的 [报告服务器进程运行状况](https://docs.aws.amazon.com/gamelift/latest/developerguide/gamelift-sdk-server-api.html#gamelift-sdk-server-health)。

```cpp
bool OnSessionHealthCheck()
{
    ...
}
```


### `OnDestroySessionBegin`

当会话开始终止时，`Multiplayer::SessionNotificationBus::Events::OnDestroySessionBegin` 通知以执行清理操作。在此步骤中，建议在服务器端清理级别数据。

```cpp
bool OnDestroySessionBegin()
{
    ...
}
```

### `OnDestroySessionEnd`

会话终止后，`Multiplayer::SessionNotificationBus::Events::OnDestroySessionEnd`对于任何后续操作，如关闭应用程序进程等，都会广播通知。

```cpp
bool OnDestroySessionEnd()
{
    ...
}
```
