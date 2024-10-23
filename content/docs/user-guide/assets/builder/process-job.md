---
linkTitle: ProcessJob
title: Python Asset Builder ProcessJob
description: Python Asset Builder 中的 ProcessJob 可为 Asset Processor 生成作业信息并生成产品资产。 
weight: 300
toc: true
---

当**资产处理器**有任务需要**资产生成器**处理时，它就会调用已注册的回调。回调会执行以下操作：

* 处理源资产。
* 在临时目录中生成至少一个产品资产。
* 通过 `ProcessJobResponse` 实例中的 `JobProduct` 条目注册产品资产。
* 在 `ProcessJobResponse` 中返回成功值。

## ProcessJobRequest

`azlmbr.asset.builder.ProcessJobRequest` 是处理源资产的`OnProcessJobRequest`函数的输入。它包含处理任务所需的输入数据。

| 字段 | 类型 | 说明 |
| - | - | - |
| `sourceFile` | String | 源资产名称。 |
| `watchFolder` | String | 源资产的扫描目录。 |
| `fullPath` | String | 源资产路径。 |
| `builderGuid` | `azlmbr.math.Uuid` | Asset Builder ID. |
| `jobDescription` | `azlmbr.asset.builder.JobDescriptor` | 在 `OnCreateJobs` 中创建的该作业的作业描述符。 |
| `tempDirPath` | String | 资产创建器为该任务请求创建任务输出时使用的临时目录。 |
| `platformInfo` | `azlmbr.asset.builder.PlatformInfo` | 有关该工作平台的信息。 |
| `sourceFileDependencyList` | List[`azlmbr.asset.builder.SourceFileDependency`] | 源资产依赖性信息。 |
| `sourceFileUUID` | `azlmbr.math.Uuid` | 源资产的通用唯一标识符 (UUID)。 |
| `jobId` | Number | 此作业的作业 ID，也是 `JobCancelListener` 的地址。 |

`jobDescription`字段包含`OnCreateJobs`步骤为该作业创建的作业参数。

`tempDirPath`字段是资产创建器用来写出此任务的中间和最终产品资产的临时目录的路径。输出的产品资产存储在临时目录中，与**资产缓存**和`.pak`文件中最终产品资产的位置相对应。

`sourceFileUUID`字段既是源资产的唯一 ID，也是该任务输出的所有产品资产的资产 ID 的第一部分。产品资产由 `JobProduct` 结构中使用的 `subId` 区分。

## ProcessJobResponse 

`azlmbr.asset.builder.ProcessJobResponse`是 `OnProcessJobRequest`回调返回的类，用于描述作业结果。`ProcessJobResponse` 包含的作业数据在`outputProducts`字段中显示了作业的输出、结果代码和要重新处理的时间表源。

| 字段 | 类型 | 说明 |
| - | - | - |
| `resultCode` | `azlmbr.asset.builder.ProcessJobResponse Result Code` | 处理任务的结果。 |
| `outputProducts` | List[`azlmbr.asset.builder.JobProduct`] | 工作产品资产清单。 |
| `requiresSubIdGeneration` | Boolean | 确定遗留产品资产是否需要生成子 ID。 |
| `sourcesToReprocess` | List[String] | 触发重建的绝对源资产路径。 |

`resultCode`字段默认为`azlmbr.asset.builder.ProcessJobResponse_Failed`。空的 `ProcessJobResponse` 表示任务失败。

`outputProducts` 字段表示哪些产品资产需要复制到资产缓存。`outputProducts` 列表为空表示任务失败。

由于在此任务中执行的工作，`sourcesToReprocess`字段会触发源资产（通过绝对路径）的重建。要重新处理这些源资产，构建程序会更新处理这些资产的资产构建程序的 `CreateJobs` 中的指纹，如更改源依赖关系。

### ProcessJobResponse 结果代码 

`azlmbr.asset.builder.ProcessJobResponse Result Code` 有'Success'、'Failed'和三种具体的失败情况。

| 结果代码 | 说明 |
| - | - |
| `azlmbr.asset.builder.ProcessJobResponse_Success` | 处理任务已成功完成。 |
| `azlmbr.asset.builder.ProcessJobResponse_Failed` | 处理任务未生成所有预期的产品资产和数据。 |
| `azlmbr.asset.builder.ProcessJobResponse_Crashed` | 工具或内部应用程序接口在处理任务过程中出现异常。 |
| `azlmbr.asset.builder.ProcessJobResponse_Cancelled` | 处理任务在处理过程中被取消。 |
| `azlmbr.asset.builder.ProcessJobResponse_NetworkIssue` | 处理任务无法连接远程服务或资源。 |

### JobProduct 

成功的流程作业会在 `outputProducts` 字段中返回一个或多个 `azlmbr.asset.builder.JobProduct` 条目。

`productSubID`字段是该流程任务创建的每个产品文件的稳定且唯一的产品标识符。它可以是任何无符号的 32 位整数，用于区分来自同一源资产的不同输出产品资产。如果源资产的流程任务只生成一个产品资产，则生成器可以使用 `0`。

`productAssetType` 字段映射到 C++ 的`AZ::Data::AssetData`类型 ID。

