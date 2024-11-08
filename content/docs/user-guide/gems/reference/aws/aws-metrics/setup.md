---
linktitle: 设置Metrics
title: 设置 AWS Metrics Gem
description: 了解如何为您的 Open 3D Engine （O3DE） 项目设置 AWS Metrics Gem。
toc: true
weight: 100
---

## 先决条件

AWS Metrics Gem 需要安装和配置以下内容：

* AWS 云开发工具包 （AWS CDK）
* AWS 凭证
* O3DE AWS 核心 Gem

 [AWS Command Line Interface](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-welcome.html) (CLI) （版本 2） 是可选的，但也强烈建议使用。

查看 [Getting Started with AWS Gems](/docs/user-guide/gems/reference/aws/aws-core/getting-started/) 获取有关安装和配置这些先决条件的帮助。

## 设置 AWS 指标

完成以下设置步骤以在您的项目中使用 AWS Metrics Gem：

* 在您的项目中启用 AWS Metrics Gem。
* 部署 AWS CDK 应用程序。
* 更新资源映射文件以使用已部署的 AWS 资源。

### 1.启用 AWS Metrics Gem

如果您尚未在项目中添加和构建 **AWS Metrics Gem**，请按照步骤将 [添加 Gem](/docs/user-guide/project-config/add-remove-gems/)添加到当前项目。

### 2. 部署 AWS CDK 应用程序

使用 AWS Metrics Gem 目录中的 AWS CDK“`synth`”和“`deploy`”命令部署示例 CDK 应用程序并构建 AWS 支持的分析管道，如下图所示。有关这些部署操作的帮助，请参阅 AWS Core 文档中 [部署 CDK 应用程序](/docs/user-guide/gems/reference/aws/aws-core/cdk-application/) 中的合成和部署步骤。

![Analytics pipeline provided by the sample AWS CDK application](/images/user-guide/gems/reference/aws/aws-metrics/sample-analytics-pipeline.png)

该管道侧重于两个使用案例：用于操作的热/近乎实时，以及用于 BI 使用案例（例如 DAU、MAU）的冷/批处理。示例分析管道使用 Amazon API Gateway（服务终端节点）进行用户访问和管理界面，使用 Amazon Kinesis Data Streams 和 Kinesis Data Firehose 进行流式摄取，使用 Kinesis Data Analytics 和 Amazon CloudWatch 进行实时分析，使用 Amazon Simple Storage Service （S3） 进行数据湖集成，并使用 AWS Glue 和 Amazon Athena 进行批量分析。

### 3. 更新资源映射文件

部署 CDK 应用程序后，必须更新项目的资源映射文件以导出已部署的 REST API 信息，以便您可以使用已部署的 AWS 资源。

使用 [资源映射工具](/docs/user-guide/gems/reference/aws/aws-core/resource-mapping-tool/)将`RESTApiStage` 和 `RESTApiId`映射添加到项目的资源映射文件中。新部分将类似于以下示例。

**project_aws_resource_mappings.json**

```json
{
    "AWSResourceMappings": {
        "AWSMetrics.RESTApiStage": {
            "Type": "AWS::ApiGateway::Stage",
            "Name/ID": "rest_api_stage_name"
        },
        "AWSMetrics.RESTApiId": {
            "Type": "AWS::ApiGateway::RestApi",
            "Name/ID": "rest_api_id_value"
        }
        ....
    },
    "AccountId": "12345677789",
    "Region": "us-west-2",
    "Version": "1.0.0"
}
```
