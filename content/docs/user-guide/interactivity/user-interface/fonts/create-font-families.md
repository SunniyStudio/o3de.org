---
linkTitle: 创建字体系列
description: 组合多个字体资源以创建一个字体系列组，以便在 Open 3D Engine 中的游戏 UI 中使用。
title: 创建字体系列
weight: 200
---

您可以将多个字体资源合并到一个字体系列组中。

以下是 `.fontfamily` 文件的示例。

```
<fontfamily name="MyFontFamily">
    <font>
        <file path="myfontfamily-regular.xml" />
        <file path="myfontfamily-bold.xml" tags="b" />
        <file path="myfontfamily-italic.xml" tags="i" />
        <file path="myfontfamily-bolditalic.xml" tags="b,i" />
    </font>
</fontfamily>
```

UI 系统使用字体系列定义来确定在设置文本样式时要应用的字体资源。您可以组合以下类型的资产：
+ **Unstyled** - 表示未应用样式的文本的字体。在前面的示例中，这是 `myfontfamily-regular.xml`.
+ **Bold** - 表示具有粗体样式的文本的字体。
+ **Italic** - 表示具有斜体样式的文本的字体。
+ **Bold-Italic** - 表示具有粗体和斜体样式的文本的字体。

## 字体系列文件 XML

要创建新的字体系列文件，可以创建一个新的空纯文本文件并输入内容，也可以修改现有的字体系列文件。

**将新的字体系列文件添加到 UI 中**

1. 要创建新的字体系列文件，请执行下列操作之一：
   + 打开记事本（或类似程序）并保存一个文件扩展名为 `.fontfamily`的空文本文件。
   + 复制现有的`.fontfamily`文件到游戏项目的`Fonts`目录。

1. 适当地命名你的 `.fontfamily` 文件 \(保留`.fontfamily` 扩展名\).

1. 打开 `.fontfamily` 文件并编辑内容以配置字体系列。

   例如：

   ```
   <fontfamily name="MyFontFamily">
       <font>
           <file path="myfontfamily-regular.xml" />
           <file path="myfontfamily-bold.xml" tags="b" />
           <file path="myfontfamily-italic.xml" tags="i" />
           <file path="myfontfamily-bolditalic.xml" tags="b,i" />
       </font>
   </fontfamily>
   ```

在 Asset Processor 处理完您的字体资源后，您可以通过在 UI 编辑器中选择`*.fontfamily`文件作为任何文本组件的字体来选择字体系列。要使用字体系列将自定义样式应用于文本，请参阅 [文本样式标记](../components/visual/components-text#text-markup).

`*.fontfamily`文件使用 XML。UI 系统支持`*.fontfamily`文件的以下标记和属性：

Tag: `fontfamily`
**Attribute**: `name`
字体系列的唯一名称。项目中的每个字体系列名称都必须是唯一的，并且每个 `.fontfamily` 文件只能指定一个 `fontfamily` 标签。但是，您可以在多个字体系列中重复使用相同的字体 XML 文件（由 file 标签定义）。

Tag: `font`
`file`标签的容器标签。
**Attribute**: `lang`
字体文件应与之关联的语言。仅当使用列出的语言时，才会加载字体文件。这使单个字体系列能够根据所使用的语言使用不同的字体和样式。

Tag: `file`
**Attribute**: `path`
字体 XML 的路径，即 TTF 或 OTF 文件。该路径是相对于字体系列文件的。对于给定的字体系列和多个字体系列，可以多次引用相同的字体资源。
**Attribute**: `tags`
此标签是可选的。如果省略，则在未应用样式时使用此字体文件。
Values:
+ **b** - 指明 `<b>` bold tag
+ **i** - 指明 `<i>` italic tag
+ **b,i** - 指示何时应用`<b>` 粗体和`<i>`斜体标记
