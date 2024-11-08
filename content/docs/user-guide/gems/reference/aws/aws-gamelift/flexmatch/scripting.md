---
linkTitle: FlexMatch 脚本编程
title: FlexMatch 脚本编程
description: 在 Open 3D Engine （O3DE） 中的 AWS GameLift Gem 中了解用于玩家匹配的 Script Canvas 节点。
toc: true
weight: 400
---


**AWS GameLift** 提供 Script Canvas 节点，这些节点可向 Amazon GameLift 发出请求以启动、停止和接受对战请求。

每个操作都有同步和异步版本。异步节点在 AZ JobFunction 中执行其操作，并在将来的某个时间点完成。这些操作通过 AWS HTTPS 请求或 TCP/UDP 数据包通过网络进行通信。每个异步节点都有相应的通知处理程序节点。


## StartMatchmaking
要查找一组玩家的对战并创建会话来托管对战，请使用 '`StartMatchmaking`' 节点。
使用此节点，您可以通过“`AWSGameLiftStartMatchmakingRequest`”传入属性`configuration name`、`players`和`ticket id` 。

### StartMatchmaking 案例图表

![StartMatchmaking sample graph](/images/user-guide/gems/reference/aws/aws-gamelift/startmatchmaking.PNG)


### StartMatchmakingAsync 案例图表

![StartMatchmakingAsync sample graph](/images/user-guide/gems/reference/aws/aws-gamelift/startmatchmakingasync.PNG)


## StopMatchmaking
要取消当前正在处理的对战票证，请使用 '`StopMatchmaking`' 节点。
使用此节点，您可以通过“`AWSGameLiftStopMatchmakingRequest`”传入属性“`ticket id`”。

### StopMatchmaking 案例图表

![StopMatchmaking sample graph](/images/user-guide/gems/reference/aws/aws-gamelift/stopmatchmaking.PNG)


### StopMatchmakingAsync 案例图表

![StopMatchmakingAsync sample graph](/images/user-guide/gems/reference/aws/aws-gamelift/stopmatchmakingasync.PNG)


## AcceptMatch
要注册玩家接受或拒绝建议的匹配，请使用 '`AcceptMatch`' 节点。
使用此节点，您可以通过“`AWSGameLiftAcceptMatchRequest`”传入属性`accept match`, `player ids` 和 `ticket id`。

### AcceptMatch 案例图表

![AcceptMatch sample graph](/images/user-guide/gems/reference/aws/aws-gamelift/acceptmatch.PNG)


### AcceptMatchAsync 案例图表

![AcceptMatchAsync sample graph](/images/user-guide/gems/reference/aws/aws-gamelift/acceptmatchasync.PNG)


## StartPolling
请求启动本地进程，以根据给定的票证 ID 和玩家 ID 轮询对战票证。


### StartPolling sample graph

![StartPolling sample graph](/images/user-guide/gems/reference/aws/aws-gamelift/startpolling.PNG)


## StopPolling
请求停止轮询对战票证的本地进程。


### StopPolling sample graph

![StopPolling sample graph](/images/user-guide/gems/reference/aws/aws-gamelift/stoppolling.PNG)


## Matchmaking notifications

要侦听的对战通知，用于根据对战票证事件执行所需的操作。


### OnMatchAcceptance node

OnMatchAcceptance 在找到匹配项并等待接受时触发。使用此通知接受找到的匹配项。

![OnMatchAcceptance node](/images/user-guide/gems/reference/aws/aws-gamelift/onmatchacceptance.PNG)


### OnMatchComplete node

OnMatchComplete 在匹配完成时触发。

![OnMatchComplete node](/images/user-guide/gems/reference/aws/aws-gamelift/onmatchcomplete.PNG)


### OnMatchError node

OnMatchError 在处理 match 时触发错误。

![OnMatchError node](/images/user-guide/gems/reference/aws/aws-gamelift/onmatcherror.PNG)


### OnMatchFailure node

OnMatchFailure 在匹配无法完成时触发。

![OnMatchFailure node](/images/user-guide/gems/reference/aws/aws-gamelift/onmatchfailure.PNG)
