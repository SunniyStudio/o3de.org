---
description: 使用质量系统定义质量级别和每个级别的控制台变量设置。
title: 质量系统
linkTitle: 质量系统
---

质量系统使您能够为每个质量级别定义质量级别和控制台变量设置。应用程序通常在不同的硬件上运行，并且需要不同的设置才能在每个设备上实现最佳性能并适应用户首选项。


## 质量组

可以在 [设置注册表](/docs/user-guide/settings/) `.setreg`文件中定义质量组（例如 `q_general`, `q_graphics`, `q_physics` 等），以便更轻松地在应用程序中对类似设置进行分组。 可以在每个组中定义质量级别（例如`Low`, `Medium`, `High`, `VeryHigh`）以满足您的应用需求。

质量系统按需为每个质量组创建控制台变量，以便您可以在运行时使用控制台更改该组的活动质量级别，因此，我们建议您使用带有`q_`前缀的小写组名称，该前缀是`quality`的缩写。 例如，您可以为游戏定义一个名为 `q_game` 的质量组，并在运行时使用控制台命令设置质量级别，如下所示，其中`<quality level>`是质量级别的数字索引或文本名称。

```
q_game <quality level>
```

质量组对象应在路径 `O3DE/Quality/Groups` 中定义，并具有以下格式。

```json
{
    "O3DE": {
        "Quality": {
            "Groups": {
                "q_general": {
                    "Description" : "Description with values e.g.. 0 : Low, 1 : Medium, 2 : High",
                    "Levels": [ // 质量级别名称
                        "Low",
                        "Medium",
                        "High",
                        ...
                    ],
                    "Default": 2, // Levels 数组中的 default 质量级别索引
                    "Settings": { // 每个质量级别的控制台变量设置
                        // 可以为每个级别提供 set 值的数组
                        "r_example":[32, 64, 128],
                        // 可以提供单个值供每个质量级别使用
                        "r_example2": 2,
                        // 如果设置没有为所有质量级别定义值，则
                        // 数组中的最后一个值将用于更高的质量级别
                        "r_example3": [1, 2] ,
                        ...
                    }
                }
            }
        }
    }
}
```

应用程序启动时使用的默认质量组在路径 `O3DE/Quality/DefaultGroup` 中定义。
```json
{
    "O3DE": {
        "Quality": {
            "DefaultGroup": "q_general"
        }
    }
}
```

O3DE 在 `Registry/quality.setreg` 中包含一个质量组 `q_general` ，它定义了 4 个质量级别`Low`, `Medium`, `High`, 和 `VeryHigh`，并将默认级别定义为 `High`。

#### O3DE/Registry/quality.setreg
```json
{
    "O3DE": {
        "Quality": {
            "DefaultGroup": "q_general",
            "Groups": {
                "q_general": {
                    "Description" : "Default quality group. 0 : Low, 1 : Medium, 2 : High, 3 : VeryHigh",
                    "Levels": [
                        "Low",
                        "Medium",
                        "High",
                        "VeryHigh"
                    ],
                    "Default": 3,
                    "Settings": {
                        // 控制台变量设置
                    }
                }
            }
        }
    }
}
```

Atom Feature Common Gem 还包括一个`Registry/quality.setreg`文件，该文件定义图形的质量组`q_graphics`和`q_shadows`，并演示如何创建更改其他组质量级别的设置。

#### O3DE/Gems/Atom/Feature/Common/Registry/quality.setreg
```json
{
    "O3DE": {
        "Quality": {
            "Groups": {
                "q_general": {
                    "Settings": {
                        "q_graphics": [ 0, 1, 2, 3 ] // map q_general levels 1 to 1 with graphics levels
                    }
                },
                "q_graphics": {
                    "Description": "Graphics quality settings.  0 : Low, 1 : Medium, 2 : High, 3 : VeryHigh",
                    "Levels": [
                        "Low",
                        "Medium",
                        "High",
                        "VeryHigh"
                    ],
                    "Default": 3,
                    "Settings": {
                        "q_shadows": [ 0, 1, 2, 3 ]
                    }
                },
                "q_shadows": {
                    "Description": "Shadow quality settings.  0 : Low, 1 : Medium, 2 : High, 3 : VeryHigh",
                    "Levels": [
                        "Low",
                        "Medium",
                        "High",
                        "VeryHigh"
                    ],
                    "Settings": {
                        // shadows console variable settings
                    }
                }
            }
        }
    }
}
```

在上面的示例中，当用户将`q_general`质量组的质量级别设置为`Low` 或 `0`时，`q_graphics` 和 `q_shadows`的质量级别也将设置为该级别。

## 质量级别

可以在每个质量组 JSON 对象的设置注册表 JSON 文件中定义质量级别。 

每个组 JSON 对象中的质量级别应按以下格式定义。
```json
{
    "O3DE": {
        "Quality": {
            "Groups": {
                "q_general": {
                    "Levels": [ // quality level names should be a single word
                        "Low",
                        "Medium",
                        "High",
                        "VeryHigh"
                        ...
                    ]
                }
            }
        }
    }
}
```

每个组都可以使用自定义名称和编号定义自己的质量级别。 在为不一定更好或更差的设置定义级别时，提供`Low`, `High`等质量级别名称可能很有用，例如，您可能有 `HighPerformance`, `Balanced`, `EnergyEfficient`等功耗级别设置。

## 为您的项目自定义质量级别

要覆盖和自定义引擎和项目使用的 Gem 中的质量设置，请在项目`Registry`文件夹中定义您自己的`.setreg` 文件，其中包含相应的 JSON 内容。 例如，如果您想将游戏中的默认质量级别覆盖为`VeryHigh`，并为特定质量级别提供特定的阴影控制台变量，则可以将以下`.setreg` 文件添加到您的项目中。

### Project/Registry/quality.setreg

```json
{
    "O3DE": {
        "Quality": {
            "Groups": {
                "q_general": {
                    "Default": 3 // 将默认级别更改为 VeryHigh 而不是 High
                },
                "q_shadows": {
                    "Settings": {
                        // 4 种质量中每一种的控制台变量设置示例
                        // q_shadows 中的级别 (Low, Medium, High, VeryHigh)
                        "r_shadowResolution": [128, 512, 1024, 4096]
                    }
                }
            }
        }
    }
}
```

## 特定于平台的质量设置

由于 Settings Registry 中内置了现有的平台抽象，因此可以轻松地为特定平台自定义质量设置。 通过将高质量的 `.setreg` 文件放在适当的 `Registry/Platform/<platform name>` 文件夹中，它们将用于该特定平台并覆盖 `Registry/` 文件夹中的任何设置。

例如，要更改 Android 平台上的默认质量级别，您可以将以下 `.setreg`文件添加到您的项目中。

### Project/Registry/Platform/Android/quality.setreg

```json
{
    "O3DE": {
        "Quality": {
            "Groups": {
                "q_general": {
                    "Default": 0 // 将 default level 更改为 Low
                }
            }
        }
    }
}
```

自定义其他平台的质量设置就像将 Settings Registry 文件放在相应的平台文件夹中一样简单。

`<project path>/Registry/Platform/Android|iOS|Linux|Mac|Windows`
