---
linktitle: 使用C++ API
title: 将 C++ API 与 AWS Metrics Gem 结合使用
description: 大致了解如何在 Open 3D Engine （O3DE） 中将 C++ API 与 AWS Metrics Gem 结合使用。
toc: true
weight: 400
---

AWS Metrics Gem 提供了一个 C++ API，用于使用 O3DE 事件总线提交指标和处理通知。有关使用事件总线系统的更多信息，请参阅 [EBus](/docs/user-guide/programming/messaging/ebus)文档。

## 使用 AWSMetricsRequestBus

使用 C++ 调用 API 的示例代码。

```cpp
// Submit a metrics event and buffer it for sending in batch.
bool result = false;
AWSMetricsRequestBus::BroadcastResult(result, &AWSMetricsRequests::SubmitMetrics, metricsAttributes, 0, "C++", true);
 
// Or, submit a metrics event directly without using a buffer.
bool result = false;
AWSMetricsRequestBus::BroadcastResult(result, &AWSMetricsRequests::SubmitMetrics, metricsAttributes, 0, "C++", false);
 
// Flush all the buffered metrics events.
AWSMetricsRequestBus::Broadcast(&AWSMetricsRequests::FlushMetrics);
```

## 使用 AWSMetricsNotificationBus

使用标准 EBus 通知处理程序在 C++ 中捕获 OnSendMetricsSuccess 和 OnSendMetricsFailure 通知。

## C++ API 参考

请参阅 O3DE API 参考指南 以获取 [AWS Metrics C++ API](/docs/api/gems/awsmetrics).
