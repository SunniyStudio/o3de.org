---
title: 将媒体提交到 O3DE 文档
description: 将媒体（图像、视频、音频或资源）提交到 Open 3D Engine （O3DE） 文档的指南。
linktitle: Media
weight: 700
toc: true
---

在 Open 3D Engine （O3DE） 文档中，有两种可用的方法可以将图像添加到主题中。你可以使用 markdown 语法或 `image-width` 短代码。如果要添加多个图像、非常大的图像或`.svg`图表，请考虑使用 `image-width` 短代码来限制图像的宽度，因为图像缩放到浏览器窗口的宽度。

## 使用 markdown 语法添加图片

要使用 markdown 语法添加图像，请使用类似于链接的语法，前面带有感叹号。当图像未加载时，会显示括号中的文本。括号内是指向图像资产的链接，以及双引号中的可选图像标题。

图片示例：

```markdown
![O3DE Logo](/img/logos/O3DE-Logo-with-WordMark-Black-Mono.svg "The O3DE logo")
```

图像输出：

![O3DE Logo](/img/logos/O3DE-Logo-with-WordMark-Black-Mono.svg "The O3DE logo")

## 使用 `image-width` 短代码添加图像

`image-width` 短代码添加带有替代文本的图像并限制图像的宽度。`image-width` 短代码可以确保图像大小在主题内保持一致，并且大图像在较宽的浏览器窗口中不会缩放得太大。在添加`.svg`图、非常大的图像以及需要一致图像宽度的多个图像的主题中使用此方法。

`image-width` 采用三个双引号命名参数：

1. `src="/images/<image.png>"` - 图像文件路径。
1. `width="<image width>"` - 通过指定宽度（以像素为单位）来缩放图像。
1. `alt="<image description>"` - 描述图像的字符串。

`image-width` 示例:

```markdown
{{</* image-width src="/images/welcome-guide/guide_img.png" width="700" alt="The O3DE Welcome Guide splash image." */>}}
```

`image-width` 示例输出:

{{< image-width src="/images/welcome-guide/guide_img.png" width="700" alt="The O3DE Welcome Guide splash image." >}}


## 替换文本

图像的替代文本非常重要。如果未加载图像，它会提供图像的描述。替代文本还用于辅助功能和搜索引擎优化 （SEO）。确保为图像提供的替代文本清楚地描述了图像的内容。

## 图片文件位置

图像放置在 O3DE 文档存储库的 `/static/` 目录的子目录中。

特定于指南的图像（如界面屏幕截图和图表）位于`/static/images/`的子目录中。`/static/images/` 的目录结构是 `/content/docs/` 目录结构的镜像。像 logo 这样的共享图片被放置在 `/static/img/` 的子目录中。在 PR 中提交图像时，请确保将图像放置在适当的子目录中。

## 一般图像指南

为了标准化演示并将 O3DE 文档存储库保持在 1 GB 以下，在创建用于文档的图像时，请遵循以下准则：
  
* 图片中的文本应遵循我们的样式指南，无格式要求。
* 请勿在屏幕截图中包含个人身份信息 （PII）。PII 包括但不限于名称、地理信息、项目名称、IP 地址、DNS 名称和目录路径。我们建议您裁剪可能包含 PII 的图像区域，并专门为不暴露 PII 的文档贡献创建项目。不建议使用常见的图像过滤器来模糊或伪装 PII，因为它们有时可以反转。要从图像中删除 PII：

  1. 在任何图像编辑器中，在 PII 周围绘制一个矩形选择框。

  1. 使用剪切工具或 **CTRL+X** 从图像中完全剪切信息。

  1. 用纯色填充空白选择区域，例如黑色或白色。

  1. 保存编辑后的图像并将其包含在您的 PR 中。

    {{< important >}}
  在将图像添加到拉取请求之前，请务必彻底检查所有图像的 PII！
    {{< /important >}}

## 截图

屏幕截图是各种 O3DE 用户界面或其他应用程序（如内容创建工具）的界面的图像。

