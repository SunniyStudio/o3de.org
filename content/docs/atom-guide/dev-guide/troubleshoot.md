---
linkTitle: 故障排除
title: Atom 渲染器故障排除
description: 在 Atom Renderer（集成到 Open 3D Engine (O3DE) 中的图形引擎）中排除 GPU 问题。
toc: true
weight: 1000
---

本指南可帮助您在运行**Atom 渲染器**时识别和处理图形处理器（GPU）崩溃。

GPU 崩溃的一个关键指标是 “设备移除 ”故障，当驱动程序异常导致 GPU 处于非运行状态时，就会发生这种故障。您可以在[O3DE日志文件](/docs/user-guide/appendix/log-files)中找到这种故障。如果您遇到这种崩溃，请按照本页上的说明报告问题，并尝试几种技术来进一步调试问题。

## 报告问题

在报告问题之前，请检查是否有类似的 [现有问题](https://github.com/o3de/o3de/issues)。否则，请 [创建问题](https://github.com/o3de/o3de/issues/new/choose) 并提供以下信息：

- 重现崩溃的步骤。尽量减少信息量并使其具体化。

- GPU 供应商和驱动程序版本。例如，英伟达（NVIDIA）、AMD、英特尔（Intel）、苹果（Apple）等。

- 渲染硬件接口 (RHI)。例如，Vulkan、Direct3D 12 或 Metal。


## 测试其他 GPU 或 RHI 后端

如果您的机器有多个 GPU（例如多个独立 GPU 或集成 GPU）或多个 RHI 后端（例如 Vulkan 和 DirectX），请尝试测试不同的 GPU 或它们的不同组合。

要优先使用一个 GPU 而不是另一个，请在命令行界面 (CLI) 中输入 `-forceAdapter=<device>`。对于 `<device>`（设备名称），可以输入任何不区分大小写的部分匹配。

**示例**

```cmd
--forceAdapter="NVIDIA GeForce GTX 1080"
```

要使用非默认 RHI 后端，请在 CLI 中输入 `--rhi=<rhi>`，并将 `<rhi>` 替换为 `vulkan` 或 `dx12`。这仅适用于默认 RHI 后端为 `D3D12` 的 Windows 计算机。

**示例**

```cmd
--rhi=vulkan
```

{{< note >}}
如果不知道 GPU 的名称，可以查看命令窗口输出或 [O3DE 日志文件](/docs/user-guide/appendix/log-files)。Atom 启动时，RHI 会将所有 RHI 设备的名称打印到命令窗口。

例如，该输出列出了机器上的三个 RHI 设备。

```cmd
Initializing RHI...
    Enumerated physical device: Intel(R) UHD Graphics 630
    Enumerated physical device: NVIDIA GeForce RTX 2070 with Max-Q
Design
    Enumerated physical device: Microsoft Basic Render Driver
```
{{< /note >}}


## 启用驱动程序验证

您可以在 RHI 设备中启用验证，以输出在 RHI 级别生成的额外调试信息。要启用 RHI 级验证，请在 CLI 中输入 `--rhi-device-validation=`，并传递下列值之一。对于 Vulkan，在启用设备验证之前，请确保您已经为机器安装了 [Vulkan SDK](https://vulkan.lunarg.com/sdk/home)。

- `--rhi-device-validation="enable"`: 启用基本的驱动程序验证。
- `--rhi-device-validation="verbose"`: 启用基本驱动程序验证和附加报告。
- `--rhi-device-validation="gpu"`: 启用基于 GPU 的验证 (GBV)。这种验证模式运行速度较慢，但能捕捉到`enable` 和 `verbose`模式可能捕捉不到的问题。请注意，该模式可能会产生误报。


## DRED 日志（仅限 Windows DirectX）

在使用 DirectX12 后端时发生超时检测和恢复 (TDR) 崩溃时，[设备已删除扩展数据](https://microsoft.github.io/DirectX-Specs/d3d/DeviceRemovedExtendedData.html) (DRED) 面包屑
会被写入`.o3de/Logs/DRED/`中带有时间戳的日志文件。该日志包含导致崩溃的所有事件、绘制、障碍和派发，以及未能完成的命令。
在日志的 `===ERROR START===` 行之后和 `===ERROR END===` 行之前，可以看到可能导致设备移除的违规命令。

**示例**

在 Windows 日志查看器中显示的带有 DRED 面包屑的日志示例。

<img src="/images/atom-guide/dev-guide/dred_log_sample.png" alt="DRED Log Sample" style="max-width: 800px"/>

红线划定了包含错误命令的区域。根据崩溃时 GPU 上正在运行的工作数量，错误区域可能会更大或更小。在本例中，违规命令是在`FullsceenShadowPass`中发出的一条`DRAWINSTANCED`命令。

{{< note >}}
由于引擎在编译时使用了`-DLY_PIX_ENABLED` CMake 配置标志，因此上述日志中包含了带有通名的标签事件。有关集成[微软 Windows 版 PIX 事件运行时](https://devblogs.microsoft.com/pix/winpixeventruntime/)的信息，请遵循 O3DE Wiki 页面[CPU&GPU 调试和剖析工具](https://github.com/o3de/o3de/wiki/CPU-&-GPU-Debugging-and-Profiling-Tools#pix-cpu--gpu-profiling))中的说明。

有关 DRED 操作的详细信息，请查阅 DirectX 规范 [`WriteBufferImmediate`](https://microsoft.github.io/DirectX-Specs/d3d/D3D12WriteBufferImmediate.html) 和 [DRED](https://microsoft.github.io/DirectX-Specs/d3d/DeviceRemovedExtendedData.html)。
{{< /note >}}
