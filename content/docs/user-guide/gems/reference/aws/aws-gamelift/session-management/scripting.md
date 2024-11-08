---
linkTitle: Session Management 脚本编程
title: Session Management 脚本编程
description: 在 Open 3D Engine （O3DE） 中的 AWS GameLift Gem 中了解用于多人游戏会话管理的 Script Canvas 节点。
toc: true
weight: 400
---


**AWS GameLift** 提供 Script Canvas 节点，这些节点可向 Amazon GameLift 发出请求以创建、搜索、加入和离开游戏会话。

每个操作都有同步和异步版本。异步节点在 AZ JobFunction 中执行其操作，并在将来的某个时间点完成。这些操作通过 AWS HTTPS 请求或 TCP/UDP 数据包通过网络进行通信。每个异步节点都有相应的通知处理程序节点。

## ConfigureGameLiftClient

要对 GameLift 发出请求，您必须使用**ConfigureGameLiftClient** 节点创建适当的 GameLift 客户端。使用此节点，您可以指定 GameLift 队组创建和驻留的 AWS 区域。

请注意，您必须以正确的格式指定 AWS 区域。例如，对于美国东部（弗吉尼亚北部）区域，请指定 **us-east-1**。有关受支持区域的列表，请参阅 AWS 一般参考中的 [Amazon GameLift 终端节点和配额](https://docs.aws.amazon.com/general/latest/gr/gamelift.html)。

### ConfigureGameLiftClient sample graph

![ConfigureGameLiftClient sample graph](/images/user-guide/gems/reference/aws/aws-gamelift/configureclient.PNG)


## CreateSession

使用 **CreateSession** 节点创建会话。使用 **AWSGameLiftCreateSessionRequest** 节点或 **AWSGameLiftCreateSessionOnQueueRequest** 节点配置会话。
建议在队列后面异步创建 session。请参阅 [GameLift 游戏会话队列的最佳实践](https://docs.aws.amazon.com/gamelift/latest/developerguide/queues-best-practices.html)。
成功创建会话后，游戏的其他实例可以使用 **SearchSessions** 节点发现它，然后使用 **JoinSession** 节点加入它。

### CreateSession sample graph

![CreateSession sample graph](/images/user-guide/gems/reference/aws/aws-gamelift/createsession.PNG)


### CreateSessionAsync sample graph

![CreateSessionAsync sample graph](/images/user-guide/gems/reference/aws/aws-gamelift/createsessionasync.PNG)


### AWSGameLiftCreateSessionOnQueue sample graph
![AWSGameLiftCreateSessionOnQueue sample graph](/images/user-guide/gems/reference/aws/aws-gamelift/createsessiononqueue.PNG)


### AWSGameLiftCreateSessionOnQueueAsync sample graph
![AWSGameLiftCreateSessionOnQueueAsync sample graph](/images/user-guide/gems/reference/aws/aws-gamelift/createsessiononqueueasync.PNG)


## SearchSessions

使用 SearchSessions 节点获取当前处于活动状态且可加入的游戏会话的列表。您可以使用 **AWSGameLiftSearchSessionsRequest** 节点指定搜索条件。

成功后，SearchSessions 节点将返回 **SearchSessionsResponse** 对象列表，这些对象提供找到的符合搜索条件的会话的详细信息。

### SearchSessions sample graph

![SearchSessions sample graph](/images/user-guide/gems/reference/aws/aws-gamelift/searchsessions.PNG)
  

### SearchSessionsAsync sample graph

![SearchSessionsAsync sample graph](/images/user-guide/gems/reference/aws/aws-gamelift/searchsessionasync.PNG)


## JoinSession

搜索会话后，您可以使用 '`JoinSession`' 节点加入会话。使用从 SearchSession 检索到的`game session id`指定会话。如果游戏成功连接到服务器，您将自动前往服务器的地图并加入游戏。

使用此节点，您可以通过“`AWSGameLiftJoinSessionRequest`”传入属性`game session id` 和 `player id`。
  
### JoinSession sample graph

![JoinSession sample graph](/images/user-guide/gems/reference/aws/aws-gamelift/joinsession.PNG)


### JoinSessionAsync sample graph

![JoinSessionAsync sample graph](/images/user-guide/gems/reference/aws/aws-gamelift/joinsessionasync.PNG)


## LeaveSession

要断开与已加入会话的连接，请使用 **LeaveSession** 节点。


### LeaveSession sample graph

![LeaveSession sample graph](/images/user-guide/gems/reference/aws/aws-gamelift/leavesession.PNG)  


### LeaveSessionAsync sample graph

![LeaveSessionAsync sample graph](/images/user-guide/gems/reference/aws/aws-gamelift/leavesessionasync.PNG)  