* 所有屏幕截图使用 PNG 格式(`.png`)。
* 以高清分辨率 （1920x1080） 拍摄界面屏幕截图。
* 确保图像分辨率的宽度和高度像素数为偶数。
* 不要对界面屏幕截图使用激进的压缩。高度压缩的文件可能会使文本和 UI 元素难以辨认。
* 屏幕截图的大小不应超过 512 KB。
* 对屏幕截图使用默认界面比例。
* 屏幕截图使用默认界面颜色主题。
* 尽可能使用默认界面布局。
* 不要缩放具有界面元素的屏幕截图。
* 尽可能裁剪界面图像以仅包含必要的元素。
* 教程和用户指南内容的屏幕截图应包含 O3DE 用户免费提供的模型、角色和纹理资产：
  * 作为 O3DE 的一部分分发的资产。
  * 使用宽松的开源许可证分发的资产，例如 [斯坦福 3D 扫描存储库](https://graphics.stanford.edu/data/3Dscanrep/) 中的资产。
  * 可以在 [Blender](https://blender.org) 和 [Krita](https://krita.org/)等开源内容应用程序中快速复制的资源。
  * 如果您使用第三方分发的资源，请在文档中为该资源提供适当的归属和链接。

## 艺术图像

巧妙的图像用于艺术或营销目的，例如演示 Atom 渲染器的输出或 Atom 材质的保真度的图像。

* 使用 PNG 格式 （`.png`） 或 JPEG 格式 （`.jpg`） 制作巧妙的图像。
* 巧妙的图像可能高达 UHD 分辨率 （3840x2160）。
* 巧妙的图像大小不应超过 1 MB。
* 巧妙的图像可能包含非免费提供或不易复制的资产。

## 图像注释

* 我们建议使用开源工具 [ShareX](https://getsharex.com/)进行图像注释。
* 在注释中使用尽可能少的文本。在图像下方添加有序列表，用于图像注释的文本解释.
* 在图像注释中使用 [Open Sans](https://fonts.google.com/specimen/Open+Sans) 粗体字体。
* 在图像注释中使用美式英语。
* 使用纯色填充，而不是渐变填充。
* 避免使用受色盲影响的颜色。我们建议使用白色文本和罗丹明红 C（RGB：225,0,152 十六进制：E10098）进行图像注释。请参阅以下带注释的图像示例：

{{< image-width "/images/welcome-guide/ui-editor-labeled.png" "1000" "An annotated image of O3DE Editor's user interface." >}}

## 图表

* 使用 [draw io](https://app.diagrams.net) 等图表工具创建图表。
* 图表使用 SVG 格式 （`.svg`）。
* 保持图表简单。将复杂的图表分解成更小的块。
* 在图表中使用尽可能少的文本。
* 图表文本使用 [Open Sans](https://fonts.google.com/specimen/Open+Sans) 字体。
* 使用纯色填充，而不是渐变填充。
* 避免使用受色盲影响的颜色。对红色、绿色、蓝色和黄色的色调进行选择性选择。

## 动画图像

由于存储库大小的限制，贡献中不接受动画“`.gif`”图像。如果您必须在主题中包含动画图像，则可以使用“`.mp4`”视频。但是，如果动画演示了一系列步骤，则可以考虑使用图像条而不是视频。

### 图像条

为了演示步骤，请考虑使用一个水平的 2 到 4 面板注释图像条，以演示该过程的开始、操作和结果。请参阅以下示例：

{{< image-width "/images/contributing/to-docs/image-strip-example.png" "700" "An image strip example with two panels." >}}

1. 在 **Perspective** 中 **左键单击** 实体以将其选中。
1. 在变换小工具的 **Z** 轴上拖动 **，以在世界 **Z** 轴上移动实体。

### 使用 `video` 短代码嵌入视频

`/images/` 目录中的视频可以嵌入到带有 `video`短代码的主题中。使用此方法可在主题中包含动画，而不是动画 `.gif`。

在 docs 贡献中提交视频时，请记住以下几点：

* `.mp4`是首选的视频格式。`.ogg` 和 `.webm` 也被短代码支持，但各种浏览器都不支持。
* 视频必须按照与图像相同的约定放置在 `/static/images/` 目录结构中。
* 视频大小应小于 512 KB，且大小不得超过 1 MB。
* 如果需要，请确保视频中的文本清晰易读。
* 确保视频分辨率不超过所需分辨率。
* 确保视频分辨率的宽度和高度像素数为偶数。
* 适当地裁剪和构图视频的主题。
* 除非示例要求，否则视频不应包含音频。
* 虽然海报图片不是必需的，但建议使用。

**参数**

`video`**需要** 以下命名参数：

1. `src="/images/<video.mp4>"` - 视频文件路径。
1. `info="<video description>"` - 描述视频的字符串。

`video` 支持以下 **可选** 命名参数：

1. `poster="/images/<image.png>"` - 在视频加载或视频加载失败时显示的静态图像。此图像的大小和纵横比应与视频相同。
1. `autoplay="true"` - 视频加载后立即播放。
1. `loop="true"` - 视频循环播放。
1. `width="<video width>"` - 通过指定宽度（以像素为单位）来缩放视频。
1. `muted="true"` - 如果存在音轨，则视频将静音。
1. `type="video/<mp4 OR ogg OR webm>"` - 视频类型。MP4 是默认的。

这是两个始终启用的附加选项。

1. `preload="auto"` - 将视频与页面一起加载。
1. `controls` - 包括视频的播放器控件。

**示例**

1. 基本 `video` 用法，仅包含必需的参数。

    ```markdown
      {{</* video src="/images/contributing/to-docs/TestVideo.mp4" info="This is a test video." */>}}
    ```
    输出:

    {{< video src="/images/contributing/to-docs/TestVideo.mp4" info="This is a test video." >}}

<br>

2. 高级`video`用法和可选参数，用于启用自动播放、循环播放视频、将视频缩放到 250 像素以及包含海报图像。

    ```markdown
      {{</* video src="/images/contributing/to-docs/TestVideo.mp4" info="This is a test video." autoplay="true" loop="true" width="250" poster="/images/contributing/to-docs/TestPoster.png" */>}}
    ```

    输出:

    {{< video src="/images/contributing/to-docs/TestVideo.mp4" info="This is a test video." autoplay="true" loop="true" width="250" poster="/images/contributing/to-docs/TestPoster.png" >}}


## 使用 `youtube-width` 短代码嵌入 Youtube 视频

使用 `youtube-width` 短代码将 Youtube 视频嵌入到您的页面中。`youtube-width` 短代码是 Hugo 内置的 [`youtube` shortcode](https://gohugo.io/content-management/shortcodes/#youtube) 的扩展版本，允许您控制嵌入视频的大小。

`youtube-width` 支持以下双引号参数：

1. id
2. title
3. width (建议使用默认值 （50%）)

**示例** 

1. 没有 `width` 参数的 `youtube-width` 示例使用默认值 `width: 50%`：

    ```markdown
    {{</* youtube-width id="CQmjAxr7LZs" title="What is O3DE?" */>}}
    ```

    输出:

    {{< youtube-width id="CQmjAxr7LZs" title="What is O3DE?" >}}

    <br>

2. 带有 `width` 参数的 `youtube-width` 示例：

    ```markdown
    {{</* youtube-width id="CQmjAxr7LZs" title="What is O3DE?" width="100%" */>}}
    ```

    输出:

    {{< youtube-width id="CQmjAxr7LZs" title="What is O3DE?" width="100%" >}}
