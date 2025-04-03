---
linkTitle: 描述符
title: Python Asset Builder 描述符
description: Python 资产创建器会在Open 3D Engine (O3DE)的资产处理器中注册创建器。
weight: 200
toc: true
---

描述符为**资产处理器**提供了识别**Python Asset Builder**所需的信息，以便它能调用创建作业和处理作业事件的回调。它还注册了 Python 资产创建器可处理的源文件类型的文件模式。要注册 Python 资产生成器，需要定义一个函数来创建资产生成器描述符 (`azlmbr.asset.builder.AssetBuilderDesc`)并注册 Python 资产生成器(`azlmbr.asset.builder.PythonAssetBuilderRequestBus`)。

## Asset Builder Descriptor

`azlmbr.asset.builder.AssetBuilderDesc` 是一个提供四个字段的类，用于描述从 Python 资产生成器到资产处理器的过程。

| 字段 | 类型 | 说明 |
| - | - | - |
| `busId` | `azlmbr.math.Uuid` | 资产创建器的通用唯一 ID (UUID)。 |
| `name` | String | Asset Builder的名称。 |
| `patterns` | List[`azlmbr.asset.builder.AssetBuilderPattern`] | 资产创建器可处理的文件模式集合。 |
| `version` | Number | 资产创建工具的版本号。 |

{{< note >}}
资产处理器使用资产生成器的 UUID 和版本号来跟踪产品资产和资产生成器生成的作业。如果 UUID 或版本号被修改，以前由资产生成器处理过的所有作业都会被重新处理。当资产创建器被修改时，增加 `version` 值，以确保所有资产都使用最新的资产创建器重新处理。
{{< /note >}}

### 创建UUID

`busId` 字段是一个 UUID，资产处理器用它来识别和跟踪来自资产生成器的流程作业。它还用于为资产生成器生成的产品资产生成子 ID。更改 UUID 会触发重新处理以前由资产生成器处理过的所有源资产。可以使用 `uuid` Python 模块为 `busId` 字段生成 UUID。

```python
> python
>>> import uuid
>>> uuid.uuid4()
UUID('639f403e-1b7e-4cfe-a250-90e6767247cb')
```

### 注册文件模式

`patterns`字段是一个`AssetBuilderPattern`列表，用于指定资产生成器可以处理的资产类型。模式可以是文件扩展名的通配符，如 `*.myasset`，也可以是文件和文件夹的正则表达式，如`^[a-zA-Z]:\\MyAssets[\\\S|*\S]?.*$)`。

`azlmbr.asset.builder.AssetBuilderPattern` 是一种结构，它定义了源资产要使用的模式类型。

| 字段 | 类型 | 说明 |
| - | - | - |
| `type` | `azlmbr.asset.builder.AssetBuilderPattern Type` | 资产模式的类型： `azlmbr.asset.builder.AssetBuilderPattern_Wildcard` 或 `azlmbr.asset.builder.AssetBuilderPattern_Regex`. |
| `pattern` | String | 要使用的文件路径模式。 |

## 注册 Python 资产创建器

必须注册 Python 资产创建器，这样资产处理器才能意识到它的存在，并对支持的资产类型的创建任务和处理任务事件做出响应。

要注册资产生成器，请将脚本与`azlmbr.asset.builder.PythonAssetBuilderRequestBus`绑定到资产生成过程。成功注册 Python 资产生成器后，创建一个`azlmbr.asset.builder.PythonBuilderNotificationBusHandler`。

`azlmbr.asset.builder.PythonAssetBuilderRequestBus` 是一个单例 EBus，提供启用 Python 资产构建器的方法。

| 类型 | 说明 | 输入 | 输出 |
| - | - | - | - |
| `RegisterAssetBuilder` | 使用生成器描述符注册资产生成器。 | `azlmbr.asset.builder.AssetBuilderDesc` | `Outcome_bool` |
| `GetExecutableFolder` | 返回当前可执行文件夹。 |  | `Outcome_string` |

## 添加通知总线处理程序

资产创建器使用通知总线处理程序来处理 “创建作业 ”和 “处理作业 ”事件。处理程序必须在全局模块范围内创建，这样回调才能保持激活状态。

创建一个`azlmbr.asset.builder.PythonBuilderNotificationBusHandler`。将资产创建器的 `busId` 连接到通知总线处理程序，并为创建作业和处理作业提供回调处理程序。

| 处理程序 | 说明 | 输入 | 输出 |
| - | - | - | - |
| `OnCreateJobsRequest` | 回调函数类型，用于根据作业请求创建作业。 | Tuple(`azlmbr.asset.builder.CreateJobsRequest`) | `azlmbr.asset.builder.CreateJobsResponse` |
| `OnProcessJobRequest` | 回调函数类型，用于处理来自处理作业请求的作业。 | `azlmbr.asset.builder.ProcessJobRequest` | `azlmbr.asset.builder.ProcessJobResponse` |

## 示例： 注册 Python 资产创建器

下面的示例定义了一个完整的描述符，并注册了一个 Python 资产创建器。

```python
import azlmbr.asset
import azlmbr.asset.builder
import azlmbr.bus
import azlmbr.math

busId = azlmbr.math.Uuid_CreateString('{CF5C74D1-9ED4-4851-85B1-9B15090DBEC7}', 0)

def on_create_jobs(args):
    # TODO: create jobs logic.
    return azlmbr.asset.builder.CreateJobsResponse()

def on_process_job(args):
    # TODO: process job logic.
    return azlmbr.asset.builder.ProcessJobResponse()

# register asset builder
def register_asset_builder():
    assetPattern = azlmbr.asset.builder.AssetBuilderPattern()
    assetPattern.pattern = '*.newasset'
    assetPattern.type = azlmbr.asset.builder.AssetBuilderPattern_Wildcard

    builderDescriptor = azlmbr.asset.builder.AssetBuilderDesc()
    builderDescriptor.name = "New Asset"
    builderDescriptor.patterns = [assetPattern]
    builderDescriptor.busId = busId
    builderDescriptor.version = 1

    outcome = azlmbr.asset.builder.PythonAssetBuilderRequestBus(azlmbr.bus.Broadcast, 'RegisterAssetBuilder', builderDescriptor)
    if outcome.IsSuccess():
        # created the asset builder to hook into the notification bus
        h = azlmbr.asset.builder.PythonBuilderNotificationBusHandler()
        h.connect(busId)
        h.add_callback('OnCreateJobsRequest', on_create_jobs)
        h.add_callback('OnProcessJobRequest', on_process_job)
        return h

# the module global asset builder handler
handler = register_asset_builder()
```
