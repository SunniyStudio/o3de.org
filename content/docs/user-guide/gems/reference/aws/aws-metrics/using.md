---
title: 使用已部署的资源
description: 了解如何使用您为 Open 3D Engine （O3DE） 项目部署的 AWS Metrics Gem 资源。
toc: true
weight: 200
---

部署 AWS Metrics 资源后，您可以使用实时分析和批处理分析等功能。

## 使用实时分析

部署 AWS Cloud Development Kit （AWS CDK） 应用程序后，将自动创建一个 **Amazon CloudWatch** 控制面板，以显示 AWS CDK 应用程序中定义的运行状况指标和示例指标 （TotalLogin）。

要启动实时分析，请转到 Amazon Kinesis 控制台或使用 AWS CLI  [`start-application`](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/kinesisanalytics/start-application.html)  命令启动 Kinesis Data Analytics 应用程序。

{{< important >}}
请注意，您需要按小时付费才能运行该应用程序。
{{< /important >}}

当应用程序运行时，CloudWatch 控制面板将开始显示您从客户端发送的指标。

![Sample metrics dashboard](/images/user-guide/gems/reference/aws/aws-metrics/sample-metrics-dashboard.png)

为实时分析部署的大多数组件都是可扩展的。您可以从 AWS CDK 应用程序代码或 AWS 控制台自定义 Kinesis Data Analytics 应用程序、分析处理 Lambda 和 CloudWatch 控制面板，以转换和分析您的流数据。

请注意，重新部署 AWS CDK 应用程序将覆盖您通过 AWS 控制台所做的任何更改。因此，建议您通过 AWS CDK 应用程序代码进行修改并重新部署 AWS CDK 应用程序。

## 启用批处理分析

要启用可选的批处理分析功能，请在合成和部署 AWS CDK 应用程序时指定`batch_processing=true` 。

```cmd
cdk synth -c batch_processing=true
cdk deploy -c batch_processing=true
```

指标数据将发送到 **Amazon S3 数据湖**并以 parquet 格式保存。要使数据可被发现，请转到 **AWS Glue** 控制台或使用 AWS CLI [`start-crawler`](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/glue/start-crawler.html)命令启动由 AWS CDK 应用程序部署的 Glue 爬网程序。爬网程序完成其工作后，指标事件表将更新。然后，您可以通过 Amazon Athena 引擎对数据运行查询。

在您的 AWS CDK 应用程序中，在项目的特定工作组下创建了一些命名查询。您可以将这些示例查询作为示例运行，也可以创建自定义查询以进行批处理分析。

您还可以创建 **Amazon QuickSight** 控制面板（不包含在 AWS CDK 应用程序中）来可视化统计数据。有关说明，请参阅 Game Analytics Pipeline 文档中的 [构建 Amazon QuickSight 控制面板](https://docs.aws.amazon.com/solutions/latest/game-analytics-pipeline/deployment.html#step5)。

与实时分析类似，您还可以自定义用于批处理的组件。例如，您可以在事件处理 Lambda 中定义自定义转换，或更改 Glue 爬网程序的行为以定期运行它。

## AWS CloudFormation 堆栈输出

AWS CDK 应用程序部署的 AWS CloudFormation 堆栈包含以下输出。您可以使用这些输出作为参考，并基于它们创建资源映射文件。

| 输出 | 说明 |
| --- | --- |
| **AdminPolicyOutput** | 用于调用服务的管理员策略 ARN。 |
| **AnalyticsApplicationName** | Kinesis Data Analytics 应用程序来处理实时指标数据。 |
| **DashboardName** | CloudWatch 控制面板，用于监控运行状况和实时指标。 |
| **DeploymentStage** | REST API 部署的阶段。 |
| **EventsCrawlerName** | Glue 爬网程序，以使用指标事件表填充 AWS Glue 数据目录。 |
| **RestApiId** | 分析管道的服务 API ID。 |
| **UserPolicyOutput** | 用于调用 service 的用户策略 ARN。 |
