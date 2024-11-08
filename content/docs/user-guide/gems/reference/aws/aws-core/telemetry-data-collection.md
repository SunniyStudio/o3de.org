---
linktitle: Metric Data Collection
title: AWS Core Metric Data Collection
description: Open 3D Engine 中的 AWS Core Gem 收集的指标数据。
weight: 500
---

AWS Core Gem 包括一个选项，用于定期向 AWS 发送使用情况指标。我们使用这些数据来更好地了解客户如何使用通过 Gem 提供的 AWS 解决方案。启用后，我们会收集以下信息并将其发送到 AWS：

## 收集的指标
2
| 字段            | 说明                | 示例                                  |
|------------------|----------------------------|------------------------------------------|
| `version`        | 定义生成此事件的指标架构的版本。 | `Schema Policy 1.1` |
| `o3de_version`   | 定义提供度量的 O3DE 版本（如果已知）。 | `2107.1` |
| `platform`       | 标识运行 O3DE 编辑器的平台。 | `Mac` |
| `platform_version` | 标识版本或子平台类型。 | `10.15.7` |
| `timestamp` | 用于指标生成的时间戳。 | `2007-04-05T14:30` |
| `active_aws_gems` | 项目中活动的 AWS Gem 列表，以 '/' 分隔。 | `AWSCore/AWSMetrics/AWSClientAuth` |

收集的数据受 [AWS 隐私政策](https://aws.amazon.com/privacy/) 的约束。

## Opting Out

您可以通过 Editor 首选项选择退出，**Edit** -> **Editor Settings** -> **Global Preferences** -> **AWS**。

或者，你可以修改编辑器注册表文件`{YOUR-PROJECT}/user/Registry/editorpreferences.setreg`，并重启编辑器。

注册表设置：

```json
{
  "Amazon": {
    "Telemetry": {
      "SendAWSUsageMetric": true
    }
  }
}
```