从 Python 确定资产类型 ID 的一种方法是使用资产的显示名称调用 `AssetCatalogRequestBus` ：

```python
assetType = azlmbr.asset.AssetCatalogRequestBus(azlmbr.bus.Broadcast, 'GetAssetTypeByDisplayName', "Font")
print(f'Asset type {assetType}')
```

`dependenciesHandled`向资产处理器表明，资产创建器已输出此作业产品资产的所有可能依赖关系。如果没有输出产品资产，该值也可以为 `True`。只有当资产创建器输出了其依赖项或输出产品没有依赖项时，才应将此设置为  `True`。设置为`False`时，资产处理器会发出未处理依赖关系的警告。

`JobProduct` 构造函数接收产品资产名称（相对于源资产路径）、资产类型（UUID）和产品子 ID 编号。

| 字段 | 类型 | 说明 |
| - | - | - |
| productFileName | String | 相对或绝对产品资产路径。 |
| productAssetType | `azlmbr.math.Uuid` | 产品资产添加到**资产目录**的资产类型 ID。 |
| productSubID | Number | 稳定、唯一的产品标识符。 |
| productDependencies | List[`ProductDependency`] | 该源资产的产品资产依赖关系。 |
| pathDependencies | Set{`ProductPathDependency`} | 通过源资产的相对路径指定产品依赖关系。 |
| dependenciesHandled | Boolean | 表示资产创建器是否已输出所有可能的依赖关系。 |
| JobProduct | Constructor | Inputs: `productFileName:str`, `productAssetType:azlmbr.math.Uuid`, `productSubID:number` |

### ProductDependency 

`azlmbr.asset.builder.ProductDependency` 包含产品依赖性信息，资产创建器会将该信息发送给资产处理器，以表明产品资产在加载或打包时依赖于另一个产品资产。

| 字段 | 类型 | 说明 |
| - | - | - |
| `dependencyId` | `azlmbr.math.Uuid` | 该产品资产依赖关系的资产 ID。 |

### ProductPathDependency 

`azlmbr.asset.builder.ProductPathDependency` 表示 Asset Builder 检测到的产品对另一产品资产的依赖信息（相对于源资产路径）。如果源资产 ID 可以确定，我们建议您使用 `productDependencies` 代替以资产 ID 表示产品的依赖信息。最好尽可能依赖产品资产，以避免引入意外依赖。

`dependencyType` 字段表示路径指向源资产还是产品资产。

| 字段 | 类型 | 说明 |
| - | - | - |
| `dependencyPath` | String | Relative path to the asset dependency. |
| `dependencyType` | `azlmbr.asset.builder.ProductPathDependency Type` | Indicates if the dependency path points to a source asset or a product asset. |

### ProductPathDependency 类型 

`azlmbr.asset.builder.ProductPathDependency Type` 表示如何使用 `ProductPathDependency` 中的依赖路径。对源资产的依赖会转换为对所有已生成产品资产的依赖。最好尽可能依赖产品资产，以避免引入意外依赖。
 
| 类型 | 说明 |
| - | - |
| `azlmbr.asset.builder.ProductPathDependency_ProductFile` | 如果源资产依赖于另一个产品资产文件，则值应为 `SourceFile`。 |
| `azlmbr.asset.builder.ProductPathDependency_SourceFile` | 如果源资产依赖于另一个源资产，则值应为 `ProductFile`。 |

## 示例: ProcessJob

下面的示例演示了当资产处理器检测到扫描目录中带有已注册模式的新源资产或已更改的源资产时，资产生成器如何处理作业。

```python
# Using the incoming 'request' find the type of job via 'jobKey' to determine what to do
def on_process_job(args):
    try:
        # Get request information
        request = args[0]

        # Prepare output folder
        basePath, _ = os.path.split(request.sourceFile)
        outputPath = os.path.join(request.tempDirPath, basePath)
        os.makedirs(outputPath)

        # Write out a simple file
        productFileNameOnly = 'myfile.txt'
        filename = os.path.join(outputPath, productFileNameOnly)
        file = open(filename, "w+")
        file.write('some data')
        file.close()

        # Prepare output entry data
        productOutputs = []
        basePath, sceneFile = os.path.split(request.sourceFile)
        assetProductName = os.path.join(basePath, productFileNameOnly)
        outputFilename = os.path.join(request.tempDirPath, assetinfoFilename)

        # Create job product entry
        assetType = azlmbr.math.Uuid_CreateString('{F67CC648-EA51-464C-9F5E-4A9CE41A7F86}', 0)
        product = azlmbr.asset.builder.JobProduct(assetProductName, assetType, 0)
        product.dependenciesHandled = True
        productOutputs.append(product)

        # Fill out response object
        response = azlmbr.asset.builder.ProcessJobResponse()
        response.outputProducts = productOutputs
        response.resultCode = azlmbr.asset.builder.ProcessJobResponse_Success
        return response
    except:
        # An exception should record a proper failure
        response = azlmbr.asset.builder.ProcessJobResponse()
        response.resultCode = azlmbr.asset.builder.ProcessJobResponse_Crashed
        return response
```
