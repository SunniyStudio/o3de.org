---
linktitle: 设置 Client Auth
title: 设置 AWS Client Auth Gem
description: 了解如何为您的 Open 3D Engine （O3DE） 项目设置 AWS Client Auth Gem。
toc: true
weight: 100
---

## 先决条件

AWS Client Auth Gem 需要安装和配置以下内容：

* AWS Cloud Development Kit (CDK)
* AWS 凭据
* O3DE AWS Core Gem

[AWS命令行接口](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-welcome.html) (CLI) (version 2) 是可选的，但也强烈建议使用。

有关安装和配置这些先决条件的帮助，请参阅 [AWS Gem 入门](/docs/user-guide/gems/reference/aws/aws-core/getting-started/)。

## 设置 AWS Client Auth

完成以下设置步骤以在您的项目中使用 AWS 客户端身份验证 Gem：

* 在您的项目中启用 AWS Client Auth Gem。
* 配置项目设置。
* 部署 CDK 应用程序。
* 更新资源映射文件以使用已部署的 AWS 资源。

### 1. 启用 the AWS Client Auth Gem

如果您尚未将 **AWS Client Auth Gem** 添加并构建到您的项目中，请按照说明[将 Gem 添加到项目](/docs/user-guide/project-config/add-remove-gems/)。

### 2. 配置身份验证提供程序

对于此步骤，请确定您将使用哪个身份验证提供程序，并按照 [使用身份验证提供程序](./authentication-providers/)中的步骤将项目配置为使用该提供程序。您可以使用其中一个受支持的提供程序，也可以扩展 Gem 以支持您自己的提供程序。

### 3. 部署 CDK 应用程序

使用 CDK 合成器和 deploy 命令从 AWS 客户端身份验证 Gem 目录部署AWS资源以进行身份验证和授权支持。有关这些部署操作的帮助，请参阅 AWS Core 文档中 [部署 CDK 应用程序](/docs/user-guide/gems/reference/aws/aws-core/cdk-application/)中的合成和部署步骤。

在设置用于部署的常量时，AWS Client Auth Gem 支持以下其他可选值。如果设置，CDK 应用程序将尝试使用 Amazon Cognito 身份池启用经过身份验证的提供商授权。

**`GOOGLE_APP_CLIENT_ID`**: **Google** 应用程序客户端 ID，用于在 Amazon Cognito 身份池中启用 Google 身份验证的授权。

**`LOGIN_WITH_AMAZON_APP_CLIENT_ID`**: **Login with Amazon** (LWA) 应用程序客户端 ID，用于在 Amazon Cognito 身份池中启用 LWA 身份验证的授权。

您可以使用 [部署脚本](https://docs.aws.amazon.com/cdk/v2/guide/environments.html)来设置这些环境变量。

成功部署后，将预置以下资源。

#### AWS Client Auth堆栈

对为 AWS Client Auth Gem 预置的所有资源进行分组。删除堆栈将从 AWS Client Auth 堆栈中删除所有资源。

#### IAM 角色

1. **CognitoUserPoolSMSRole**: 允许 Amazon Cognito 用户池发送 SMS 消息以进行注册和 MFA。

1. **Authenticated CognitoIdentityPoolRole**: 通过提供与附加到此角色的托管策略关联的凭证，向经过身份验证的客户端提供权限。

1. **Anonymous CognitoIdentityPoolRole**: 通过提供与附加到此角色的托管策略关联的凭证，向匿名客户端提供权限。

#### Amazon Cognito用户池

在启用电子邮件/电话注册的情况下创建用户池，SMS_MFA启用和可选。

#### Amazon Cognito身份池

创建身份池以支持经过身份验证的身份和匿名身份。

##### 身份验证

1. **Amazon Cognito user pool**: 使用创建的用户池进行身份验证。
2. **Google**: 使用 Google 身份验证提供程序进行身份验证。
3. **Login With Amazon**: 使用 LWA 身份验证提供程序进行身份验证。

### 4. 更新资源映射文件

部署 CDK 应用程序后，必须更新项目的资源映射文件以导出已部署的 REST API 信息，以便您可以使用已部署的 AWS 资源。

使用 [资源映射工具](/docs/user-guide/gems/reference/aws/aws-core/resource-mapping-tool/)将以下映射添加到项目的资源映射文件中：

* `AWSClientAuth.CognitoUserPoolId`
* `AWSClientAuth.CognitoUserPoolAppClientId`
* `AWSClientAuth.CognitoIdentityPoolId`
