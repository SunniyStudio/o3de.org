---
linkTitle: 配置
title: Asset Processor 配置
description: 使用 AssetProcessorPlatformConfig.setreg 配置Open 3D Engine (O3DE) 的资产处理器。
weight: 300
toc: true
---

## 设置注册表

**Asset Processor** 用于项目的许多内容处理规则都由设置注册表值控制。您可以在 [`Registry/AssetProcessorPlatformConfig.setreg`](https://github.com/o3de/o3de/blob/development/Registry/AssetProcessorPlatformConfig.setreg)中查看默认值。该文件注释详尽，并包含大量示例，您可以使用这些示例自定义项目资产管道的配置。

一些附加设置还存在于[Registry/bootstrap.setreg](https://github.com/o3de/o3de/blob/development/Registry/bootstrap.setreg)。

{{< todo issue="https://github.com/o3de/o3de.org/issues/706" >}}
要添加特定于项目或特定于用户的覆盖，可以使用与其他设置注册表覆盖相同的模式。链接的问题涵盖了对设置注册表覆盖功能的记录。
{{</todo>}}

### 处理多个平台的资产

要处理非主机平台的资产，可以在设置注册表路径`Amazon/AssetProcessor/Settings/Platforms`下启用其他平台。请注意，这只是为项目启用平台的一个步骤。有关如何设置和部署到其他平台，请参阅文档中的 [平台](/docs/user-guide/platforms/)  部分。

### 运行资产处理器的多个实例

要运行 Asset Processor 的多个实例，必须通过更改 `Amazon/AzCore/Bootstrap/remote_port` 设置（默认在 `bootstrap.setreg` 中），将每个实例配置为使用不同的服务器端口。 这对同时打开多个项目非常有用。

例如
```json
{
    "Amazon": {
        "AzCore": {
            "Bootstrap": {
                "remote_port": 12345,
            }
        }
    }
}
```

### 资产任务管理

默认情况下，资产处理器会为主机上的每个逻辑内核创建一个工作任务。如果要控制 Worker 作业的数量，可调整设置 `Amazon/AssetProcessor/Settings/Jobs/maxJobs`。如果值为零，资产处理器将为机器上的每个逻辑内核创建一个作业。在调试时，如果希望将调试器附加到处理特定资产的运行构建程序上，1 的值会很有用。设置一个低于逻辑内核数的值，可以减少资产处理所消耗的资源，但代价是资产处理时间会更长。

### 资产缓存服务器

资产缓存服务器的地址可以通过设置路径`Amazon/AssetProcessor/Settings/Server/CacheServerAddress`来设置。在大多数情况下，这将是一个网络共享，Windows 的示例如下`T:\\AssetServerCache`。

{{< note >}}
此功能正在开发中。如果您对资产缓存服务器有任何疑问，请通过 [O3DE Discord](https://discord.com/invite/o3de) 联系我们。
{{< /note >}}

### 元数据文件

资产的元数据文件在设置路径`Amazon/AssetProcessor/Settings/MetaDataTypes`下进行管理，元数据扩展名与相关文件扩展名之间有一个格式映射。例如，FBX 文件使用扩展名为`.fbx.assetinfo`的元数据文件类型，在此设置路径下定义为 `"fbx.assetinfo": "fbx"`。添加新资产类型时，如果要在元数据文件中跟踪设置，请确保将其添加到此设置路径中。这样，当元数据文件发生变化时，资产处理器将重新运行源资产的处理任务。

### 扫描文件夹

扫描文件夹可以作为注册表设置添加。大多数情况下，你会使用 Gems 或项目的资产文件夹，它们会被自动添加为扫描文件夹。不过，如果你需要添加一个新的扫描文件夹来跟踪源资产，可以在注册表设置路径  `Amazon/AssetProcessor/Settings/`下添加，模式为：

```json
"ScanFolder <scan folder name>": {
  "watch": "<scan folder path>",
  "display": "<Display name for the scan folder>",
  "recursive": <0|1>, // 1 scans all subdirectories for assets recursively, 0 only includes assets in this folder and not subfolders.
  "order": <any number> // The order this scan folder is handled compared to other scan folders.
}
```

当两个资产到扫描文件夹根目录的相对路径完全相同时，就会使用扫描文件夹顺序。在这种情况下，扫描文件夹中优先级较高的源资产将被优先使用。

### 排除文件夹

排除系统用于阻止与资产处理模式匹配的文件。这些模式存在于注册表路径 `Amazon/AssetProcessor/Settings/Exclude`下，模式如下：

```json
"Exclude <exclusion title>": {
  "pattern": "<regex of files to exclude>"
}
```

### 资产处理标签

每个平台都有一个标签集，应用于注册路径 `Amazon/AssetProcessor/Settings/`下。其他系统会检查这些标签以进行处理，但这些标签本身不会直接触发任何活动。平台标签遵循以下模式：

```json
"Platform <platform name>": {
  "tags:" "<comma-separated list of tags>"
}
```

### 复制任务

复制任务用于将源资产作为产品资产直接复制到缓存中。有些资产不需要额外处理，因此通过这种方式设置复制任务比编写整个基于 C++ 的生成器来将该类型的内容作为资产提供给您的游戏更快。复制任务由注册表设置模式管理，该模式位于`Amazon/AssetProcessor/Settings/`下，使用以下模式：

```json
"RC <Copy job name>": {
  // 匹配类型可以是 glob 或模式，不要同时使用两种类型。模式适用于基于正则表达式的匹配，而 glob 则适用于只想匹配文件扩展名的情况。
  "glob": "*.<extension>",
  "pattern": "<regex pattern to match this copy job>", // 要么是glob ，要么是pattern，不能两者兼得
  "productAssetType": "<The UUID for the AZ::Data::AssetType for the product asset>",
  "params": "<copy|skip>", // "copy"用于复制任务，"skip"用于跳过处理。"skip"主要用于不需要特定资产类型的平台。
  "<platform tag>": "<copy|skip>", // 复制或跳过符合此标记的平台。例如，如果不想在带有服务器标记的平台上运行复制任务，这一行应为`"server": "skip"`。有关详情，请参阅前面的平台标记部分。
}
```
