---
linktitle: AWS Client Auth
title: AWS Client Auth Gem
description: Open 3D Engine （O3DE） AWS Client Auth Gem Gem 简介。
toc: true
---

**AWS Client Auth** Gem 允许经过身份验证的用户或匿名用户在运行您的游戏或模拟时访问 AWS 服务。它使用以下任何受支持的身份提供商提供身份验证选项：

* Amazon Cognito user pool
* [Google Identity](https://developers.google.com/identity)
* [Login with Amazon](https://developer.amazon.com/login-with-amazon)

Gem 根据当前经过身份验证的登录状态从 Amazon Cognito 身份池获取 AWS 凭证。如果找到用户的成功登录，则获取经过身份验证的 AWS 凭证。否则，将获取匿名 AWS 凭证。

AWS 凭证与 AWS 原生 SDK 客户端对象共享所有权。每当新用户登录、注销或刷新令牌时，都会刷新或更新凭据。

如果找到多个登录，则“`GetCredentials`”会将这些身份链接在一起。然后，无论您使用哪个身份验证提供商，您都会获得相同的身份。

## 功能

此 Gem 具有以下主要功能：

* Amazon Cognito 用户池中的用户管理
    * 通过电子邮件或电话确认注册
    * 多重身份验证 （MFA）
    * 忘记密码处理

* 通过支持的身份验证提供程序的多种身份验证方法。
    * 使用 Amazon Cognito 用户池中的用户名和密码登录。
    * 使用 Amazon Cognito 用户池中的 OAuth 密码流进行 MFA 登录。
    * 在 Google 和 Login with Amazon 中使用 OAuth 设备流程进行 MFA 登录。
    * Provider 模式，用于为自定义身份验证提供程序添加实现。
    * 按需刷新过期的令牌。

* 身份验证提供程序状态的 AWS 凭证检索。
    * 使用有效令牌登录的用户的经过身份验证的凭据。
    * 无用户登录时的匿名凭据。
    * 使用现有令牌自动刷新凭据。
    * 在刷新已验证的令牌时刷新凭据。
    * 凭据在注销时失效。
    * 使用共享的“`AWSCredentialsProvider`”对象与 AWS 原生开发工具包服务客户端共享凭证。
    * 跨不同身份验证提供程序的多个登录链接身份。

## 启用 AWS Client Auth Gem

要启用 AWS Client Auth Gem，请执行以下操作：

1. 使用 **O3DE Project Manager** 或命令行将 AWS Client Auth Gem 添加到您的项目中。请注意，AWS Client Auth 需要以下 Gem 作为依赖项：

    * [AWS Core](/docs/user-guide/gems/reference/aws/aws-core)
    * **HttpRequestor**

1. 使用 Project Manager、Visual Studio 或 CMake 构建您的项目。

1. 要为您的项目配置 AWS Client Auth，请按照 [设置 AWS Client Auth Gem](./setup/)。
