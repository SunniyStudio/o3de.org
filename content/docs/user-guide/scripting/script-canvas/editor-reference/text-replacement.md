---
linkTitle: 节点文本替换
title: Script Canvas 节点文本替换
description: 了解如何在 Open 3D Engine （O3DE） 中自定义 Script Canvas 节点上的文本。
weight: 500
---

**Script Canvas** 有一个文本替换系统，允许进一步自定义所有可用节点。方法名称、类别、参数名称和工具提示都可以更新，以改进它们在 Script Canvas 图形中的措辞。

## 运作方式

创建 Script Canvas 节点时，开发人员可以提供标识和阐明详细信息，例如节点名称、它们在 **Node Palette** 中的类别、其参数和返回值的名称以及其工具提示。但是，所有这些详细信息都可以使用文本替换文件覆盖。

文本替换文件使用 JSON 格式和文件扩展名 `.names`。您可以在目录中找到为节点生成并包含在 O3DE 中的所有文本替换文件 `[Gems/ScriptCanvas/Assets/TranslationAssets](https://github.com/o3de/o3de/tree/development/Gems/ScriptCanvas/Assets/TranslationAssets)`.

您可以使用 Script Canvas 编辑器为新节点或现有节点生成或更新`.names`文件，如以下部分所示。

**Asset Processor** 处理 Script Canvas 编辑器的所有`.names`文件，以便在节点调色板中以及在图表中绘制节点时使用。

## 更新文本的规则

要自定义`.names` 文件中的节点，您可以更新任何 `details` 字段中包含的以下任何字段：

| 字段 | 说明 |
| --- | --- |
| `name` | 节点、参数或返回类型的名称。 |
| `category` | Node Palette （节点调色板） 中要将节点放置在其中的类别。通过使用 “/” 字符支持嵌套类别。示例：“Gameplay/Missions”。 |
| `tooltip` | 提供元素的工具提示。这可以是节点本身，也可以是在方法或参数上添加工具提示时的任何节点插槽。 |
| `subtitle` | 这可用于条目 details 对象。它会覆盖节点的 subtitle。 |

{{< important >}}
`.names` 文件中的某些字段不应被修改。系统使用它们在文本字段数据库中创建键。
{{< /important >}}

**以下字段不应更改**:

* `base`
* `context`
* `variant`
* `typeid`

对这些字段的任何更改都会导致 Script Canvas Editor 无法找到要更新的文本，并导致翻译数据被忽略。

## 开始

### 示例：生成翻译文件以替换文本

1. 打开 Script Canvas Editor。

1. 在 Node Palette 中导航或搜索所需的节点。在此示例中，我们将使用 AWSGameLiftPlayer。

    ![AWSGameLiftPlayer cateogry displayed in the Node Palette](/images/user-guide/scripting/script-canvas/text-replacement-0.png)

1. 右键单击 AWSGameLiftPlayer 下的任何节点。

    ![Context menu displayed after right-clicking an AWSGameLiftPlayer node](/images/user-guide/scripting/script-canvas/text-replacement-1.png)

1. 选择 **Generate Translation**。这将打开一个文件资源管理器窗口，其中包含一个名为`AWSGameLiftPlayer.names`的自动生成的文件。

    如果您不想对自动生成的数据进行任何更改，可以关闭 Script Canvas Editor 窗口并重新打开它。将显示生成或编辑 `.names` 文件所做的任何更改。

    ![Nodes names in AWS Game Lift Player category displayed with spaces](/images/user-guide/scripting/script-canvas/text-replacement-2.png)

    在此示例中，所有名称现在都用空格分隔，并且命名遵循 Script Canvas 命名约定，使用 camel 大小写。

### 示例：更改节点类别

您已成功为某个节点生成翻译数据，但其类别仍位于 “Other” 下。要更改此设置，请编辑生成的文件`AWSGameLiftPlayer.names`.

