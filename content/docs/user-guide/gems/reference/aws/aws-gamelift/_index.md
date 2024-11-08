---
linkTitle: AWS GameLift
title: AWS GameLift Gem
description: AWS GameLift Gem 提供了一个框架，用于扩展 O3DE 联网层和多人游戏 Gem 以与 Amazon GameLift 配合使用。
toc: true
---

## 功能概述
**AWS GameLift** Gem 提供以下功能：

**Amazon GameLift 集成**
- 一个框架，用于扩展 O3DE 联网层，并允许 **Multiplayer** Gem 与 Amazon 的专用服务器解决方案 [Amazon GameLift](https://docs.aws.amazon.com/gamelift/latest/developerguide/gamelift-intro.html) 配合使用。AWS GameLift Gem 提供与 [GameLift 服务器开发工具包](https://docs.aws.amazon.com/gamelift/latest/developerguide/gamelift-supported.html#gamelift-supported-servers)和 AWS 开发工具包客户端（以调用 GameLift 服务本身）的集成。

**构建和打包管理**
- 打包和（可选）上传专用服务器内部版本的说明。
- 示例 [AWS Cloud Development Kit （AWS CDK）](https://docs.aws.amazon.com/cdk/v2/guide/home.html) 应用程序。您可以部署AWS CDK应用程序来设置基本的 GameLift 资源，或者通过添加或更新已部署的资源来修改应用程序以满足您的需求。
  
## 版本亮点

- 支持 CreateSession（在队组/队列上）、SearchSessions、JoinSession和 LeaveSession 通过 GameLift。
- 支持 FlexMatch，包括通过 GameLift 进行回填。
- 支持 Windows/Linux 专用服务器和多平台客户端启动器。
- 支持 AWS CDK 应用程序来管理 GameLift 资源。


## 相关信息

为了更好地理解本 AWS GameLift Gem 指南中涵盖的主题，我们建议您查看以下内容：
- [What Is Amazon GameLift? (Amazon GameLift Developer Guide)](https://docs.aws.amazon.com/gamelift/latest/developerguide/gamelift-intro.html)
- [FlexMatch](https://docs.aws.amazon.com/gamelift/latest/flexmatchguide/match-intro.html) - 了解匹配和 Amazon GameLift 的工作原理。
- [Multiplayer](/docs/user-guide/networking/multiplayer/) - 了解多人游戏在 O3DE 中的工作原理。
- [Networking](/docs/user-guide/networking/)


## 主题

| 主题 | 说明 |
| - | - |
| [AWS GameLift Gem 安装](gem-setup/) | 在 O3DE 中设置 AWS GameLift Gem。 |
| 使用 AWS GameLift Gem 准备游戏 |  |
| <ul><li> [Game Session 管理](session-management/)</li></ul>| 了解如何为游戏进行会话管理准备。 |
| <ul><li> [FlexMatch 支持](flexmatch/) </li></ul>| 了解如何准备游戏以进行匹配和回填现有匹配。 |
| <ul><li> [AWS GameLift Gem 高级主题](advanced-topics/) </li></ul>| 了解使用 AWS GameLift Gem 准备游戏的一些高级方法。|
| [AWS GameLift Gem本地测试](local-testing/) | 验证AWS GameLift Gem 功能集成 使用 GameLift Local，这是一个命令行工具，用于启动托管 GameLift 服务的独立版本。 |
| [适用于 Windows 的 AWS GameLift Gem 生成包打包](build-packaging-for-windows/) | 了解如何打包 Windows 专用服务器版本，以便您可以在 GameLift 上安装和运行它们。 |
| [适用于 Linux 的 AWS GameLift Gem 生成包打包](build-packaging-for-linux/) | 了解如何打包 Linux 专用服务器版本，以便您可以在 GameLift 上安装和运行它们。|
| [AWS GameLift Gem 资源管理](resource-management/) | 了解可用于建模和部署 GameLift 资源的示例 AWS CDK 应用程序。 |
| AWS GameLift Gem Multiplayer Sample | Coming soon! |
