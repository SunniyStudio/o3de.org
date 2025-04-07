---
title: "收集图形性能指标"
description: ""
toc: false
---

可以代表 Atom （O3DE 3D Graphics Renderer） 收集四个基本性能指标（以 _μseconds_ 为单位）： 
1. **图形模拟时间**：测量在 RPISystem：：Simulate（） 中花费的时间。
2. **图形渲染时间**：测量在 RPISystem：：RenderTick（） 中花费的时间。
3. **引擎 CPU 时间**：测量调用 RPISystemComponent：：OnSystemTick（） 之间所花费的时间。此时间的倒数等于当前场景的“每秒帧数”：FPS=1'000'000/`Engine Cpu Time`。 
4. **帧 GPU 时间**：测量 GPU 中每帧花费的时间。

## 如何收集性能数据
用户可以通过以下 CVAR 控制数据类型、数据量和收集时间：  
1. **r_metricsDataLogType**: 默认值 `statistical`。此 CVAR 是一个字符串。如果它以字母 'a' 或 'A' 开头，则性能数据类型将为 “All Samples（详细）”。这意味着对于每个参数，输出文件中将为每个测量的渲染帧生成一行数据。如果捕获了 10'000 帧的数据，则每个参数的 10'000 行将添加到输出 JSON 文件中。如果此字符串以不同的字母开头，则它将以紧凑的 “Statistical” 格式收集数据，这意味着无论帧数如何，每个参数只会生成一行数据。每行数据将是所有帧的统计摘要：最小值、最大值、平均值、标准差和样本数。
2. **r_metricsFrameCountPerCaptureBatch**= 默认值`1200` 帧。每批的帧数。数据是分批捕获的。在此 CVAR 中，用户定义每批将测量多少帧。在 “statistical” 模式下，每个批次将表示为每个参数的单行统计摘要。
3. **r_metricsWaitTimePerCaptureBatch**: 默认值 `0` 秒。在捕获每个批次的数据之前要等待多少秒。
4. **r_metricsNumberOfCaptureBatches**: 默认值 `0` 批次（OFF，不收集任何数据）。当此 CVAR 大于 0 时，将开始此数量的批次的性能数据收集。一旦此计数器回到 0，性能收集就会停止，并且名为`<project>/user/Performance_Graphics_<Date>.json`的文件将与性能数据一起提供。  
5. **r_metricsQuitUponCompletion**: 默认值 `false`。如果设置为 `true`，则应用程序将在 Number Of Capture Batches 达到 0 时退出。 
6. **r_metricsMeasureGpuTime**: 默认值 `false`。启用后，将收集量度`Frame Gpu Time`。请谨慎使用，因为启用此指标后可能会影响性能。例如，如果场景以 300fps 的速度运行，则性能可能会降低到 ~290fps 。

以下是输出 JSON 文件内容的示例：
```json
[
{"name":"Graphics Simulation Time","cat":"Graphics-Win64-vulkan","ph":"X","ts":1667948193877268,"pid":56808,"tid":55956,"args":    {"avg":122.34280000000005,"min":99.0,"max":474.0,"sampleCount":2500,"units":"us","variance":143.1585515806325,"stdev":11.964888281159691,"mostRecentSampleValue":109.0},"dur":122},
{"name":"Graphics Render Time","cat":"Graphics-Win64-vulkan","ph":"X","ts":1667948193877420,"pid":56808,"tid":55956,"args": {"avg":2964.004799999998,"min":2605.0,"max":5060.0,"sampleCount":2500,"units":"us","variance":36986.33691172465,"stdev":192.31832183056468,"mostRecentSampleValue":3004.0},"dur":2964},
{"name":"Engine Cpu Time","cat":"Graphics-Win64-vulkan","ph":"X","ts":1667948193983164,"pid":56808,"tid":55956,"args": {"avg":3425.472589035615,"min":2939.0,"max":110194.0,"sampleCount":2499,"units":"us","variance":4618503.889060195,"stdev":2149.070470938586,"mostRecentSampleValue":3313.0},"dur":3425},
{"name":"Frame Gpu Time","cat":"Graphics-Win64-vulkan","ph":"X","ts":1667948193983204,"pid":56808,"tid":55956,"args": {"avg":2992.4170673076865,"min":2823.0,"max":4775.0,"sampleCount":2496,"units":"us","variance":98729.87688694714,"stdev":314.21310743975516,"mostRecentSampleValue":2841.0},"dur":2992}
]
```
`"dur"`属性是`"args"."sampleCount"`中指定的所有帧中参数的平均值（以微秒为单位）。在上面的示例中，帧数为 2500。

## 一些例子：
### 示例 1： 
让我们使用名为 `DefaultLevel` 的关卡来收集 AutomatedTesting 项目的性能数据：
在此示例中，我们将等待 30 秒，仅捕获 1 批 2500 帧的统计数据。
```
> D:
> cd GIT\o3de\build\bin\profile
> .\AutomatedTesting.GameLauncher.exe --r_metricsWaitTimePerCaptureBatch=30 --r_metricsFrameCountPerCaptureBatch=2500 --r_metricsNumberOfCaptureBatches=1 +LoadLevel=DefaultLevel
```
完成后，您应该会看到以下包含结果的文件：
`D:\GIT\o3de\AutomatedTesting\user\Performance_Graphics_<date>.json`  

### 示例 2：
让我们使用名为 `Interior_03.prefab` 的关卡来收集 Loft 项目的性能数据：
在此示例中，我们将等待 30 秒，仅捕获 1 批 2500 帧的统计数据，并在完成后退出。
```
> D:
> cd GIT\LoftSample\build\bin\profile
> .\LoftSample.GameLauncher.exe --r_metricsWaitTimePerCaptureBatch=30 --r_metricsFrameCountPerCaptureBatch=2500 --r_metricsNumberOfCaptureBatches=1 --r_metricsQuitUponCompletion=true +LoadLevel=D:\GIT\LoftSample\Prpject\Levels\ArchVis\Loft\Interior_03.prefab
```
完成后，您应该会看到以下包含结果的文件：
`D:\GIT\LoftSample\user\Performance_Graphics_<date>.json`  

