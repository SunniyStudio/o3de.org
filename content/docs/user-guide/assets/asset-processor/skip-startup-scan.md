---
linkTitle: 跳过启动扫描
title: Asset Processor跳过启动扫描
description: 对于在资产处理器关闭时修改的资产，跳过启动检查。
weight: 500
toc: true
---

如果在资产处理器关闭时没有修改资产，跳过启动扫描可改善 O3DE 编辑器的启动时间。你可以随时切换启动扫描模式，但需要重新启动资产处理器才能使更改生效。资产处理器会在会话之间保存你的首选项。

跳过启动扫描旨在辅助快速代码迭代工作流（例如编辑、构建和调试循环）。只有在不修改资产处理管道或不对源资产进行更改的情况下才可使用。一旦进入闲置状态，资产处理器就会监视资产变化。

启用跳过启动扫描后
1. 资产处理器启动后会跳过对资产修改的启动检查，直接进入空闲状态。
2. 2. O3DE 编辑器会收到来自资产处理器的信息，表明所有资产都已准备就绪，可以使用。
3. O3DE 编辑器解锁启动。

禁用跳过启动扫描时
1. 资产处理器会扫描并处理在关闭状态下被修改的任何资产。
2. 2. O3DE 编辑器从资产处理器收到一条信息，表明**关键**资产已处理完毕，可以使用。
3. O3DE 编辑器解锁启动。
4. 资产处理器完成所有资产的处理并进入空闲状态。

## 选择启动扫描模式

![The Skip Startup Scan settings in Asset Processor](/images/user-guide/assets/asset-processor/skip-startup-scan-settings.png)

资产处理器图形用户界面的 “跳过启动扫描模式 ”默认为禁用。要在图形用户界面中启用/禁用 “跳过启动扫描模式”，请执行以下操作：
1. 在Asset Processor中选择Settings标签页。
2. 切换**Skip Startup Scan**选项。
3. 重启Asset Processor和编辑器。

{{< important >}}
如果在资产处理器未运行时修改资产，跳过启动检查可能会导致意想不到的行为。
即使跳过启动扫描模式处于活动状态，也可以 [执行全面扫描](/docs/user-guide/assets/asset-processor/faster-scanning/#perform-a-full-scan)。
{{< /important >}}
