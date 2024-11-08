---
linkTitle: 高级主题
title: AWS GameLift Gem 高级主题
description: 了解在 Open 3D Engine （O3DE） 中使用 AWS GameLift Gem 的一些高级方法。
toc: true
weight: 500
---

本主题介绍了使用 **AWS GameLift Gem 的一些高级方法。


## 使用资源映射文件查找队列

*GameLift 队列* 是一组 Amazon EC2 虚拟机，称为*实例*，代表您的托管资源。*队列 ID* 是 Amazon GameLift 生成的每个队列的唯一标识符。

在大多数情况下，您的游戏将使用多个队组。为避免在脚本中使用固定队列 ID，您可以使用资源映射文件，该文件允许您在运行时查找队列 ID。使用资源映射文件有助于保持代码或脚本的整洁和一致。有关更多信息，请参阅 [资源映射文件](/docs/user-guide/gems/reference/aws/aws-core/resource-mapping-files/)。

以下示例使用 [Script Canvas](/docs/user-guide/scripting/script-canvas/)，但您也可以使用 [AWS GameLift Gem C++ API](/docs/api/gems/awsgamelift/)。


### 示例：创建一个游戏会话，每个游戏组最多有 1 个玩家

此示例脚本演示如何从资源映射文件中查找队列。然后，使用队组创建一个允许最多一个玩家的会话。（您也可以使用 AWS GameLift Gem C++ API 执行此示例。

在 Script Canvas 中，执行以下步骤：

1. 创建`AWSGameLiftCreateSessionRequest`类型的变量并命名为`request`。

2. 通过变量的getter节点获取 `request`的对象。

3. 使用 **GetResourceNameId** 节点查找资源键为“`MyGameLiftFleetId`”的队组。

4. 使用 **FleetId** setter 节点设置“`request`”对象的“`FleetId`”。

5. 使用 **MaxPlayer** setter 节点设置 '`request`' 对象的最大玩家数。

6. 使用 **CreateSessionAsync** 节点创建游戏会话。

![A script that gets a fleet from a resource mapping file and creates a session with 1 maximum player.](/images/user-guide/gems/reference/aws/aws-gamelift/createsessionandresourcemapping.PNG)


### 示例：跨多个队组搜索游戏会话

此示例脚本向您展示如何使用资源映射文件跨多个队组搜索活动游戏会话。如果您有多个队列，则可以将队列 ID 存储在数组中。然后，您可以遍历每个队组，并在相应的队组中搜索活动游戏会话。（您也可以使用 AWS GameLift Gem C++ API 执行此示例。

在 Script Canvas 中，执行以下步骤：

1. 使用 **Get fleetId Keys** 和 **For Each** 节点迭代“`FleetId`”数组。

2. 对于每个队列 ID：

   - 使用 **GetResourceNameId** 节点查找具有相应资源键的队列。

   - 使用变量的 getter 节点获取 '`request`' 对象。

   - 使用 **FleetId** setter 节点设置“`request`”对象的“`FleetId`”。

   - 使用 **SearchSessionsAsync** 节点在队列中搜索活动且可加入的会话。

![A script that iterates through an array of fleets and searches for an active session.](/images/user-guide/gems/reference/aws/aws-gamelift/searchsessionsandresourcemapping.PNG)


## 搜索和加入会话

搜索会话的最简单方法是浏览服务器，它显示了所有可用的游戏。然后，玩家可以根据他们想要玩的类型筛选游戏，选择游戏并加入会话。

以下示例使用 [Script Canvas](/docs/user-guide/scripting/script-canvas/)，但您也可以使用 [AWS GameLift Gem C++ API](/docs/api/gems/awsgamelift/)。


### 示例：根据筛选条件搜索会话

此示例脚本演示如何在相应的队列中搜索最多 10 个会话。

在 Script Canvas 中，执行以下步骤：

1. 创建一个类型为“`AWSGameLiftSearchSessionRequest`”的变量，并将其命名为“`searchrequest`”。

