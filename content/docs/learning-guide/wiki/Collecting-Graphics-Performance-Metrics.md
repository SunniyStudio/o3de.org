---
title: "Collecting-Graphics-Performance-Metrics"
description: ""
toc: false
---

There are four basic performance metrics (in _microseconds_) that can be collected on behalf of Atom (The O3DE 3D Graphics Renderer):  
1. **Graphics Simulation Time**: Measures the time spent inside RPISystem::Simulate().
2. **Graphics Render Time**: Measures the time spent inside RPISystem::RenderTick().
3. **Engine Cpu Time**: Measures the time spent in between calls to RPISystemComponent::OnSystemTick(). The inverse of this time equates to the "Frames Per Second" for the current scene: FPS=1'000'000/`Engine Cpu Time`.  
4. **Frame Gpu Time**: Measures the time spent, per frame, in the GPU.

## How To Collect Performance Data
The type of data, how much data and when to collect the data is user controllable via the following CVARs:  
1. **r_metricsDataLogType**: Default value `statistical`. This CVAR is a string. If it starts with the letter 'a' or 'A', then the type of performance data will be "All Samples"(Verbose). This means that for each parameter a row of data will produced in the output file for each measured render frame. If data is captured for 10'000 frames, then 10'000 lines, per parameter, will be added to the output JSON file. If this string starts with a different letter then it will collect data in the compact "Statistical" format, which means that regardless of the number of frames only a single line of data will be produced per parameter. Each line of data will be a statistical summary over all the frames: min, max, avg, stdev and number of samples.
2. **r_metricsFrameCountPerCaptureBatch**= Default value `1200` frames. Number of frames per batch. Data is captured in batches. In this CVAR the user defines how many frames will be measured per batch. In "statistical" mode each batch will be represented as single line of statistical summary per parameter.
3. **r_metricsWaitTimePerCaptureBatch**: Default value `0` seconds. How many seconds to wait before capturing data on each batch.
4. **r_metricsNumberOfCaptureBatches**: Default value `0` batches (OFF, no data is collected). When this CVAR is greater than 0, the performance data collection will start for this amount of batches. Once this counter reaches back to 0 then the performance collection stops and a file named `<project>/user/Performance_Graphics_<Date>.json` will be available with the performance data.  
5. **r_metricsQuitUponCompletion**: Default value `false`. If set to `true`, the application will quit when Number Of Capture Batches reaches 0.  
6. **r_metricsMeasureGpuTime**: Default value `false`. When enabled, the metric `Frame Gpu Time` is collected. Use judiciously, as this metric, when enabled, can affect performance. For example, if a scene is running at 300fps, the performance may reduce to ~290fps .

Here is an example of the content of the output JSON file:  
```json
[
{"name":"Graphics Simulation Time","cat":"Graphics-Win64-vulkan","ph":"X","ts":1667948193877268,"pid":56808,"tid":55956,"args":    {"avg":122.34280000000005,"min":99.0,"max":474.0,"sampleCount":2500,"units":"us","variance":143.1585515806325,"stdev":11.964888281159691,"mostRecentSampleValue":109.0},"dur":122},
{"name":"Graphics Render Time","cat":"Graphics-Win64-vulkan","ph":"X","ts":1667948193877420,"pid":56808,"tid":55956,"args": {"avg":2964.004799999998,"min":2605.0,"max":5060.0,"sampleCount":2500,"units":"us","variance":36986.33691172465,"stdev":192.31832183056468,"mostRecentSampleValue":3004.0},"dur":2964},
{"name":"Engine Cpu Time","cat":"Graphics-Win64-vulkan","ph":"X","ts":1667948193983164,"pid":56808,"tid":55956,"args": {"avg":3425.472589035615,"min":2939.0,"max":110194.0,"sampleCount":2499,"units":"us","variance":4618503.889060195,"stdev":2149.070470938586,"mostRecentSampleValue":3313.0},"dur":3425},
{"name":"Frame Gpu Time","cat":"Graphics-Win64-vulkan","ph":"X","ts":1667948193983204,"pid":56808,"tid":55956,"args": {"avg":2992.4170673076865,"min":2823.0,"max":4775.0,"sampleCount":2496,"units":"us","variance":98729.87688694714,"stdev":314.21310743975516,"mostRecentSampleValue":2841.0},"dur":2992}
]
```
The `"dur"` property is the average value of the parameter in microseconds across all frames specified in `"args"."sampleCount"`. In the example above the number of frames was 2500.  

## Some examples:
### Example 1:  
Let's collect performance data on the AutomatedTesting project using the level named `DefaultLevel`:
In this example we'll wait 30 seconds, capture statistical data for only 1 batch of 2500 frames.
```
> D:
> cd GIT\o3de\build\bin\profile
> .\AutomatedTesting.GameLauncher.exe --r_metricsWaitTimePerCaptureBatch=30 --r_metricsFrameCountPerCaptureBatch=2500 --r_metricsNumberOfCaptureBatches=1 +LoadLevel=DefaultLevel
```
When completed you should see the following file with the results:
`D:\GIT\o3de\AutomatedTesting\user\Performance_Graphics_<date>.json`  

### Example 2:
Let's collect performance data on the Loft project using the level named `Interior_03.prefab`:
In this example we'll wait 30 seconds, capture statistical data for only 1 batch of 2500 frames, and quit when done.
```
> D:
> cd GIT\LoftSample\build\bin\profile
> .\LoftSample.GameLauncher.exe --r_metricsWaitTimePerCaptureBatch=30 --r_metricsFrameCountPerCaptureBatch=2500 --r_metricsNumberOfCaptureBatches=1 --r_metricsQuitUponCompletion=true +LoadLevel=D:\GIT\LoftSample\Prpject\Levels\ArchVis\Loft\Interior_03.prefab
```
When completed you should see the following file with the results:
`D:\GIT\LoftSample\user\Performance_Graphics_<date>.json`  

