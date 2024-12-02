---
title: "测试"
date: 2021-03-02T00:23:56-05:00
weight: 1300
---

测试自动化是编写小代码片段（测试）以验证您创建的 Open 3D Engine 扩展和工具的所需功能的过程。

{{< important >}}
O3DE 项目的贡献者需要按照 [Open 3D Foundation Testing 特别兴趣小组](https://github.com/o3de/sig-testing) 制定的标准为其功能编写测试。如果您计划参与，请确保您熟悉项目测试标准和框架。
{{< /important >}} 

## 测试运行程序

O3DE 使用标准测试运行程序来收集、执行和收集结果。官方支持的测试运行程序包括：

* [GoogleTest](https://github.com/google/googletest)
* [GoogleBenchmark](https://github.com/google/benchmark)
* [PyTest](https://docs.pytest.org)

## 测试工具包

Open 3D Engine （O3DE）** 提供了多个工具包，使编写测试更快、更安全、更一致。在编写测试之前，请花点时间熟悉这些工具。

### AzTest

[AzTest](/docs/api/frameworks/aztest/)是抽象、函数和包装器的集合，用于更轻松地编写 C++ 测试。

### EditorPythonTestTools

EditorPythonTestTools 是一组专注于访问 Editor 功能的测试工具。每当您想要自动执行 Editor 中发生的任务时，都应该使用这些工具。

### LyTestTools

[LyTestTools](/docs/user-guide/testing/lytesttools/) 正在测试生产力工具，用于跨不同环境编写和调试测试。这包括（但不限于）环境操作、创建/收集图像和日志以及启动项目启动器。