1. 在文件中导航到最外层的  `details` 字段，然后添加
`"category": "AWS Game Lift"`.

1. 此外，简化 `name` 从 `"AWS Game Lift Player"` 到 `"Player"`.

    ```json
    {
        "entries": [
            {
                "base": "AWSGameLiftPlayer",
                "context": "BehaviorClass",
                "variant": "",
                "details": {
                    "category": "AWS Game Lift",
                    "name": "Player"
                },
                "methods": [
                    ... STRIPPED FOR SIZE ... (See Appendix 1 for complete file before/after)
                ]
            }
        ]
    }
    ```

1. 将更改保存到 `AWSGameLiftPlayer.names`, 关闭 Script Canvas Editor 并再次打开它。

    节点现在位于具有给定名称的正确类别中：

    ![Node palette with Player nodes placed in AWS Game Lift category](/images/user-guide/scripting/script-canvas/text-replacement-3.png)

### 示例：更新翻译文件

在这种情况下，您无需生成新的翻译文件。相反，您只需对其进行编辑。

1. 右键单击 AWSGameLiftPlayer 下的任何节点。

    ![Context menu displayed after right-clicking an AWSGameLiftPlayer node](/images/user-guide/scripting/script-canvas/text-replacement-1.png)

1. 在上下文菜单中选择 **Explore Translation Data**。它将在包含相关翻译数据的文件上打开一个文件资源管理器窗口。在此示例中，文件为 `AWSGameLiftPlayer.names`.

1. 在您喜欢的文本编辑器中打开文件。

1. 查找并更新任何所需的元素。

1. 保存对 `AWSGameLiftPlayer.names`的更改，关闭 Script Canvas 编辑器，然后再次打开它。您应该会看到对节点的更新。

## 最佳实践

在处理`.names`文件时，请使用以下文本替换最佳实践。

* 不要修改 `base`, `context`, `variant`, or `typeid` 字段。
* 使用带空格的驼峰式大小写语法。
* 添加`tooltip` 字段以提供详细信息和上下文。
* 使用 `category` 字段来组织节点。
* 通过关闭并重新打开 Script Canvas 编辑器来检查您的更改。
* 注意`.names`文件上的 Asset Processor 错误。这些可能是由 JSON 语法中的错误引起的。
* 将 `.names` 文件提交到源代码管理;这将确保它们被跟踪并且不会覆盖更改。
* 生成翻译数据将覆盖现有翻译数据。小心不要丢失工作。如有疑问，请先使用 **Explore Translation Data** 菜单操作。
* 不同类型的节点会生成不同类型的 `.names` 文件。一些`.names`文件定义单个节点;其他 API 在单个文件中定义多个节点。

## 附录

### 默认生成 `AWSGameLiftPlayer.names`

