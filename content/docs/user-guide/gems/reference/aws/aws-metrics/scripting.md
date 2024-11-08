---
linktitle: Metrics脚本编程
title: AWS Metrics 脚本编程
description: 将 Script Canvas 或 Lua 与 AWS Metrics Gem 和 Open 3D Engine （O3DE） 结合使用以生成和提交指标的示例。
weight: 300
toc: true
---

设置 AWS Metrics Gem 后，客户端可以使用 Script Canvas、Lua 或 C++ 生成和提交指标事件。以下示例演示了如何使用 Script Canvas 和 Lua 编写行为脚本。有关 C++ 示例，请参阅 [将 C++ API 与 AWS 指标 Gem 结合使用](./cpp-api/) 。

## Script Canvas

### 示例：提交指标事件

下图显示了如何使用 AWS Metrics 节点提交指标事件并侦听成功或失败通知。

![Script Canvas graph example of submitting a metrics event](/images/user-guide/gems/reference/aws/aws-metrics/scripting-submitting-metrics-event.png)

{{< todo issue="https://github.com/o3de/o3de.org/issues/1551" >}}
将此图像分成更小的部分并描述每个部分。
{{< /todo >}}

## Lua

以下脚本提供了使用 Lua 调用 AWS Metrics API 的简单示例。

```lua
-- Create a new metrics attribute.
attribute = AWSMetrics_MetricsAttribute()

-- Set the name of the metrics attribute.
attribute:SetName("attribute_name")

-- Set the value of the metrics attribute.
-- You can set string, integer, or double as the attribute value.
-- Call SetStrValue, SetIntValue, or SetDoubleValue for the corresponding value type.
attribute:SetStrValue("attribute_value")

-- Add the attribute to the metrics attribute list.
attributeList = AWSMetrics_AttributesSubmissionList()
attributeList.attributes:push_back(attribute)
 
-- Generate a new metrics event using the metrics attribute list and submit it.
AWSMetricsRequestBus.Broadcast.SubmitMetrics(attributeList.attributes, "event_source", true)
 
-- Flush the buffer and send all the metrics.
AWSMetricsRequestBus.Broadcast.FlushMetrics()
```

您还可以向事件添加多个量度属性。[事件 JSON 架构](./event-schema/)  中未包含的自定义量度属性将作为平面 JSON 字典添加到“`event_data`”字段中。

{{< note >}}
在提交之前，将根据 JSON 架构验证 metrics 事件。如果缺少任何必需的属性，或者属性值与预期模式不匹配，则会丢弃该事件。
{{< /note >}}

### 捕获通知

要在 Lua 中捕获通知，请在激活期间连接到“`AWSMetricsNotificationBus`”总线，并在停用期间断开与该总线的连接，如以下示例所示。

```lua
function sample:OnActivate()
    -- Connect to the AWSMetricsNotificationBus bus.
    self.metricsNotificationHandler = AWSMetricsNotificationBus.Connect(self, self.entityId)
end

function sample:OnSendMetricsSuccess(requestId)
    -- Do something after metrics events send successfully.
end
 
function sample:OnSendMetricsFailure(requestId, errorMessage)
    -- Do something when metrics events fail to send.
end
 
function sample:OnDeactivate()
    -- Disconnect from the AWSMetricsNotificationBus bus.
    self.metricsNotificationHandler:Disconnect()
end
```
