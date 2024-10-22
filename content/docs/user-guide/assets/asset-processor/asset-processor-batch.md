---
linkTitle: 批处理
title: Asset Processor Batch
description: Asset Processor Batch 应用程序可用于在构建服务器上编译项目的所有资产。
weight: 200
toc: true
---

您可以使用 **Asset Processor Batch** 作为 **Open 3D Engine (O3DE)** 自动构建系统的一部分。`AssetProcessorBatch.exe`应用程序为当前项目和启用的平台编译所有资产。如果编译成功且无错误，则以`0`代码退出。

`AssetProcessorBatch.exe` 接受以下命令行参数，用于覆盖默认行为：

* `--platforms=<comma separated list>`
* `--project-path=<name of game folder>`

示例:

```cmd
AssetProcessorBatch.exe --platforms=pc,ios --project-path=MyTestProjectPath
```