2. 使用变量的 getter 节点获取 '`searchrequest`' 的对象。

3. 使用 **GetResourceNameId** 节点查找资源键为“`MyGameLiftFleetId`”的队组。

4. 使用 **FleetId** setter 节点设置“`searchrequest`”对象的“`FleetId`”。

5. 使用 **MaxResult** setter 节点按最大会话数筛选 '`searchrequest`' 的对象。

7. 使用 **SearchSessionsAsync** 节点搜索会话。

![A script that searches for a maximum of ten sessions in the corresponding fleet.](/images/user-guide/gems/reference/aws/aws-gamelift/searchactivesessions.PNG)


### 示例：浏览搜索结果并加入会话

从上一个示例获取搜索结果后，此脚本将选择第一个游戏会话并加入该会话。该脚本在 **SearchSessionsAsync** 完成并显示结果后自动启动。

1. 创建一个 **AWSGameLiftSessionAsyncRequestNotificationBus** 节点。然后，通过添加 **OnSearchSessionsAsyncComplete** 事件，将此脚本设置为在搜索会话后自动开始。这将返回 '`SearchSessionsResponse`' 对象。

2. 使用 **SessionConfigs** 节点从 '`SearchSessionsResponse`' 对象获取 '`SessionConfig`' 数组。

3. 使用 **Get First Element** 节点从数组中获取第一个 '`SessionConfig`' 对象。

4. 使用 **SessionId** setter 节点设置 '`SessionConfig`' 对象的 '`SessionId`'。

5. 创建一个类型为“`AWSGameLiftJoinSessionRequest`”的变量，并将其命名为“`joinrequest`”。

6. 使用 **SessionId** setter 节点，将 '`joinrequest`' 对象的会话 ID 设置为 '`SessionConfig`' 的会话 ID。

7. 使用 **CreatePlayerId** 节点为会话创建玩家 ID。

8. 使用 **PlayerId ** setter 节点设置 '`joinrequest`' 对象的 '`PlayerId`'。

9. 允许玩家使用 **JoinSessionAsync** 节点加入会话。

![A script that chooses a game from search results and joins the first session.](/images/user-guide/gems/reference/aws/aws-gamelift/searchandjoin.PNG)


## 从 Amazon Cognito 身份池获取凭证

如果您的客户端需要与 AWS 资源交互，您可以使用临时 AWS 凭证为您的资源提供有限的权限。您的客户端可能需要与 AWS 资源交互，以向 GameLift 发出请求，并允许玩家与您的资源交互。借助 [AWS 客户端身份验证 Gem](/docs/user-guide/gems/reference/aws/aws-client-auth/)，您可以使用玩家的登录信息从 Amazon Cognito 身份池获取其 AWS 凭证。

要从身份池获取玩家的凭证，他们必须成功登录。玩家登录后，您可以使用 AWS GameLift Gem 配置客户端，并通过调用`AWSGameLiftClientManager::ConfigureGameLiftClient()`来设置其 AWS 凭证。


## 为多个服务器进程设置日志路径

您可以将 GameLift 队列配置为在每个实例上运行多个进程。这可以帮助您更高效地使用托管资源，并可能有助于降低总体托管成本。有关更多信息，请参阅《Amazon GameLift 开发人员指南》中的 [管理启动游戏服务器以进行托管的方式](https://docs.aws.amazon.com/gamelift/latest/developerguide/fleets-multiprocess.html) 。

要支持对同一 GameLift 实例上的多个进程进行日志记录，请输入以下命令。您可以通过在 '`project-path`' 启动参数中提供唯一标识符来为服务器进程设置唯一的日志路径。在此示例中，日志文件保存到 '`C:\game\process1\user\log\`' 而不是默认日志路径。

```cmd
C:\game\bin\server.exe --engine-path=C:\game --project-path=C:\game\process1 --project-cache-path=C:\game\assets -bg_ConnectToAssetProcessor= 0
```
