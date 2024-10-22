---
linkTitle: 设置
title: Asset Processor 设置
description: 用于AssetProcessor 和 AssetProcessorBatch的命令行设置
weight: 900
toc: true
---

资产处理器和资产处理器批处理暴露了许多命令行设置，可在手动启动时使用。

例如，如果要在 Windows 上手动启动资产处理器，但要延迟启动以附加调试器：
```
AssetProcessor --project-path="C:/o3de/o3de/AutomatedTesting" --engine-path="C:/o3de/o3de" --waitOnLaunch
```

## 常规设置
手动启动资产处理器或批量资产处理器时，可以设置以下常规属性：

| 设置                 | 说明                   | 默认值                           |
|--------------------|----------------------|-------------------------------|
| project-name       | 当前项目的名称。             | 与传递的 `project-path` 相关联的项目名称。 |
| project-cache-path | 项目缓存文件夹的完整路径。        | 与传递的 `project-path` 相关联的缓存路径。 |
| project-path       | 提供资产处理器应使用的项目路径（必填）。 |                               |
| engine-path        | 为引擎提供引擎路径。           | 与传递的 `project-path` 相关联的引擎路径。 |


## 高级设置
还可以使用以下命令行选项控制行为。 有关资产处理器配置设置的详情，另请参阅 [配置](configuration.md)。

| 设置                              | 说明                                                                      | 默认值                                                                  |
|---------------------------------|-------------------------------------------------------------------------|----------------------------------------------------------------------|
| acceptInput                     | 通过 ControlRequestHandler 启用外部控制消息传递，与自动测试一起使用。                          |                                                                      |
| additionalScanFolders           | 与 `dependencyScanPattern` 一起使用，可进一步过滤扫描。                                |                                                                      |
| debugOutput                     | 启用后，支持该功能的构建器将以产品资产的形式输出调试信息。主要用于场景文件。                                  |                                                                      |
| dependencyScanMaxIteration      | 用于限制运行依赖性扫描模式（dependencyScanPattern）时每行的递归搜索次数。                         | 800                                                                  |
| dependencyScanPattern (dsp)     | 扫描符合给定模式的资产，查找缺失的产品依赖关系。                                                |                                                                      |
| enableQueryLogging              | 启用数据库查询日志。                                                              |                                                                      |
| fileDependencyScanPattern (fsp) | 与 `dependencyScanPattern` 一起使用，可进一步过滤扫描。                                |                                                                      |
| help (h)                        | 显示所有可用命令及其说明。                                                           |                                                                      |
| sortJobsByDBSourceName          | 启用后，可按数据库源名称而不是作业 ID 对具有相同优先级和依赖关系的待处理作业进行排序。这对自动测试非常有用，每次都能以相同的顺序处理资产。 |                                                                      |
| truncatefingerprint             | 截断用于处理资产的指纹。如果你计划压缩产品资产以便在另一台机器上共享，这将非常有用，因为某些压缩格式（如 zip）会截断文件模式时间戳。    |                                                                      |
| waitOnLaunch                    | 在初始化过程中短暂暂停资产处理器。 为真时等待 20 秒。如果要附加调试器，此功能很有用。                           | false                                                                |
| warningLevel                    | 配置 AssetProcessor 的错误和警告报告级别。默认为 0，1 表示致命错误，2 表示致命错误和警告。                | 默认值为 0。                                                              |
| zeroAnalysisMode                | 在检查处理源资产时使用文件修改时间。                                                      | AssetProcessor默认为 true，可在资产处理器用户界面中更改。Asset Processor Batch默认为false。 | 

请注意，AssetProcessor 和 Asset Processor Batch 可能有不同的默认值。
