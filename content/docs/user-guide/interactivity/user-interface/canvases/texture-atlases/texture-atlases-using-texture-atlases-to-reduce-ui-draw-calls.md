---
linkTitle: 使用纹理贴图集减少 UI 绘制调用
description: 使用纹理贴图集减少 UI 绘制调用
title: 使用纹理贴图集减少 UI 绘制调用
weight: 300
---

UI 着色器可以组合最多使用 16 个纹理的绘制调用。如果超出此限制，您可以使用纹理图集来减少绘制调用的数量。

决定将哪些纹理添加到纹理图集的过程分为两部分：

1. 找出哪些画布具有绘制调用，这些调用可以通过使用纹理图集来减少。

1. 确定要放置在特定纹理图集中的纹理。

要收集数据以做出这些决定，您可以使用 O3DE 编辑器控制台。

**确定要添加到纹理图集的纹理**

1. 在 O3DE Editor 控制台中，将 `ui_DisplayDrawCallData` 控制台变量设置为 **1** 以显示每个加载的画布的绘制调用信息，如以下示例所示。

   ```
   ui_DisplayDrawCallData 1
   ```

1. 在绘制调用信息显示的最后一列中，记下 **XTex** 下的值。**XTex** 列显示当达到着色器支持的最多 16 个纹理时绘制调用的数量。要减少绘制调用的数量，请使用纹理图集。

![Location of the XTex column in the draw call information output.](/images/user-guide/interactivity/user-interface/canvases/texture-atlases/ui-editor-texture-atlases-8.png)

1. 在 O3DE Editor 控制台中，输入 `ui_ReportDrawCalls`控制台命令。此命令将所有活动画布的绘制调用报告输出到文本文件中。

1. 打开 `drawcallreport.txt` 日志文件。

1. 在报告结束时，检查以下两个部分以确定要放入纹理图集的纹理。

   ```
   --------------------------------------------------------------------------------------------
   Textures used on multiple canvases that are causing extra draw calls
   --------------------------------------------------------------------------------------------
   ```

   ```
   --------------------------------------------------------------------------------------------
   Per canvas report of textures used on only that canvas that are causing extra draw calls
   --------------------------------------------------------------------------------------------
   ```

1. 在决定将哪些纹理放入纹理图集时，请考虑以下几点：
   + 仅当屏幕上同时使用图集中的纹理时，纹理图集才会减少绘制调用。因此，为了减少绘制调用，请将将要同时在屏幕上的纹理放入同一纹理图集中。

     例如，假设您有纹理集 A 和 B。A 中的纹理以一种屏幕状态显示，B 中的纹理以不同的屏幕状态显示。在这种情况下，将 A 中的纹理放入一个纹理图集中，将 B 中的纹理放入不同的纹理图集中。
      + 纹理图集的默认最大大小为`4096x4096`。

由于这会限制纹理图集中的纹理数量，因此让 UI 加载多个单独的纹理图集是一种很好的做法。

有关`ui_DisplayDrawCallData` 和 `ui_ReportDrawCalls` 命令的更多信息，请参阅 [调试 UI 画布](/docs/user-guide/interactivity/user-interface/canvases/debugging-ui-canvases)。
