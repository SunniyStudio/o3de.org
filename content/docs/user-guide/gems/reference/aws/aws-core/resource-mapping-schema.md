---
title: JSON 资源映射架构
description: 查看 Open 3D Engine （O3DE） 中 AWS Gems 使用的 JSON 资源映射架构。
toc: true
weight: 320
---

AWS Core Gem 为资源映射提供以下 JSON 架构。

```json
{
    "$schema": "http://json-schema.org/draft-04/schema",
    "type": "object",
    "title": "The AWS Resource Mapping Root schema",
    "required": ["AWSResourceMappings", "AccountId", "Region", "Version"],
    "properties": {
        "AWSResourceMappings": {
            "type": "object",
            "title": "The AWSResourceMappings schema",
            "patternProperties": {
                "^.+$": {
                    "type": "object",
                    "title": "The AWS Resource Entry schema",
                    "required": ["Type", "Name/ID"],
                    "properties": {
                        "Type": {
                            "$ref": "#/NonEmptyString"
                        },
                        "Name/ID": {
                            "$ref": "#/NonEmptyString"
                        },
                        "AccountId": {
                            "$ref": "#/AccountIdString"
                        },
                        "Region": {
                            "$ref": "#/RegionString"
                        }
                    }
                }
            },
            "additionalProperties": false
        },
        "AccountId": {
            "$ref": "#/AccountIdString"
        },
        "Region": {
            "$ref": "#/RegionString"
        },
        "Version": {
            "pattern": "^[0-9]{1}.[0-9]{1}.[0-9]{1}$"
        }
    },
    "AccountIdString": {
        "type": "string",
        "pattern": "^[0-9]{12}$|EMPTY|^$"
    },
    "NonEmptyString": {
        "type": "string",
        "minLength": 1
    },
    "RegionString": {
        "type": "string",
        "pattern": "^[a-z]{2}-[a-z]{4,9}-[0-9]{1}$"
    },
    "additionalProperties": false
}
```
