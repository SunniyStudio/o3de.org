---
linkTitle: 调试 UI 画布
description: ' 使用控制台变量和命令在 Open 3D Engine 中调试 UI 画布。 '
title: 调试 UI 画布
weight: 700
---

您可以使用以下控制台命令和控制台变量在游戏运行时显示 UI 的调试信息。

## ui\_DisplayCanvasData 

显示已启用或已加载的画布的画布数据。

|值 |描述 |
|-- |--- |
|0 |关闭。|
|1 |显示每个加载的画布的一行信息。|
|2 |仅显示已启用的画布的信息。|

以下示例显示 5 个已加载 UI 画布的数据。

![Canvas data for five loaded UI canvases.](/images/user-guide/interactivity/user-interface/canvases/ui-editor-debugging-ui-canvases-1.png)

下表描述了每列。

| **列**     |**说明** |
|-----------| --- |
| NN        | 画布在列表中的索引号。画布将按其绘制顺序列出。 |
| Name      | 叶画布名称。 |
| En        | 画布是否已启用。 |
| Po        | 画布是否接受位置输入 （例如，鼠标输入）。 |
| Na        | 画布是否启用了导航。 |
| DrawOrder | 绘制顺序，用于对加载的画布列表进行排序。 |
| nElem     | 画布中的 UI 元素数。 |
| nEnab     | 画布中已启用的 UI 元素的数量。如果未启用父级，则不会对元素进行计数。 |
| nRend     | 画布中已启用的可渲染元素的数量（正在渲染的图像、文本和粒子效果的数量）。 |
| nRCtl     | 画布中启用的“渲染控制”元素（蒙版和推子）的数量。|
| nImg      | 具有 UiImageComponents 的已启用 UI 元素的数量。 |
| nText     | 使用 UiTextComponents 启用的 UI 元素的数量。 |
| nMask     | 具有 UiMaskComponents 的已启用 UI 元素的数量。 |
| nFadr     | 使用 UiFaderComponents 启用的 UI 元素的数量。 |
| nIntr     | 具有可交互组件（Button、Slider、TextInput 等）的已启用 UI 元素的数量。 |
| nUpdt     | 具有侦听更新（即可能每帧执行某项操作）的组件的已启用 UI 元素的数量。 |
| ActiveInt | 此画布上活动可交互对象的名称（如果有）。 |
| HoverInt  | 此画布上当前悬停可交互对象的名称（如果有）。 |

## ui\_DisplayDrawCallData 

显示用于渲染 UI 画布的绘制调用数。此变量对于性能优化和调试非常有用。


|价值 |描述 |
|--- |--- |
|0 |关闭显示。|
|1 |打开显示屏。|

以下示例数据显示了四个 UI 画布的绘制调用信息。

![Draw call information for four UI canvases.](/images/user-guide/interactivity/user-interface/canvases/ui-editor-debugging-ui-canvases-2.png)

下表描述了每列。

| 列 | 说明 |
| --- | --- |
| NN | 画布在列表中的索引号。画布将按其绘制顺序列出。 |
| Canvas name | 叶画布名称。 |
| nDraw | 绘制调用的数量。 |
| nPrim | 基元 （例如，图像和文本字符串） 的数量。 |
| nTris | 为 UI 渲染的三角形数。 |
| nMask | 渲染图中蒙版渲染节点的数量。 |
| nRTs | 渲染图中渲染目标渲染节点的数量。 |
| nUTex | 此帧中画布中渲染的唯一纹理的数量。 |
| XMask | 使用遮罩导致的绘制调用数。 一个遮罩可能会导致最多四个额外的绘制调用。  |
| XRT | 呈现目标导致的绘制调用数。 |
| XBlnd | 混合模式更改导致的绘制调用数。 |
| XSrgb | 由 Srgb 写入更改引起的绘制调用数。此数据点仅针对呈现目标（例如，播放视频）显示。 |
| XMaxV | 由需要超过 65536 个顶点或 16384 个四边形的渲染节点引起的绘制调用数。这种情况并不常见。例如，显示值需要超过 16000 个字符的文本。|
| XTex | 达到着色器支持的最多 16 个纹理时发生的绘制调用数。要减少这些调用的数量，您可以使用纹理贴图集。有关详细信息，请参阅 [使用纹理图集](/docs/user-guide/interactivity/user-interface/canvases/texture-atlases). |

## ui\_DisplayElemBounds 

此控制台命令在屏幕上显示一个覆盖层，该覆盖层显示元素的矩形。默认情况下，它显示每个已启用的 UI 画布的每个 UI 元素的矩形边界。

如果您有多个已启用的 UI 画布，并且只想查看一个画布的矩形边界，请使用`ui_DisplayElemBoundsCanvasIndex`控制台变量。要使用`ui_DisplayElemBoundsCanvasIndex`控制台变量，请指定要显示其边界的画布的索引。要查找已启用画布的索引，请使用 `ui_DisplayCanvasData 2` 设置。

下面的示例演示嵌套滚动框的矩形边界。

![Rectangular bounds displayed for nested scrollboxes.](/images/user-guide/interactivity/user-interface/canvases/ui-editor-debugging-ui-canvases-3.png)

## ui\_DisplayTextureData 

显示 UI 正在使用的纹理。

显示屏显示 UI 系统在当前帧中使用的每个纹理的尺寸、数据大小、纹理格式和路径名。纹理按它们使用的内存量降序排序。

以下示例显示了当前帧中 13 个唯一纹理的数据。

![Data for 13 unique textures in the current frame.](/images/user-guide/interactivity/user-interface/canvases/ui-editor-debugging-ui-canvases-4.png)

## ui\_ReportDrawCalls 

将绘制调用的报告写入日志文件。

命令输出显示日志文件的位置，如以下示例所示。

![Entering the ui_ReportDrawCalls command.](/images/user-guide/interactivity/user-interface/canvases/ui-editor-debugging-ui-canvases-5.png)

日志文件将写入日志目录中的 `drawcallreport.txt` 中。

日志文件列出了每个已启用画布的所有绘制调用。该报告可用于确定如何减少绘制调用的数量。

有关详细信息，请参阅 [使用纹理图集减少 UI 绘制调用](/docs/user-guide/interactivity/user-interface/canvases/texture-atlases/texture-atlases-using-texture-atlases-to-reduce-ui-draw-calls).
