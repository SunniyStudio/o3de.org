---
linktitle: AWS Metrics
title: AWS Metrics Gem
description: Open 3D Engine （O3DE） AWS 指标 Gem 简介。
toc: true
---

**AWS Metrics** Gem 为 O3DE 开发人员提供可扩展、开箱即用的检测和分析解决方案。它使用 AWS Core Gem 构建分析管道，并将您连接到支持对分析数据执行常见指标操作的 AWS 服务，包括：

*摄入
*存储
*分析
*监测
*报告

使用此 Gem，您可以以线程安全的方式从 C++、Lua 和 Script Canvas 生成和提交指标事件。要使用 AWS 服务进行实时和批量分析，您可以部署示例 AWS Cloud Development Kit （AWS CDK） 应用程序作为合理的起点。您还可以将提供的 AWS CDK 应用程序扩展为完整的生产就绪型解决方案，以满足您的生产和扩展需求。请参阅 AWS 网站上的 [Game Analytics Pipeline](https://aws.amazon.com/solutions/implementations/game-analytics-pipeline/)，详细了解架构。

## 启用 AWS Metrics Gem

要启用 AWS Metrics Gem，请执行以下操作：

1. 使用 **O3DE Project Manager** 或命令行将 AWS Metrics Gem 添加到您的项目中。请注意，AWS Metrics 需要 [AWS 核心 Gem](/docs/user-guide/gems/reference/aws/aws-core) 作为依赖项。

1. 使用 Project Manager、Visual Studio 或 CMake 构建您的项目。

1. 要为您的项目配置 AWS 指标，请按照 [设置AWS Metrics Gem](./setup/)。
