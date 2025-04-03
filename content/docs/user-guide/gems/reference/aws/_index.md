---
linktitle: AWS Gems
title: AWS, AWS Gems, 和 O3DE
description: 使用 AWS Gems 访问 Open 3D Engine （O3DE） 中的 AWS 云连接服务。
---

Amazon Web Services （AWS） 是一个云平台，提供广泛而强大的服务集合。您可以使用这些云服务上传或下载文件、访问数据库、在云中执行代码以及执行许多其他操作。使用云服务可以省去维护它所依赖的基础设施的麻烦。

有关 **Open 3D Engine （O3DE）** 中的 AWS Gems 的概述，请观看下面的视频。然后，请继续阅读以了解有关在 O3DE 项目中使用 AWS 的更多信息！

{{< bilibili-width id="BV1sU4y137wj" title="AWS Gems in O3DE" >}}

## 基于云的资源

当您使用 AWS 云服务时，您可以通过 *资源* 进行此操作：一个可供您使用、帮助或支持的基于云的实体。例如，资源包括数据库、文件存储位置、服务运行的代码等。

创建资源时，它存在于云中，但您可以使用它并管理其内容。您还可以指定个人或组访问或使用资源的权限。例如，您可以允许公众中的任何人从您的数据库中读取数据，但不能写入或修改数据库。

## 资源组

要创建与 AWS 连接的功能（例如指标报告管道），AWS Gems 使用 [AWS Cloud Development Kit](https://docs.aws.amazon.com/cdk/v2/guide/getting_started.html)（AWS CDK） 对资源进行建模和部署。每个 AWS Gem 都包含一个 AWS CDK 应用程序，用于定义该功能所需的 AWS 资源。由于 AWS CDK 应用程序将资源建模为代码，因此您可以扩展或组合 AWS CDK 结构以创建强大的后端服务。

## AWS 账户

AWS 账户 拥有您的 AWS 资源。您和您的团队可以使用 AWS 账户共享对相同资源的访问权限。例如，您团队的 AWS 账户可能拥有您和团队成员都可以使用的数据库资源。

您或您团队中的某个人是管理员。管理员为您的团队创建 AWS 账户，并向团队成员授予访问该账户资源的权限。

## AWS Gems

AWS Gems 提供示例 AWS CDK 应用程序以及使用它们的所有代码。这些 Gem 支持您的 AWS 资源的建模、部署和通信。

以下 AWS Gem 可用：
2
| Gem               | 详情 |
|-------------------|---------|
| [AWS Core](aws-core/) | 为所有 AWS 功能 Gem 提供通用机制和基本配置，包括凭证管理、资源共享和处理对 AWS 服务的调用。此 Gem 还包括适用于 C++ 的 AWS 开发工具包的平台扩展。 |
| [AWS Client Auth](aws-client-auth/) | 提供 OAuth 登录身份验证流程，并使用 OpenID 令牌获取临时 AWS 凭证。|
| [AWS Metrics](aws-metrics/) | 报告和聚合指标数据，以便在 AWS 中进行实时或批量分析。 |
| [AWS GameLift](aws-gamelift/) | 提供一个框架来扩展 O3DE 联网层和Multiplayer Gem 以与 Amazon GameLift 配合使用。|
