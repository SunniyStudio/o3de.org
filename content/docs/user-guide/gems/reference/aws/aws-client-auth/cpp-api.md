---
linktitle: 使用C++ API
title: 将 C++ API 与 AWS Client Auth Gem 结合使用
description: 简要了解如何在 Open 3D Engine （O3DE） 中将 C++ API 与 AWS Client Auth Gem 结合使用。
toc: true
weight: 400
---

AWS Client Auth Gem 提供了一个 C++ API，用于发出客户端授权和用户管理请求，以及使用 O3DE 事件总线处理通知。有关使用事件总线系统的更多信息，请参阅 [EBus](/docs/user-guide/programming/messaging/ebus)文档。

## C++ API 参考

O3DE API 参考指南包含对 [AWS Client Auth](/docs/api/gems/awsclientauth)  C++ API 的完整引用。

具体而言，对于 Amazon Cognito 授权请求，请参阅处理程序函数的[IAWSCognitoAuthorizationRequests](https://o3de.org/docs/api/gems/awsclientauth/class_a_w_s_client_auth_1_1_i_a_w_s_cognito_authorization_requests.html) 接口和 [AWSCognitoAuthorizationNotifications](https://o3de.org/docs/api/gems/awsclientauth/class_a_w_s_client_auth_1_1_a_w_s_cognito_authorization_notifications.html)  通知总线的文档。

对于 Amazon Cognito 用户管理请求，请参阅[IAWSCognitoUserManagementRequests](https://o3de.org/docs/api/gems/awsclientauth/class_a_w_s_client_auth_1_1_i_a_w_s_cognito_user_management_requests.html)接口的文档以及处理程序函数的[AWSCognitoUserManagementNotifications](https://o3de.org/docs/api/gems/awsclientauth/class_a_w_s_client_auth_1_1_a_w_s_cognito_user_management_notifications.html)通知总线。

有关使用受支持的身份验证提供程序之一的身份验证请求，请参阅 [AuthenticationProviderInterface][AuthenticationProviderInterface](https://o3de.org/docs/api/gems/awsclientauth/class_a_w_s_client_auth_1_1_i_authentication_provider_requests.html)接口的文档以及处理程序函数的 [AuthenticationProviderNotifications](https://o3de.org/docs/api/gems/awsclientauth/class_a_w_s_client_auth_1_1_authentication_provider_notifications.html) 通知总线.

有关特定身份验证提供程序实施的参考文档，请参阅[AWSCognitoAuthenticationProvider](https://o3de.org/docs/api/gems/awsclientauth/class_a_w_s_client_auth_1_1_a_w_s_cognito_authentication_provider.html), [GoogleAuthenticationProvider](https://o3de.org/docs/api/gems/awsclientauth/class_a_w_s_client_auth_1_1_google_authentication_provider.html), 和 [LWAAuthenticationProvider](https://o3de.org/docs/api/gems/awsclientauth/class_a_w_s_client_auth_1_1_l_w_a_authentication_provider.html)。
