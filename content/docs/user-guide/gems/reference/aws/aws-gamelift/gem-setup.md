---
linkTitle: 安装
title: AWS GameLift Gem 安装
description: 了解如何在 Open 3D Engine （O3DE） 中设置 AWS GameLift Gem。
toc: true
weight: 200
---

本主题将介绍如何在 Open 3D Engine （O3DE） 中设置 **AWS GameLift** Gem，以便您可以在项目中使用 Amazon GameLift。

## 1. 了解 GameLift

GameLift 允许玩家通过创建游戏会话来连接到您的游戏。有关更多信息，请参阅 Amazon GameLift 开发人员指南 中的 [玩家如何连接到游戏](https://docs.aws.amazon.com/gamelift/latest/developerguide/game-sessions-intro.html) 。

游戏会话是在服务器上运行的具有给定属性集的游戏实例。游戏会话可以是公共的，以便其他玩家可以找到并加入它，也可以是私有的，以便只有收到邀请或通知的玩家才能加入。 

会话的生命周期包括以下阶段：
   1. 使用所需设置创建新会话。 
   2. 等待玩家请求加入会话。 
   3. 保留想要加入的玩家。 
   4. 满足条件时销毁会话。例如，当最后一个玩家离开会话时。


## 2. 启用 AWS GameLift Gem


### 依赖

AWS GameLift Gem 依赖于以下 Gem：

- [AWS Core Gem](/docs/user-guide/gems/reference/aws/aws-core): 提供在 O3DE 中使用 AWS 服务的框架。
- [Multiplayer Gem](/docs/user-guide/gems/reference/multiplayer/multiplayer-gem/): 通过扩展网络框架来提供多人游戏功能，如连接和托管。


### 启用 AWS GameLift Gem 及其依赖项

要启用 AWS GameLift Gem 在您的项目中：
1. 打开 **Project Manager**.
2. 打开项目下的菜单，然后选择 **Edit Project Settings...**.
3. 选择 **Configure Gems**.
4. 启用 AWS GameLift Gem 和依赖 Gem。


### 包括 AWS GameLift Gem 静态库

您必须在项目的 CMake 构建目标中包含 AWS GameLift Gem 静态库。

1. **(必须)** 您必须将**Gem::AWSGameLift.Server.Static**作为项目服务器目标的 **BUILD_DEPENDENCIES** 之一。有关配置了不同服务器和客户端目标的联网项目的示例，查看[O3DE 多人游戏项目模板](https://github.com/o3de/o3de-extras/blob/development/Templates/Multiplayer/Template/Gem/Code/CMakeLists.txt) 或 [O3DE 多人游戏案例项目](https://github.com/o3de/o3de-multiplayersample/blob/development/Gem/Code/CMakeLists.txt)。

    ```cpp
    ly_add_target(
        NAME YourProject.Server.Static STATIC
        ...
        BUILD_DEPENDENCIES
            PUBLIC
            ...
            PRIVATE
                ...
                Gem::AWSGameLift.Server.Static
    )
    ```

2. **(必须)** 要确保 Amazon GameLift Server 开发工具包正确初始化，您必须指示您的项目服务器系统组件需要“`AWSGameLiftServerService`”。

   ```cpp
    void YourProjectServerSystemComponent::GetRequiredServices(AZ::ComponentDescriptor::DependencyArrayType& required)
    {
        ...
        required.push_back(AZ_CRC_CE("AWSGameLiftServerService"));
        ...
    }
   ```

3. **(可选)**  客户端不需要包含 GameLift 客户端库即可加入游戏会话。 [建议客户端不要直接向 GameLift 发出请求](https://docs.aws.amazon.com/gamelift/latest/developerguide/gamelift_quickstart_customservers_designbackend.html)，相反，它们连接到游戏平台服务器，这些服务器代表玩家发出所有 GameLift 服务请求，并提供帐户登录、身份管理、更新和有关游戏模式的信息。

但是，如果您需要在开发或测试期间直接从客户端发出 GameLift 服务请求C++，则必须将 **Gem::AWSGameLift.Client.Static** 作为客户端目标的 **BUILD_DEPENDENCIES** 包含在内。有关 Amazon GameLift 服务 API 的更多信息，请参阅 [GameLift 服务 API 参考](https://docs.aws.amazon.com/gamelift/latest/developerguide/reference-awssdk.html)。

    ```cpp
    ly_add_target(
        NAME YourProject.Client.Static STATIC
        ...
        BUILD_DEPENDENCIES
        PUBLIC
            ...
        PRIVATE
            ...
            Gem::AWSCore.Static
            Gem::AWSGameLift.Client.Static
    )
    ```

## 3. 集成游戏和专用服务器

勾选 [会话管理集成](session-management/integration/) 来管理游戏和专用服务器内的游戏会话。
要支持可选的 FlexMatch 功能，请选中 [FlexMatch 集成](flexmatch/integration/)。


## 4. Set up AWS credentials

要在 O3DE 中使用 AWS 资源，您必须为用户设置 AWS 凭证。

有关在 O3DE 中配置 AWS 凭证的更多详细信息，请参阅 [配置 AWS 凭证](/docs/user-guide/gems/reference/aws/aws-core/configuring-credentials/)。

请检查 [GameLift 的 IAM 策略示例](https://docs.aws.amazon.com/gamelift/latest/developerguide/gamelift-iam-policy-examples.html)以配置使用 GameLift 服务所需的适当权限。

要使用 FlexMatch 功能进行对战和回填，请检查 [设置 FlexMatch](https://docs.aws.amazon.com/gamelift/latest/flexmatchguide/match-setting-up.html)以获取所需的权限。

{{< note >}}
只有针对 GameLift 服务执行远程测试和基础设施构建的开发人员才需要此步骤。

或者，您也可以使用 GameLift Local 在本地测试服务器的 GameLift 集成，这不需要AWS凭证。借助对客户端覆盖的支持，您可以针对 GameLift Local 进行测试。有关更多信息，请参阅 [AWS GameLift Gem 本地测试](/docs/user-guide/gems/reference/aws/aws-gamelift/local-testing/)。
{{< /note >}}


## 5. 设置 AWS CLI 和 AWS CDK

AWS GameLift Gem 提供了一个示例AWS Cloud Development Kit （AWS CDK） 应用程序，您可以使用它来建模 GameLift 资源并将服务器部署到 GameLift。为此，您必须在本地计算机上安装[AWS Command Line Interface (AWS CLI)](https://aws.amazon.com/cli/) 和 [AWS CDK](https://aws.amazon.com/cdk/)。