```json
{
    "entries": [
        {
            "base": "AWSGameLiftPlayer",
            "context": "BehaviorClass",
            "variant": "",
            "details": {
                "name": "AWS Game Lift Player"
            },
            "methods": [
                {
                    "base": "GetLatencyInMs",
                    "details": {
                        "name": "Get Latency In Ms"
                    },
                    "params": [
                        {
                            "typeid": "{B62C118E-C55D-4903-8ECB-E58E8CA613C4}",
                            "details": {
                                "name": "AWS Game Lift Player"
                            }
                        }
                    ],
                    "results": [
                        {
                            "typeid": "{3F80885A-9011-5172-8D94-87108B31D950}",
                            "details": {
                                "name": "Latency In Ms"
                            }
                        }
                    ]
                },
                {
                    "base": "SetLatencyInMs",
                    "details": {
                        "name": "Set Latency In Ms"
                    },
                    "params": [
                        {
                            "typeid": "{B62C118E-C55D-4903-8ECB-E58E8CA613C4}",
                            "details": {
                                "name": "AWS Game Lift Player"
                            }
                        },
                        {
                            "typeid": "{3F80885A-9011-5172-8D94-87108B31D950}",
                            "details": {
                                "name": "Latency In Ms"
                            }
                        }
                    ]
                },
                {
                    "base": "GetPlayerAttributes",
                    "details": {
                        "name": "Get Player Attributes"
                    },
                    "params": [
                        {
                            "typeid": "{B62C118E-C55D-4903-8ECB-E58E8CA613C4}",
                            "details": {
                                "name": "AWS Game Lift Player"
                            }
                        }
                    ],
                    "results": [
                        {
                            "typeid": "{F8A7460C-2CC2-5755-AFDA-49B1109A751E}",
                            "details": {
                                "name": "Player Attributes"
                            }
                        }
                    ]
                },
                {
                    "base": "SetPlayerAttributes",
                    "details": {
                        "name": "Set Player Attributes"
                    },
                    "params": [
                        {
                            "typeid": "{B62C118E-C55D-4903-8ECB-E58E8CA613C4}",
                            "details": {
                                "name": "AWS Game Lift Player"
                            }
                        },
                        {
                            "typeid": "{F8A7460C-2CC2-5755-AFDA-49B1109A751E}",
                            "details": {
                                "name": "Player Attributes"
                            }
                        }
                    ]
                },
                {
                    "base": "GetPlayerId",
                    "details": {
                        "name": "Get Player Id"
                    },
                    "params": [
                        {
                            "typeid": "{B62C118E-C55D-4903-8ECB-E58E8CA613C4}",
                            "details": {
                                "name": "AWS Game Lift Player"
                            }
                        }
                    ],
                    "results": [
                        {
                            "typeid": "{03AAAB3F-5C47-5A66-9EBC-D5FA4DB353C9}",
                            "details": {
                                "name": "Player Id"
                            }
                        }
                    ]
                },
                {
                    "base": "SetPlayerId",
                    "details": {
                        "name": "Set Player Id"
                    },
                    "params": [
                        {
                            "typeid": "{B62C118E-C55D-4903-8ECB-E58E8CA613C4}",
                            "details": {
                                "name": "AWS Game Lift Player"
                            }
                        },
                        {
                            "typeid": "{03AAAB3F-5C47-5A66-9EBC-D5FA4DB353C9}",
                            "details": {
                                "name": "Player Id"
                            }
                        }
                    ]
                },
                {
                    "base": "GetTeam",
                    "details": {
                        "name": "Get Team"
                    },
                    "params": [
                        {
                            "typeid": "{B62C118E-C55D-4903-8ECB-E58E8CA613C4}",
                            "details": {
                                "name": "AWS Game Lift Player"
                            }
                        }
                    ],
                    "results": [
                        {
                            "typeid": "{03AAAB3F-5C47-5A66-9EBC-D5FA4DB353C9}",
                            "details": {
                                "name": "Team"
                            }
                        }
                    ]
                },
                {
                    "base": "SetTeam",
                    "details": {
                        "name": "Set Team"
                    },
                    "params": [
                        {
                            "typeid": "{B62C118E-C55D-4903-8ECB-E58E8CA613C4}",
                            "details": {
                                "name": "AWS Game Lift Player"
                            }
                        },
                        {
                            "typeid": "{03AAAB3F-5C47-5A66-9EBC-D5FA4DB353C9}",
                            "details": {
                                "name": "Team"
                            }
                        }
                    ]
                }
            ]
        }
    ]
}
```

### 修改 `AWSGameLiftPlayer.names`

