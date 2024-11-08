---
linktitle: 脚本编程
title: AWS Core 脚本编程
description: 在 Open 3D Engine （O3DE） 中将 AWS Core Gem 与 Script Canvas 结合使用的示例。
weight: 400
toc: true
---

本主题演示了 AWS Core Gem 定义的 Script Canvas 节点。

## Amazon S3

### GetObject

输入数据引脚：

* Bucket Resource KeyName
* Object KeyName
* Outfile Name

如果函数成功，您将从 **AWSS3BehaviorNotificationBus** 总线上的 **OnGetObjectSuccess** 事件处理程序收到一条成功消息。文件将下载到指定为 Outfile Name 的位置。

如果该函数导致错误，您将从 **AWSS3BehaviorNotificationBus** 总线上的 **OnGetObjectError** 事件处理程序收到一条错误消息。

![Scripting AWS S3 GetObject node](/images/user-guide/gems/reference/aws/aws-core/scripting-s3-get-object.png)

### HeadObject

输入数据引脚：

* Bucket Resource KeyName
* Object KeyName

如果函数成功，您将从 **AWSS3BehaviorNotificationBus** 总线上的 **OnHeadObjectSuccess** 事件处理程序收到一条成功消息。

如果该函数导致错误，您将从 **AWSS3BehaviorNotificationBus** 总线上的 **OnHeadObjectError** 事件处理程序收到一条错误消息。

![Scripting AWS S3 HeadObject node](/images/user-guide/gems/reference/aws/aws-core/scripting-s3-head-object.png)

## Amazon DynamoDB

### GetItem

输入数据引脚：

* Table Resource KeyName
* Key Map

键映射变量格式：

![GetItem - Key variable properties](/images/user-guide/gems/reference/aws/aws-core/scripting-dynamodb-get-item-key-variable.png)

请参阅Amazon DynamoDB API 中的 [AttributeValue](https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_AttributeValue.html) 。

如果函数成功，您将在 **AWSDynamoDBBehaviorNotificationBus** 总线上的 **OnGetItemSuccess** 事件处理程序的 '`AttributeValue`' 字符串映射中获得结果。

如果该函数导致错误，您将从 **AWSDynamoDBBehaviorNotificationBus** 总线上的 **OnGetItemError** 事件处理程序收到一条错误消息。

![Scripting AWS DynamoDB GetItem node](/images/user-guide/gems/reference/aws/aws-core/scripting-dynamodb-get-item.png)

## AWS Lambda

### Invoke

输入数据引脚：

* Function Resource KeyName
* Payload

如果函数成功，您将从 **AWSLambdaBehaviorNotificationBus** 总线上的 **OnInvokeSuccess** 事件处理程序获取返回值。

如果该函数导致错误，您将从 **AWSLambdaBehaviorNotificationBus** 总线上的 **OnInvokeError** 事件处理程序收到一条错误消息。

![Scripting AWS Lambda Invoke node](/images/user-guide/gems/reference/aws/aws-core/scripting-lambda-invoke.png)
