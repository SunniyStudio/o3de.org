---
linkTitle: CreateJobs
title: Python Asset Builder CreateJobs
description: 使用 Python Asset Builder 中的 CreateJobs 为资产处理器创建流程作业信息。
weight: 300
toc: true
---

CreateJobs 为**资产处理器**生成资产处理任务。当资产处理器检测到新的或更新的源资产并确定合适的**资产生成器**来处理源资产时，它会向资产生成器发送包含源资产信息（包括其路径）的`CreateJobsRequest`。资产生成器会以 `CreateJobsResponse`作为回应，其中包含 `JobDescriptor` 结构以及源和作业依赖关系。

## CreateJobsRequest

`azlmbr.asset.builder.CreateJobsRequest` 是一个提供作业请求信息的类。它由资产处理器发送给资产生成器。它提供的数据用于创建`JobDescriptor`，以便为特定目标平台处理源资产。

| 字段 | 类型 | 说明 |
| - | - | - |
| `builderId` | `azlmbr.math.Uuid` | 确定资产创建者。 |
| `watchFolder` | String | 包含源资产的子目录。 |
| `sourceFile` | String | 相对于扫描目录的源资产路径。 |
| `sourceFileUUID` | `azlmbr.math.Uuid` | 源资产的 UUID，用作产品资产 ID 的一部分。 |
| `enabledPlatforms` | List[`azlmbr.asset.builder.PlatformInfo`] | 有关主机和目标平台的信息。 |

### PlatformInfo

`azlmbr.asset.builder.PlatformInfo` 是一个提供项目已启用的主机和目标平台信息的类。

| 字段 | 类型 | 说明 |
| - | - | - |
| `identifier` | 字符串 | 平台 ID，如 “pc ”或 “ios”。 |
| `tags` | List[String] | 平台可用的标签。 |

## CreateJobsResponse

`azlmbr.asset.builder.CreateJobsResponse` 是一个决定如何处理源资产的类。它为每个源资产以及每个主机和目标平台创建一个 `JobDescriptor` 。它还提供有关源依赖性的信息，以及 `CreateJobsRequest`的结果。

| 字段 | 类型 | 说明 |
| - | - | - |
| `result` | `azlmbr.asset.builder.CreateJobsResponse Return Code` | `CreateJobsRequest`的结果代码。 |
| `sourceFileDependencyList` | List[`SourceFileDependency`] | 运行源资产处理任务所需的依赖项。 |
| `createJobOutputs` | List[`JobDescriptor`] | 每个源资产以及每个主机和目标平台的 `JobDescriptor` 结构列表。 |

### CreateJobsResponse 返回代码

`CreateJobsResponse result` 包含来自 `CreateJobs` 请求的下列 `azlmbr.asset.builder.CreateJobsResponse` 返回代码之一。

| 返回代码 | 说明 |
| - | - |
| `azlmbr.asset.builder.CreateJobsResponse_ResultFailed` | 创建任务失败。 |
| `azlmbr.asset.builder.CreateJobsResponse_ResultShuttingDown` | 资产生成器正在关闭。 |
| `azlmbr.asset.builder.CreateJobsResponse_ResultSuccess` | 创建任务成功，|

### SourceFileDependency

`azlmbr.asset.builder.SourceFileDependency` 是一个包含依赖源文件的路径和 ID 的类。

| 字段 | 类型 | 说明 |
| - | - | - |
| `sourceFileDependencyPath` | String | 依赖项的路径（相对于资产目录或绝对路径）。|
| `sourceFileDependencyUUID` | `azlmbr.math.Uuid` | 依赖关系的 UUID（不含子 ID）。 |
| `sourceDependencyType` | `azlmbr.asset.builder.SourceFileDependency Type` | 绝对依赖关系或通配符匹配 (`azlmbr.asset.builder.SourceFileDependency_Absolute` 是默认值)。 |

{{< note >}}
资产生成器无需同时向**资产处理器**提供`sourceFileDependencyUUID`和 `sourceFileDependencyPath` 信息。只需提供其中一个即可。
{{< /note >}}

### SourceFileDependency 类型

`azlmbr.asset.builder.SourceFileDependency Type` 指定绝对依赖关系或通配符匹配。

| 类型 | 说明 |
| - | - |
| `azlmbr.asset.builder.SourceFileDependency_Absolute` | 绝对源文件依赖关系。 |
| `azlmbr.asset.builder.SourceFileDependency_Wildcards` | 允许通配符匹配。 |

## JobDescriptor

`azlmbr.asset.builder.JobDescriptor` 是资产创建器用来存储工作相关信息的一个类。

