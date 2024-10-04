---
linktitle: 日志文件
title: Open 3D Engine 日志文件
description: 了解在 Open 3D Engine (O3DE) 中查找常见日志文件的位置。
---

在以下位置查找这些常见的 **Open 3D Engine (O3DE)** 日志文件：

| 日志类型                                          | 位置                                                                                                              |
|-----------------------------------------------|-----------------------------------------------------------------------------------------------------------------|
| **Asset Processor** 作业日志                      | `<PROJECT_ROOT>/user/log/JobLogs`                                                                               |
| **CMake** 构建日志                                | `<BUILD_DIR>/CMakeFiles/CMakeOutput.log` <br> `<BUILD_DIR>/CMakeFiles/CMakeProjectBuildError.log`               |
| **O3DE Editor** 日志和转储文件 <br> （也被其它O3DE应用程序使用） | 当设置项目路径时: `<PROJECT_ROOT>/user/log` <br> 当未设置项目路径时: `<ENGINE_ROOT>/user/log` |
| **Project Manager** 日志                        | `<USER>/.o3de/Logs/O3DE.log`                                                                                    |
| **DirectX12 Device Removed Reports** 日志     | `<USER>/.o3de/Logs/DRED/DRED_<TIMESTAMP>.log`                                                                   |

{{< note >}}
可使用 TraceViewer 脚本读取扩展名为`.azel`的事件日志： `<ENGINE_ROOT>/Tools/EventLogTools/TraceViewer.py`.
{{< /note >}}