```json
{
    "entries": [
        {
            "base": "AWSGameLiftPlayer",
            "context": "BehaviorClass",
            "variant": "",
            "details": {
                "name": "Player",
                "category": "AWS Game Lift"
            },
            "methods": [
                {
                    "base": "GetLatencyInMs",
                    "details": {
                        "name": "Get Latency In Ms"
                    },
                    "params": [
                        {
                            "typeid": "{B62C118E-C55D-4903-8ECB-E58E8CA613C4}",
                            "details": {
                                "name": "Player"
                            }
                        }
                    ],
                    "results": [
                        {
                            "typeid": "{3F80885A-9011-5172-8D94-87108B31D950}",
                            "details": {
                                "name": "Latency In Ms"
                            }
                        }
                    ]
                },
                {
                    "base": "SetLatencyInMs",
                    "details": {
                        "name": "Set Latency In Ms"
                    },
                    "params": [
                        {
                            "typeid": "{B62C118E-C55D-4903-8ECB-E58E8CA613C4}",
                            "details": {
                                "name": "Player"
                            }
                        },
                        {
                            "typeid": "{3F80885A-9011-5172-8D94-87108B31D950}",
                            "details": {
                                "name": "Latency In Ms"
                            }
                        }
                    ]
                },
                {
                    "base": "GetPlayerAttributes",
                    "details": {
                        "name": "Get Player Attributes"
                    },
                    "params": [
                        {
                            "typeid": "{B62C118E-C55D-4903-8ECB-E58E8CA613C4}",
                            "details": {
                                "name": "Player"
                            }
                        }
                    ],
                    "results": [
                        {
                            "typeid": "{F8A7460C-2CC2-5755-AFDA-49B1109A751E}",
                            "details": {
                                "name": "Player Attributes"
                            }
                        }
                    ]
                },
                {
                    "base": "SetPlayerAttributes",
                    "details": {
                        "name": "Set Player Attributes"
                    },
                    "params": [
                        {
                            "typeid": "{B62C118E-C55D-4903-8ECB-E58E8CA613C4}",
                            "details": {
                                "name": "Player"
                            }
                        },
                        {
                            "typeid": "{F8A7460C-2CC2-5755-AFDA-49B1109A751E}",
                            "details": {
                                "name": "Player Attributes"
                            }
                        }
                    ]
                },
                {
                    "base": "GetPlayerId",
                    "details": {
                        "name": "Get Player Id"
                    },
                    "params": [
                        {
                            "typeid": "{B62C118E-C55D-4903-8ECB-E58E8CA613C4}",
                            "details": {
                                "name": "Player"
                            }
                        }
                    ],
                    "results": [
                        {
                            "typeid": "{03AAAB3F-5C47-5A66-9EBC-D5FA4DB353C9}",
                            "details": {
                                "name": "Player Id"
                            }
                        }
                    ]
                },
                {
                    "base": "SetPlayerId",
                    "details": {
                        "name": "Set Player Id"
                    },
                    "params": [
                        {
                            "typeid": "{B62C118E-C55D-4903-8ECB-E58E8CA613C4}",
                            "details": {
                                "name": "Player"
                            }
                        },
                        {
                            "typeid": "{03AAAB3F-5C47-5A66-9EBC-D5FA4DB353C9}",
                            "details": {
                                "name": "Player Id"
                            }
                        }
                    ]
                },
                {
                    "base": "GetTeam",
                    "details": {
                        "name": "Get Team"
                    },
                    "params": [
                        {
                            "typeid": "{B62C118E-C55D-4903-8ECB-E58E8CA613C4}",
                            "details": {
                                "name": "Player"
                            }
                        }
                    ],
                    "results": [
                        {
                            "typeid": "{03AAAB3F-5C47-5A66-9EBC-D5FA4DB353C9}",
                            "details": {
                                "name": "Team"
                            }
                        }
                    ]
                },
                {
                    "base": "SetTeam",
                    "details": {
                        "name": "Set Team"
                    },
                    "params": [
                        {
                            "typeid": "{B62C118E-C55D-4903-8ECB-E58E8CA613C4}",
                            "details": {
                                "name": "Player"
                            }
                        },
                        {
                            "typeid": "{03AAAB3F-5C47-5A66-9EBC-D5FA4DB353C9}",
                            "details": {
                                "name": "Team"
                            }
                        }
                    ]
                }
            ]
        }
    ]
}
```
