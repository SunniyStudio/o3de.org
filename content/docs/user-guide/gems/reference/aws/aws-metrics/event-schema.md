---
linktitle: Event Schema
title: AWS Metrics API Event Schema
description: 查看 JSON 事件架构，用于在 Open 3D Engine （O3DE） 中验证 AWS Metrics 输入事件。
toc: true
weight: 600
---

AWS Metrics Gem 提供以下 JSON 架构来验证输入事件。有关自定义架构的信息，请参阅有关 [Metrics Event Schema](./advanced-topics/#metrics-event-schema) 的高级主题。

```json
{
    "$schema": "http://json-schema.org/draft-04/schema#",
    "title": "AWSMetrics API Event Schema",
    "description": "Metrics Event sent to the service API",
    "type": "object",
    "additionalProperties": false,
    "properties": {
        "application_id": {
            "type": "string",
            "pattern": "^[0-9-.]+-\\{[0-9A-F]{8}-[0-9A-F]{4}-[1-5][0-9A-F]{3}-[89AB][0-9A-F]{3}-[0-9A-F]{12}\\}$",
            "description": "Identifier for the application that generated the event."
        },
        "event_id": {
            "type": "string",
            "pattern": "^\\{[0-9A-F]{8}-[0-9A-F]{4}-[1-5][0-9A-F]{3}-[89AB][0-9A-F]{3}-[0-9A-F]{12}\\}$",
            "description": "A random UUID that uniquely identifies an event."
        },
        "event_type": {
            "type": "string",
            "pattern": "^[A-Za-z0-9-_.]+$",
            "description": "Identifies the type of event."
        },
        "event_name": {
            "type": "string",
            "pattern": "^[A-Za-z0-9-_.]+$",
            "description": "Name of the event that occurred."
        },
        "event_timestamp": {
            "type": "string",
            "pattern": "^(\\d{4})-(\\d{2})-(\\d{2})T(\\d{2})\\:(\\d{2})\\:(\\d{2})Z$",
            "description": "Timestamp of the event in the UTC ISO8601 format."
        },
        "event_version": {
            "type": "string",
            "pattern": "^[A-Za-z0-9-_.]+$",
            "description": "An API version for this event format."
        },
        "event_source": {
            "type": "string",
            "pattern": "^[A-Za-z0-9-_.]+$",
            "description": "Source of the event."
        },
        "event_data": {
            "type": "object",
            "description": "Custom metrics attributes defined by this event."
        }
    },
    "required": [ "application_id", "event_id", "event_name", "event_timestamp" ]
}
```
