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

| Field | Type | Description |
| - | - | - |
| `sourceFileDependencyPath` | String | Path (relative to the assets directory or absolute) to the dependency. |
| `sourceFileDependencyUUID` | `azlmbr.math.Uuid` | UUID (without the sub ID) of the dependency. |
| `sourceDependencyType` | `azlmbr.asset.builder.SourceFileDependency Type` | Absolute dependency or wildcard match (`azlmbr.asset.builder.SourceFileDependency_Absolute` is the default). |

{{< note >}}
The Asset Builder does not need to provide both the `sourceFileDependencyUUID` and the `sourceFileDependencyPath` info to **Asset Processor**. Either one is sufficient.
{{< /note >}}

### SourceFileDependency Type

`azlmbr.asset.builder.SourceFileDependency Type` specifies an absolute dependency or a wildcard match.

| Type | Description |
| - | - |
| `azlmbr.asset.builder.SourceFileDependency_Absolute` | An absolute source file dependency. |
| `azlmbr.asset.builder.SourceFileDependency_Wildcards` | Allow wildcard matches. |

## JobDescriptor

`azlmbr.asset.builder.JobDescriptor` is a class used by the Asset Builder to store job-related information.

| Field | Type | Description |
| - | - | - |
| `jobParameters` | `JobParameterMap` | Asset Builder specific parameters to pass to the `ProcessJobRequest`. |
| `additionalFingerprintInfo` | String | Additional info that should be taken into account when fingerprinting this job. |
| `jobKey` | String | Job specific key, for example, TIFF Job. |
| `priority` | Integer | Priority value for the jobs within the job queue. |
| `checkExclusiveLock` | Boolean | Attempt to get an exclusive lock on the source asset file before processing the job when `True`. |
| `checkServer` | Boolean | Check the server for the outputs of this job before processing the job. |
| `jobDependencyList` | List[`azlmbr.asset.builder.JobDependency`] | Required for jobs that want to declare job dependencies on other jobs. |
| `failOnError` | Boolean | Errors, asserts, and exceptions automatically cause the job to fail when `True`. |
| `set_platform_identifier(platformIdentifier:string)` | Method | Sets the identifier for the build platform. |
| `get_platform_identifier()` | Method | Returns the identifier for the build platform. |

The `priority` field is the value for the jobs within the job queue. A priority value less than `0` means the job's priority is not considered. A priority value of `0` or greater prioritizes the job by value. The higher the value, the higher priority.

{{< note >}}
Priorities for critical and non-critical jobs are set separately.
{{< /note >}}

The `checkExclusiveLock` field is a flag to determine whether Asset Processor needs to check the source asset file for exclusive lock before processing the job. Asset Processor will lock and unlock the source asset file to ensure it is not opened by another process. This prevents premature processing of some source asset files that are opened for writing, but have zero bytes for longer than the modification threshold. This will time out if the Asset Processor cannot get an exclusive lock.

The `checkServer` field determines whether Asset Processor needs to check the server for the outputs of this job before starting to process the job locally. If Asset Processor is running in server mode, then this is used to determine whether it needs to store the outputs of this job on the server.

If the `failOnError` field is set to `True`, then all reported errors, asserts, and exceptions cause the job to fail, even if the result code is `ProcessJobResult_Success`.

The `setplatformidentifier` and `getplatformidentifier` methods set and retrieve the platform identifier such as 'pc' or 'android' for the job description. It is the identifier of the platform from the `PlatformInfo` struct.

### JobParameterMap

The `JobParameterMap` is a map data structure that holds parameters that are passed into a job for `ProcessJob` requests. These parameters can optionally be set during the `CreateJobs` function of the builder so that they are passed along to the `ProcessJobFunction`. The values (key and value) are arbitrary and it is up to the Asset Builder how to use them.

#### Example: JobParameterMap

```python
jobParameterMap = {1 : "MyValue", 2 : "Another Value"}
```

### JobDependency

`azlmbr.asset.builder.JobDependency` is a class containing job dependency information that the builder sends to the Asset Processor.

| Field | Type | Description |
| - | - | - |
| sourceFile | `azlmbr.asset.builder.SourceFileDependency` | Source file dependency information that the Asset Builder sends to Asset Processor. |
| jobKey | String | Job key of the dependent job. |
| platformIdentifier | String | Platform identifier of the dependent job. |
| type | `azlmbr.asset.builder.JobDependency Type` | Type of `JobDependency` (order or fingerprint). |


#### JobDependency Type

`azlmbr.asset.builder.JobDependency Type` specifies the `azlmbr.asset.builder.JobDependency` type that determines when the job dependency should be processed.

| Type | Description |
| - | - |
| `azlmbr.asset.builder.JobDependency_Fingerprint` | The dependent job should get processed by Asset Processor when the fingerprint of the job it depends on changes. |
| `azlmbr.asset.builder.JobDependency_Order` | The dependent job should only run after the job it depends on is processed by Asset Processor. |
| `azlmbr.asset.builder.JobDependency_OrderOnce` | The dependent job should only run after the job it depends on is processed by Asset Processor and only if the dependencies have never been processed by Asset Processor. |

## Example: CreateJobs

The example below demonstrates how the Asset Builder might create jobs when **Asset Processor** detects a new or changed source asset with the registered pattern in a scan directory.

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