| 字段 | 类型 | 说明 |
| - | - | - |
| `jobParameters` | `JobParameterMap` | 传递给 `ProcessJobRequest` 的资产生成器特定参数。 |
| `additionalFingerprintInfo` | String | 打指纹时应考虑的其他信息。 |
| `jobKey` | String | 特定任务键，例如 TIFF 任务。 |
| `priority` | Integer | 作业队列中作业的优先级值。 |
| `checkExclusiveLock` | Boolean | `True`时，在处理任务前尝试获取源资产文件的独占锁。 |
| `checkServer` | Boolean | 在处理作业前，检查服务器是否有该作业的输出。 |
| `jobDependencyList` | List[`azlmbr.asset.builder.JobDependency`] | 对于希望声明工作依赖于其他工作的工作来说是必需的。 |
| `failOnError` | Boolean | 当 `True` 时，错误、断言和异常会自动导致作业失败。|
| `set_platform_identifier(platformIdentifier:string)` | Method | 设置构建平台的标识符。 |
| `get_platform_identifier()` | Method | 返回构建平台的标识符。 |

`priority`字段是作业队列中作业的值。优先级值小于 `0` 表示不考虑作业的优先级。优先级值大于等于 `0`，则作业的优先级按值排列。值越大，优先级越高。

{{< note >}}
关键工作和非关键工作的优先级分别设置。
{{< /note >}}

`checkExclusiveLock` 字段是一个标志，用于确定资产处理器在处理作业前是否需要检查源资产文件的独占锁。资产处理器会锁定和解锁源资产文件，以确保它不会被其他进程打开。这可防止过早处理某些已打开可写入但零字节时间超过修改阈值的源资产文件。如果资产处理器无法获得独占锁，则会超时。

`checkServer` 字段决定 Asset Processor 在本地开始处理作业前是否需要检查服务器上是否有该作业的输出。如果 Asset Processor 以服务器模式运行，则该字段用于确定是否需要在服务器上存储该作业的输出。

如果 `failOnError` 字段设置为 `True`，则所有报告的错误、断言和异常都会导致作业失败，即使结果代码是 `ProcessJobResult_Success`。

`setplatformidentifier` 和 `getplatformidentifier`方法用于设置和获取职位描述的平台标识符，如'pc' 或 'android'。这是 `PlatformInfo` 结构中的平台标识符。

### JobParameterMap

`JobParameterMap`是一个映射数据结构，用于保存为 `ProcessJob`请求而传入作业的参数。这些参数可选择在生成器的 `CreateJobs` 功能中设置，以便传递给 `ProcessJobFunction` 。参数值（键和值）是任意的，如何使用由资产创建器决定。

#### 示例: JobParameterMap

```python
jobParameterMap = {1 : "MyValue", 2 : "Another Value"}
```

### JobDependency

`azlmbr.asset.builder.JobDependency` 是一个包含作业相关性信息的类，生成器会将该信息发送给资产处理器。

| 字段 | 类型 | 说明 |
| - | - | - |
| sourceFile | `azlmbr.asset.builder.SourceFileDependency` | 资产生成器发送给资产处理器的源文件依赖性信息。 |
| jobKey | String | 从属工作的工作密钥。 |
| platformIdentifier | String | 从属任务的平台标识符。 |
| type | `azlmbr.asset.builder.JobDependency Type` | `JobDependency`类型 (顺序或指纹)。 |


#### JobDependency 类型

`azlmbr.asset.builder.JobDependency Type` 指定 `azlmbr.asset.builder.JobDependency` 类型，该类型决定何时处理作业依赖关系。

| 类型 | 说明 |
| - | - |
| `azlmbr.asset.builder.JobDependency_Fingerprint` | 当所依赖的作业的指纹发生变化时，资产处理器应处理从属作业。 |
| `azlmbr.asset.builder.JobDependency_Order` | 从属作业只能在资产处理器处理完其所依赖的作业后运行。 |
| `azlmbr.asset.builder.JobDependency_OrderOnce` | 从属作业只能在资产处理器处理完其所依赖的作业后运行，并且只能在资产处理器从未处理过从属作业的情况下运行。 |

## 示例: CreateJobs

下面的示例演示了当**资产处理器**检测到扫描目录中带有已注册模式的新源资产或已更改的源资产时，资产生成器如何创建任务。

```python
# Creates a single job to compile for each platform
def on_create_jobs(args):
    # get the request from the 'args'
    request = args[0]

    # Create job descriptor for each platform
    jobDescriptorList = []
    for platformInfo in request.enabledPlatforms:
        jobDesc = azlmbr.asset.builder.JobDescriptor()
        jobDesc.jobKey = 'My New Asset Job'
        jobDesc.priority = 1
        jobDesc.set_platform_identifier(platformInfo.identifier)
        jobDescriptorList.append(jobDesc)

    response = azlmbr.asset.builder.CreateJobsResponse()
    response.result = azlmbr.asset.builder.CreateJobsResponse_ResultSuccess
    response.createJobOutputs = jobDescriptorList
    return response
```
