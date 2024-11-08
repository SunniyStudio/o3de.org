---
linkTitle: Resource Management
title: AWS GameLift Gem Resource Management
description: "了解在 O3DE 中使用 AWS GameLift Gem 的示例 AWS CDK 应用程序"
toc: true
weight: 900
---

AWS GameLift Gem 提供了一个示例 [AWS Cloud Development Kit](https://aws.amazon.com/cdk/)(AWS CDK)  应用程序，可用于建模和部署以下 Amazon GameLift 资源：
 
*    一个[GameLift fleets](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-gamelift-fleet.html) 列表保存游戏服务器。
*   (可选) 一个 [GameLift builds](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-gamelift-build.html)列表用于 GameLift 舰队生成。
*   (可选) 一个 [Alias](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-gamelift-alias.html) 用于每个 GameLift 队列目标。
*   (可选) 一个 [game session queue](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-gamelift-gamesessionqueue.html) 处理新游戏会话的请求。建议使用队列作为放置游戏会话的主要机制。有关更多信息，请参阅.
* 0[Amazon GameLift Queues Intro](https://docs.aws.amazon.com/gamelift/latest/developerguide/queues-intro.html)文档。

## 先决条件

要部署 AWS CDK 应用程序，您必须具备以下条件：

- [AWS CLI](https://aws.amazon.com/cli/) 和 [AWS Cloud Development Kit](https://aws.amazon.com/cdk/) 安装在本地计算机上。
- 您的 AWS 凭证已设置。有关设置 AWS 凭证的说明，请参阅 [为 O3DE 配置 AWS 凭证](/docs/user-guide/gems/reference/aws/aws-core/configuring-credentials/)。

## 设置

要了解如何设置虚拟环境和可用环境变量，请参阅 AWS GameLift Gem 的自述文件，网址为`/Gems/AWSGameLift/cdk/README/`.

## 准备服务器包

准备一个可以上传到 GameLift 的服务器包。有关更多信息，请参阅 [适用于 Windows 的 AWS GameLift Gem 生成包打包](build-packaging-for-windows/).

## 更新队列配置

在部署 AWS CDK 应用程序之前，您必须更新在`/Gems/AWSGameLift/cdk/aws_gamelift/fleet_configurations.py`.

您可以在代码注释或 AWS CloudFormation用户指南的 [Amazon GameLift 资源类型参考](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/AWS_GameLift.html)  中找到每个字段的描述。

有关配置 GameLift 队组以适合您的应用程序的最佳实践，请参阅 Amazon GameLift开发者指南的[GameLift 队组设计指南](https://docs.aws.amazon.com/gamelift/latest/developerguide/fleets-design.html)。


### Fleet alias

为了更轻松地将玩家流量从一个队组切换到另一个队组，您可以为每个 GameLift 队组目标定义一个别名。

要定义队列别名，请在队列的配置中添加以下数据块：
```bash
'alias_configuration': {

    # (Required) A descriptive label that is associated with an alias. Alias names do not need to be unique.
    'name': '<alias name>',

    # (Conditional) A type of routing strategy for the GameLift fleet alias if exists.
    # Required if alias_configuration is provided.
    'routing_strategy': {

        # The message text to be used with a terminal routing strategy.
        # If you specify TERMINAL for the Type property, you must specify this property.
        # Required if specify TERMINAL for the Type property,
        'message': '',

        # (Required) A type of routing strategy.
        'type': '<routing strategy type>'

    }
}
```


### GameLift build

要创建队列，您可以在队列配置中提供以下内容之一：
- 现有 GameLift 内部版本 ID
- 压缩的本地服务器包的路径

##### 使用现有 GameLift 内部版本

要指定现有生成 ID，请编辑队列的生成配置，例如以下示例。

```bash
'build_configuration': {

    # (Required) A unique identifier for a build to be deployed on the new fleet.
    'build_id': '<existing build id>'

}
```

##### 上传本地服务器包

要从磁盘上传压缩的服务器包并自动创建 GameLift 版本，请提供本地文件路径和用于运行服务器的操作系统。由于使用 S3 来存储包文件，因此此功能会产生额外费用。

```bash
'build_configuration': {

    # (Required) The disk location of the local build file(zip).
    'build_path': '<path to the local build file>',

    # (Required) The operating system that the game server binaries are built to run on.
    'operating_system': 'WINDOWS_2012'

}
```

{{< caution >}}
如果要将服务器部署到 Amazon Linux 2 实例，则必须指定“`AMAZON_LINUX_2`”作为操作系统。
{{< /caution >}}

### 运行时配置

要为每个实例运行多个游戏服务器进程，您可以设置队组的运行时配置。[AWS GameLift Gem 高级主题](advanced-topics/)页面介绍了此设置的好处，以及支持每个服务器进程的唯一日志路径所需的启动参数。

以下示例在队列上运行两个 Windows Server 进程，并将日志文件写入“`C:\game`”下的不同子文件夹：

```bash
'server_processes': [
    {
        # (Required) The number of server processes using this configuration that
        # run concurrently on each instance.
        # Provide any integer not less than 1.
        'concurrent_executions': 1,
        
        # (Required) The location of a game build executable that contains the Init() function.
        # Game builds are installed on instances at the root:
        # Windows (custom game builds only): C:\game.
        # Linux: /local/game.
        'launch_path': 'C:\game\bin\server.exe',
        
        # (Optional) An optional list of parameters to pass to the server executable on launch.
        'parameters': '--sv_port 33450 --project-path=C:\game\process1 '
                    '--project-cache-path=C:\game\assets --engine-path=C:\game '
                    '-bg_ConnectToAssetProcessor=0'
    },
    {
        'concurrent_executions': 1,
        'launch_path': 'C:\game\bin\server.exe',
        'parameters': '--sv_port 33451 --project-path=C:\game\process2 '
                    '--project-cache-path=C:\game\assets --engine-path=C:\game '
                    '-bg_ConnectToAssetProcessor=0'
    }

]
```

## （可选）更新 FlexMatch 配置

如果您的游戏支持 FlexMatch，则需要在部署 AWS CDK 应用程序之前更新在`/Gems/AWSGameLift/cdk/aws_gamelift/flexmatch/flexmatch_configurations.py`中定义的 FlexMatch 配置。

### 规则集正文

对战规则的集合，格式为 JSON 字符串，如以下示例所示。

有关设计对战规则集的说明，请查看 [设计 FlexMatch 规则集](https://docs.aws.amazon.com/gamelift/latest/flexmatchguide/match-design-ruleset.html).

```bash
RULE_SET_BODY = '{"ruleLanguageVersion":"1.0","teams":[{"name":"Players","maxPlayers":4,"minPlayers":2}]}'
```

### Acceptance required

一个标志，用于确定使用此配置创建的对战游戏是否必须被匹配的玩家接受。要要求接受，请设置为 True。

### Required timeout

对战票证在超时之前可以保持进程中的最长持续时间（以秒为单位）。

### Additional player count

对战中对未来玩家开放的玩家槽数。

### Backfill mode

用于回填使用此对战配置创建的游戏会话的方法。

当您的游戏手动管理回填请求或不使用对战回填功能时，请指定 MANUAL。
指定 AUTOMATIC 让 GameLift 在游戏会话有一个或多个空槽时创建 StartMatchBackfill 请求。

```bash
BACKFILL_MODE = 'AUTOMATIC'
```


## 导航到 AWS CDK 应用程序文件夹

要设置 Python 环境并部署 AWS CDK 应用程序，请打开命令行窗口并导航到 AWS GameLift Gem 的“cdk”文件夹。

```cmd
$ cd o3de\Gems\AWSGameLift\cdk
```


## Synthesize stacks

要在更新队列配置后创建 AWS CloudFormation 模板，您必须合成 AWS CDK 应用程序中定义的堆栈。合成步骤可捕获定义 AWS 资源时的逻辑错误。


### 使用现有 GameLift 内部版本

如果队列配置文件中有现有的 GameLift 内部版本 ID，您可以通过在 AWS CDK 应用程序根文件夹下运行以下 AWS CLI 命令来合成堆栈：

```cmd
$ cdk synth
```


### 启用可选功能

#### 使用支持堆栈上传

如果您需要AWS CDK应用程序上传本地程序包并相应地创建 GameLift 版本，则必须启用“`upload-with-support-stack`”上下文变量：

```cmd
$ cdk synth -c upload-with-support-stack=true --all
```


启用此可选功能后，将部署额外的 AWS CloudFormation 堆栈。额外的堆栈包含支持生成包上传和创建所需的 AWS 资源。'`--all`' 参数告诉 AWS CDK 应用程序合成所有可用的堆栈。

#### 创建游戏会话队列

建议您在合成堆栈时提供“`create_game_session_queue`”上下文变量，使用此 AWS CDK 应用程序创建可选的游戏会话队列。以下示例命令合成启用了此可选功能的应用程序：

```cmd
$ cdk synth -c create_game_session_queue=true
```


#### 创建 FlexMatch 资源

您还可以使用此 AWS CDK 应用程序创建对战配置和对战规则集，方法是在合成堆栈时提供“`flex_match`”上下文变量。以下示例命令合成启用了此可选功能的应用程序：

```cmd
$ cdk synth -c flex_match=true
```


## 部署 AWS 资源

### 使用现有 GameLift 版本进行部署

如果您在队组配置文件中提供现有的 GameLift 内部版本 ID，则可以部署定义所有AWS资源的AWS CDK应用程序。要部署 CDK 应用程序，请在应用程序根文件夹下运行以下 AWS CLI 命令：

```cmd
$ cdk deploy
```


### 部署可选功能

与使用“`synth`”命令类似，如果您希望AWS CDK应用程序上传本地软件包并自动创建 GameLift 版本，则必须提供上下文变量“`create_game_session_queue`”和“`--all`”：

```cmd
$ cdk deploy -c upload-with-support-stack=true --all
```

要部署此 AWS CDK 启用了可选功能的应用程序，您必须在部署堆栈时提供相应的上下文变量。以下示例命令在启用所有可选功能的情况下部署应用程序：

```cmd
$ cdk deploy -c upload-with-support-stack=true -c create_game_session_queue=true -c flex_match=true --all
```


## 更新 AWS CDK 应用程序

要更新现有的 AWS CDK 应用程序，请重新运行用于合成和部署应用程序的相同命令。

## 销毁 AWS CDK 应用程序

要销毁 AWS CDK 应用程序部署的所有 AWS 资源，请运行以下 AWS CLI 命令：

```cmd
$ cdk destroy
```

如果您启用了任何可选功能，则可以通过提供相应的上下文变量和“`--all`”参数来销毁启用了所有可选功能的 AWS CDK 应用程序。以下命令将销毁启用了所有可选功能的 AWS CDK 应用程序：

```cmd
$ cdk -c upload-with-support-stack=true -c create_game_session_queue=true -c flex_match=true --all
```
