---
title: 使用身份验证提供程序
description: 了解如何配置第三方和自定义身份验证提供商，以便与 Open 3D Engine （O3DE） 中的 AWS 客户端身份验证 Gem 一起使用。
weight: 200
toc: true
---

AWS Client Auth Gem 支持多个预配置的第三方身份验证提供商。您还可以添加对自定义提供程序的支持。

要配置身份验证提供程序，您必须执行以下操作。

1. 启用提供商的帐户进行身份验证。
1. 创建并配置身份验证提供程序设置文件。

要创建和使用自定义提供程序，请参阅本主题末尾的 [使用自定义提供程序](#using-a-custom-provider) 中的说明。

## 使用预配置的第三方提供商

1. 为身份验证创建并启用 [Google](https://docs.aws.amazon.com/cognito/latest/developerguide/google.html) 或 [Login with Amazon](https://docs.aws.amazon.com/cognito/latest/developerguide/amazon.html) 账号。

1. 记下用于设备类型流的 **app ID** 和 **app secret**。

## 配置身份验证提供程序设置

您必须创建名为 `AuthenticationProvider.setreg` 的注册表设置文件，以配置身份验证提供程序的设置。此文件必须位于项目的注册表目录中：`<ProjectName>\Registry`。其格式如以下示例所示。

使用您在启用账户时获取的应用程序 ID 作为`AppClientId` 的值。

如果您使用的是 Google，则还必须使用为帐户提供的应用程序密钥作为 `ClientSecret` 的值。

在部署可选的 AWS Cloud Development Kit （AWS CDK） 应用程序时，请务必使用与您选择的提供商对应的 AWS CDK 常量。有关详细信息，请参阅 [设置客户端身份验证](./setup/) 中的 AWS CDK 应用程序部署步骤。

在激活`AWSClientAuthSystemComponent`期间，将读取一次这些设置和资源映射文件中的设置。
2
| 设置 | 说明 |
| --- | --- |
| **AppClientId** | 身份验证提供商在创建账户时提供的客户端 ID。 |
| **ClientSecret** | 身份验证提供商在创建帐户时提供的客户端密钥。仅对 **Google** 是必需的。 |
| **GrantType** | 请求的授权类型。看[https://oauth.net/2/grant-types/](https://oauth.net/2/grant-types/). |
| **ResponseType** | 仅对 **Login With Amazon** 是必需的。与 grant type 相同。 |
| **OAuthCodeURL** | 用于身份验证的请求代码的 URL。 |
| **OAuthTokensURL** | 用于在成功时确认和获取经过身份验证的令牌的 URL。 |

示例 `AuthenticationProvider.setreg` 文件。创建此文件时，请包含与所选提供程序对应的相应部分。

```json
{
    "AWS":
    {
        "LoginWithAmazon":
        {
            "AppClientId": "",
            "GrantType":  "device_code",
            "ResponseType":  "device_code",
            "OAuthCodeURL": "https://api.amazon.com/auth/o2/create/codepair",
            "OAuthTokensURL": "https://api.amazon.com/auth/o2/token"
        },
        "Google":
        {
            "AppClientId": "",
            "ClientSecret": "",
            "GrantType": "urn:ietf:params:oauth:grant-type:device_code",
            "OAuthCodeURL": "https://oauth2.googleapis.com/device/code",
            "OAuthTokensURL": "https://oauth2.googleapis.com/token"
        }
    }
}
```

## 使用自定义提供程序

要将自定义身份验证提供商与 AWS 客户端身份验证 Gem 一起使用，您必须具有基于 OAuth 2.0/OIDC 协议的终端节点。使用以下步骤启用您的提供商。

1. 更新 **Amazon Cognito identity pool** 以支持自定义登录提供程序。

    a. 在 AWS CDK 应用程序中，在 `constants.py` 文件中，为您的身份验证服务的 App Client ID 添加一个条目。

    b. 在`cognito_identity_pool.py`中添加相同的App Client ID 到 `supported_login_providers`。

    c. 合成并部署 AWS Client Auth 堆栈。有关这些命令的帮助，请参阅 AWSCore Gem 文档中的 [部署 CDK 应用程序](/docs/user-guide/gems/reference/aws/aws-core/cdk-application/)。
   
1. 实现 C++ 自定义提供程序。

    a. 在`AuthenticationTokens.h`中向`ProviderNameEnum`添加新的枚举值。

    b. 从`AuthenticationProviderInterface`继承，实现新的自定义提供程序。

    c. 在`AuthenticationProviderManager::CreateAuthenticationProviderObject`方法中，为上述添加支持。

    d. 如果上述 Amazon Cognito 设置正确，则授权将有效。

有关更多信息，请参阅 [AWS Client Auth](/docs/api/gems/awsclientauth) API 参考。
