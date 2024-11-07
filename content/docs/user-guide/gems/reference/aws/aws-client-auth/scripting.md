---
linktitle: Client Auth脚本编程
title: AWS Client Auth 脚本编程
description: 在 Open 3D Engine （O3DE） 中将 AWS Client Auth Gem 与 Script Canvas 和 Lua 结合使用的示例。
weight: 300
toc: true
---

本页演示了 AWS Client Auth Gem 定义的 Script Canvas 节点和 Lua 脚本的示例用法。

## Script Canvas

### 初始化

此脚本从 AWS Client Auth Gem 初始化以下事件总线 （EBus）：

* **AuthenticationProviderRequestBus**
* **AWSCognitoUserManagementRequestBus**
* **AWSCognitoAuthorizationRequestBus**

请注意，在初始化 AuthenticationProviderRequestBus 时使用了 '`Providers`' 变量。此变量是身份验证提供程序字符串的数组。

AuthenticationProviderRequestBus 总线的 **Initialize** 函数的输入数据引脚：

* 提供程序 (string array)
* `authenticationProvider.setreg`的文件路径 (string)

所有初始化函数的输出数据引脚：

* Result (boolean)

![Scripting AWS Client Auth Initialize node](/images/user-guide/gems/reference/aws/aws-client-auth/scripting-initialize.png)

### Amazon Cognito用户池密码登录流程

下图显示了 Amazon Cognito 用户池中的密码登录流程。

记下为提供程序、令牌和凭据创建的变量。

![Scripting password sign in with the Amazon Cognito user pool](/images/user-guide/gems/reference/aws/aws-client-auth/scripting-password-sign-in.png)

### Login with Amazon 设备登录流程

![Scripting LWA device sign in](/images/user-guide/gems/reference/aws/aws-client-auth/scripting-lwa-device-sign-in.png)

{{< todo issue="https://github.com/o3de/o3de.org/issues/1550" >}}
将这些图像分成更小的部分并描述每个部分。
{{< /todo >}}

## Lua

### Auth 通知处理程序

示例脚本：

```lua
local auth = {
    Properties = {
        EventNames = {"level_started", "login", "logout", "level_completed"}
    }
}
 
function auth:OnActivate()
    self.authenticationNotificationBus = AuthenticationProviderNotificationBus.Connect(self)
    self.awsCognitoAuthorizationNotificationBus = AWSCognitoAuthorizationNotificationBus.Connect(self)
    LyShineLua.ShowMouseCursor(true)
end
 
function auth:OnPasswordGrantSingleFactorSignInSuccess(tokens)
    Debug.Log("Lua:login Success. Got tokens")
end
 
function auth:OnPasswordGrantSingleFactorSignInFail(errorMessage)
    Debug.Log("Lua:login fail: "..errorMessage)
end
 
function auth:OnRequestAWSCredentialsSuccess(creds)
    Debug.Log("Lua:Creds success")
end
 
function auth:OnRequestAWSCredentialsFail(errorMessage)
    Debug.Log("Lua:Get Creds fail: "..errorMessage)
end
 
function auth:OnDeactivate()   
    self.authenticationNotificationBus:Disconnect()
    self.awsCognitoAuthorizationNotificationBus:Disconnect()
end
 
return auth
```
