---
linktitle: Metrics API
title: 在 O3DE 中收集性能指标
description: 介绍如何使用 Open 3D Engine (O3DE) 中的性能收集 API 将性能指标记录为 JSON 格式
weight: 450
---
## 性能指标收集说明
**Open 3D Engine (O3DE)** 中的性能指标收集 API 提供了一个标准化 API，用于收集与查询功能系统性能指标相关的指标。
它提供了一个 C++ 接口，用于通过事件记录器 API 记录基于 Google [跟踪事件格式](https://docs.google.com/document/d/1CvAClvFfyA5R-PhYUmn5OOQtYMH4h6I0nSsKchNAySU/preview#) 的指标。
通过[统计管理器](https://github.com/o3de/o3de/blob/development/Code/Framework/AzCore/AzCore/Statistics/StatisticsManager.h)提供了一个统计收集 API，它可以记录一段时间内帧的统计数据，并计算更高级别的统计结果，包括最小值、最大值、标准偏差和方差。
这两个 API 与性能收集器系统相连，后者封装了一个统计管理器和一个 JSON 跟踪事件记录器，用于记录统计指标数据，并将收集到的统计数据记录到 Google 跟踪事件格式中。

要了解如何使用事件记录器 API 将 JSON 跟踪事件日志设置为 Google [跟踪事件格式](https://docs.google.com/document/d/1CvAClvFfyA5R-PhYUmn5OOQtYMH4h6I0nSsKchNAySU/preview#)  格式，并在基于 Chromium 的浏览器中预览日志，请阅读 [使用 Json 跟踪事件记录器进行记录](./trace-event-logger.md) 。
