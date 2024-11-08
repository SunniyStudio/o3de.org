---
linktitle: 高级主题
title: AWS Metrics Gem 的高级主题
description: 了解 Open 3D Engine （O3DE） 中 AWS Metrics Gem 的其他功能，这些功能超出了基础知识。
toc: true
weight: 500
---

## 控制台命令

AWS Metrics Gem 提供两个“`AWSMetricsSystemComponent`”控制台命令，用于转储统计数据和启用/禁用离线记录。

### DumpStats

用于在控制台中显示指标提交的统计数据。统计数据包括以下内容。

* 发送到本地文件或 AWS 支持的后端的事件总数。
* 成功总数。
* 失败总数。
* 掉落总数。

```console
AWSMetricsSystemComponent.DumpStats
```

### EnableOfflineRecording

用于启用/禁用离线录制。如果启用此功能，指标事件将发送到本地文件。在禁用该功能时，您可以选择通过指定 '`submit`' 参数来重新提交存储在本地文件中的事件。

```console
AWSMetricsSystemComponent.EnableOfflineRecording false submit
```

## 客户端配置

您可以在客户端配置文件中更改一些客户端设置 `Gems\AWSMetrics\Code\Registry\awsMetricsClientConfiguration.setreg`.

可配置的设置包括以下内容。

| 设置 | 说明 |
| --- | --- |
| **OfflineRecording** | 是否将指标发送到本地文件，而不是 AWS 支持的后端。 <br> 默认为 false。<br> 您可以在运行时使用控制台命令`AWSMetricsSystemComponent.EnableOfflineRecording`更改此设置。 |
| **MaxQueueSizeInMb** | 用于批量发送指标事件的本地缓冲区的最大大小 （MB）。 <br> 默认值为 0.3。 <br> 由于 Kinesis Data Stream 的限制，建议小于 5 MB。有关更多信息，请参阅 Amazon Kinesis [PutRecords](https://docs.aws.amazon.com/kinesis/latest/APIReference/API_PutRecords.html) 文档。|
| **QueueFlushPeriodInSeconds** | 刷新指标事件队列的刷新周期（以秒为单位）。 <br> 默认值为 60。 |
| **MaxNumRetries** | 在丢弃指标事件之前发送指标事件的最大重试次数。<br> 默认值为 1。 |

## Metrics 事件架构

AWS Metrics Gem 使用 [事件 JSON 架构](./event-schema/) 来验证从客户端提交或发送到服务 API 的指标事件。任何未通过验证的 metrics 事件都将被丢弃。架构中未定义的任何自定义量度属性都将作为平面 JSON 字典添加到量度事件的“`event_data`”字段中。

如果要自定义架构，请在 AWS CDK 应用程序中的`Gems\AWSMetrics\Code\Include\Public\AWSMetricsConstant.h` 和 `api_spec.json`中更新它。在此更改之后，您将需要重新构建项目并重新部署 AWS CDK 应用程序。

## 迁移到生产就绪型解决方案

示例 AWS CDK 应用程序为使用 AWS 分析服务提供了一个合理的起点，该服务在安全性、成本和可用性方面都经过深思熟虑。经验丰富的开发人员还可以自定义 AWS CDK 应用程序，甚至将其迁移到生产就绪型解决方案，详见 [Game Analytics Pipeline 上的 AWS 指南](https://aws.amazon.com/solutions/implementations/game-analytics-pipeline/)。
