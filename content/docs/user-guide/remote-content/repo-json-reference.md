---
linkTitle: Repo.json 参考 
title: Repo.json 参考
description: O3DE远程存储库repo.json文件引用。
weight: 600
toc: true
---

## `repo.json` 参考

| 字段 | 必须 | 说明 |
|-|-|-|
| $schemaVersion | **Required** | 解析此文件时应使用的 JSON 架构版本。 当前默认值为 `1.0.0`. |
| additional_info | Optional | 有关此 O3DE 远程存储库及其提供的内容的其他信息。 |
| gems_data | Optional | 此存储库提供的每个 Gem 的 Gem JSON 数据列表。每个 JSON 词典中的`versions_data`字段包含每个版本的更改字段。 |
| last_updated | Optional | 上次以 ISO8601 格式更新 UTC 偏移量此文件的日期和时间 YYYY-MM-DDTHH:MM:SS.mmmmmmTZD  (e.g. 2012-03-29T10:05:45.12345+06:00).  还接受以下格式:`YYYY-MM-DD`, `YYYY-MM-DD HH:MM:SS`, 或 `YYYY-MM-DDTHH:MM:SS`. 
| origin | **Required** | 此 O3DE 远程存储库的发起者的名称：即 XYZ Inc. |
| origin_url | Optional | 此远程仓库的创建者（作者或所有者）的 URL。 |
| projects_data | Optional | 此存储库提供的每个项目的 Project JSON 数据列表。 每个 JSON 词典中的 `versions_data`字段包含每个版本的更改字段。 |
| repo_name | **Required** | 此远程存储库的名称。 此名称必须少于 64 个字符，并且仅包含字母数字、“_”或“-”字符，并以字母开头。 |
| repos | Optional | 其他远程存储库 URI 的列表。 当此远程存储库中的 Gem、项目或模板依赖于另一个远程存储库中的 Gem 时，这可能非常有用。 |
| repo_uri | **Required** | 此远程存储库的 URI。 |
| summary | Optional | 此 O3DE 远程存储库的描述。 |
| templates_data | Optional | A此存储库提供的每个模板的模板 JSON 数据列表。每个 JSON 词典中的`versions_data`字段包含每个版本的更改字段。 |

## 示例 `repo.json` 

```json
{
    "$schemaVersion": "1.0.0",
    "repo_name":"Example O3DE Remote Repository",
    "origin":"o3de-example-repository",
    "repo_uri":"https://github.com/o3de/o3de-extras",
    "summary": "An example O3DE remote repository with a single gem.",
    "additional_info": "Additional information about your repository including HTML links to any relative website, documentation or licenses.",
    "last_updated": "2023-09-27",
    "gems_data":[
        {
            "gem_name": "ExampleGem1",
            "version": "1.0.0",
            "display_name": "Example Remote Gem 1",
            "license": "Apache-2.0 Or MIT",
            "license_url": "https://github.com/o3de/o3de/blob/development/LICENSE.txt",
            "origin": "Open 3D Engine - o3de.org",
            "origin_url": "https://github.com/o3de/o3de-extras",
            "type": "Code",
            "summary": "This is an example remote gem",
            "canonical_tags": [
                "Gem"
            ],
            "user_tags": [
                "ExampleGem1"
            ],
            "icon_path": "preview.png",
            "requirements": "",
            "documentation_url": "",
            "dependencies": [],
            "repo_uri": "https://github.com/o3de/o3de-extras",
            "versions_data": [
                {
                    "version": "1.0.0",
                    "download_source_uri": "https://github.com/o3de/o3de-extras/releases/download/2.0/examplegem1-1.0.0-gem.zip",
                    "sha256": "ef75e9811b11e081bd4d16d62b638208fe9f0bd8966cfaff937e64b59343f5f7"
                }
            ]
        }
    ],
    "projects_data":[],
    "templates_data":[],
    "repos":[]
}
```
