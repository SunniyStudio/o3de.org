---
linkTitle: 本地测试
title: AWS GameLift Gem 使用 GameLift Anywhere 进行本地测试
description: "了解如何在 O3DE 中使用 AWS GameLift Gem 进行本地测试"
toc: true
weight: 600
---

在本主题中，您将了解如何使用 [Amazon GameLift Anywhere](https://docs.aws.amazon.com/gamelift/latest/developerguide/fleets-creating-anywhere.html) 在本地计算机上测试和验证 AWS GameLift Gem 功能集成。

GameLift Anywhere 可用于将环境中的硬件集成到 Amazon GameLift 游戏托管中。Amazon GameLift Anywhere 在 Anywhere 队列中向 Amazon GameLift 注册您的硬件;您可以使用自己的硬件迭代测试和构建 Game Server 项目。

使用 GameLift Anywhere，您可以验证以下内容：

* 您的游戏服务器已与 Server SDK 正确集成，并且正在与 GameLift 服务正确通信，以启动新的游戏会话、接受新玩家并报告运行状况和状态。
    
* 您的游戏客户端已与适用于 GameLift 的 AWS 开发工具包正确集成，并且能够检索有关现有游戏会话的信息、启动新的游戏会话、加入玩家游戏以及连接到游戏会话。
    

## 1. 创建位置

为您的自定义资源创建一个位置。自定义位置标记 Amazon GameLift 用于在 Anywhere 队列中运行游戏的硬件的位置。
创建自定义位置时，位置名称必须以“`custom-`”开头。

```sh
aws gamelift create-location --location-name custom-location-1 --region <Region>
```


## 2. 创建 Anywhere 队列

与创建常规 AWS 队列相比，创建 Anywhere 队列的过程要快得多，因为常规 AWS 队列通常需要大约一个小时才能完成设置。

```sh
aws gamelift create-fleet --name AnywhereFleet --compute-type ANYWHERE --locations Location=custom-location-1 --region <Region>
```
记录 '`FleetId`' 以用于后续步骤。例：**fleet-1a23bc4d-456e-78fg-h9i0-jk1l23456789**


## 3. 将本地计算机注册为 Compute

将您的本地计算机注册为 GameLift Anywhere Compute。
为了便于测试，我们假设 Server 和 Client 将在同一台机器上运行;因此我们可以将 localhost （'`127.0.0.1`'） 作为 IP 地址传递。
如果您的计算机可通过公共 IP 地址访问，请根据需要更改该值。

```sh
aws gamelift register-compute --compute-name CustomCompute1 --fleet-id <FleetId> --ip-address 127.0.0.1 --location custom-location-1 --region <Region>
```

此外，检索 compute auth 令牌，因为它对于后续步骤是必需的。

```sh
aws gamelift get-compute-auth-token --fleet-id <FleedId> --compute-name CustomCompute1
```

## 4. 在计算机上启动 Game Server 可执行文件的实例

当您准备好使用 GameLift 进行测试时，通过将“`sv_gameLiftEnabled`”和“`sv_gameliftAnywhereEnabled`”CVAR 设置为“`true`”来选择加入。默认情况下，这些 CVAR 处于关闭状态，以便于在将 GameLift 服务器开发工具包集成到您的游戏服务器之前进行本地测试。

您还需要通过以下 CVAR 填写服务器参数：
- `sv_gameliftAnywhereWebSocketUrl`
- `sv_gameliftAnywhereAuthToken`
- `sv_gameliftAnywhereFleetId`
- `sv_gameliftAnywhereHostId`
- `sv_gameliftAnywhereProcessId`

除了以下说明外，这些属性的所有值都在前面的步骤中检索：
- 应将“`HostId`”填充为“`ComputeName`”。
- 可以省略“`ProcessId`”。将从时间戳中生成唯一的默认 '`ProcessId`'。

要设置这些值，您可以在启动可执行文件时（通过命令行或在 Visual Studio 解决方案中）传递它们，如下所示：

```sh
C:\GameLiftPackageWindows\MultiplayerSample.ServerLauncher.exe --sv_gameLiftEnabled=true --sv_gameliftAnywhereEnabled=true --sv_gameliftAnywhereWebSocketUrl="<WebSocketUrl>" --sv_gameliftAnywhereAuthToken="<AuthToken>" --sv_gameliftAnywhereFleetId="<FleetId>" --sv_gameliftAnywhereHostId="<ComputeName>" --sv_gameliftAnywhereProcessId="<ProcessId>"
```
或者，您可以通过在 `MyProject/Registry` 文件夹中创建名为 `commands.MyProject_serverlauncher.setreg`，其中包含以下内容：

```json
{
    "Amazon":
    {
        "AzCore":
        {
            "Runtime":
            {
                "ConsoleCommands":
                {
                    "sv_gameLiftEnabled": "true",
                    "sv_gameliftAnywhereEnabled": "true",
                    "sv_gameliftAnywhereWebSocketUrl": "<WebSocketUrl>",
                    "sv_gameliftAnywhereAuthToken": "<AuthToken>",
                    "sv_gameliftAnywhereFleetId": "<FleetId>",
                    "sv_gameliftAnywhereHostId": "<ComputeName>",
                    "sv_gameliftAnywhereProcessId": "<ProcessId>"
                } 
            } 
        } 
    } 
}
```

这样，无论服务器是如何构建的，都将应用这些值。


## 5.开始游戏

确保你有游戏的 cmake 生成目标，如 YourProject.GameLauncher，并生成用于本地测试的应用程序。
您应该能够在 build bin folder 下找到 application，然后启动游戏。

## 6.测试游戏和服务器

测试步骤和说明实际上取决于你自己的项目，但请确保你的测试可以涵盖 CreateSession、JoinSession、LeaveSession 和 DestroySession 用例。

您还可以使用 GameLift Local 工具验证交互和日志。有关更多信息，请参阅《Amazon GameLift 开发人员指南》中的 [测试游戏服务器和客户端](https://docs.aws.amazon.com/gamelift/latest/developerguide/integration-testing-local.html#integration-testing-local-client)。
