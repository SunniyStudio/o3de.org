---
linkTitle: 快速参考
title: "风格指南：快速参考"
description: 面向 Open 3D Engine （O3DE） 文档贡献者的样式指南的精简参考。
weight: 150
toc: true
---

请遵循文档中的 **Open 3D Engine （O3DE）** 文档样式指南。这可确保文档的一致性和对用户有帮助。

| **语言** |  |
| --- | --- |
| [为辅助功能而编写](guidance#write-for-accessibility) | 文档以美国英语编写。尽可能使用简单明了的词语。写出简短而完整的句子。将信息分解为较小的文本正文。 |
| [语音和语气](guidance#voice-and-tone) | 尽可能使用 [主动语态](https://writing.wisc.edu/handbook/style/ccs_activevoice/) 和现在时动词。简单、尊重和专业地写作。 <br><br>将用户称为“您”。将 O3DE 软件、O3DE 社区或 O3D 基金会称为“我们”。 |
| [O3DE 术语](../terminology#o3de-specific-terms) | 使用术语页面中定义的特定于 O3DE 的术语。如果您引入新术语，请更新术语页面。 |
| [标准域和行业术语](../terminology#standard-domain-and-industry-terminology) | 使用领域或行业的标准术语。如果存在歧义，请包含额外的上下文以确保读者理解其含义。 |
| [应避免的术语](../terminology#terms-to-avoid-and-their-alternatives) | 在您选择的词语中以包容性为目标。 |
| [首字母缩略词、缩写和拉丁语短语](guidance#acronyms-abbreviations-and-latin-phrases) | 首先以扩展形式编写首字母缩略词或缩写词，然后在括号中写下首字母缩略词。不要缩写常用词或使用拉丁短语;使用完整的单词或类似的单词。 |
| [成语、俚语、口语或行话](guidance#idioms-slang-colloquialisms-or-jargon) | 不要使用它们。这些单词和短语可能被美国英语使用者理解，但很难翻译。  |
| **格式化** |
| [Hugo](../hugo) 和 [Markdown](format) | 文档以 Markdown （`.md`） 文件编写，并由静态站点生成器 [Hugo](https://gohugo.io/) 构建。它主要使用 [Markdown 语法](https://www.markdownguide.org/basic-syntax/)进行格式设置，并支持内联 HTML。其他内容格式包括图像、视频、标注框、数学方程式和图表。标注框是通过 [shortcodes](shortcodes)实现的。数学方程可以使用 [MathJax 格式化](tools#math-formulas) 进行渲染。图表可以在 Markdown 代码块中使用 [Mermaid syntax](tools#diagrams) 来渲染。 |
| [Metadata](metadata) | 文档必须符合元数据要求，包括 *linktitle*、*title* 和 *description*。使用 *weight* 覆盖默认的字母排序顺序。 |
| [主题标题](format.md#topic-headings) | 章节标题必须使用 H2 （`##`） 标题。小节标题为 H3 （`###`） 和 H4 （`####`） 标题。主题标题必须使用句子大小写。H1（`#`） 标题保留用于主题元数据中指定的页面标题。  |
| [用户界面、输入和热键](format#user-interface-inputs-and-hotkeys) | 粗体所有实例. |
| [文件、目录和路径](format#files-directories-and-paths) | 使用 `code-style` 格式。提供有关路径相对于什么的上下文。如果适用，请链接到 [o3de 存储库中的文件](https://github.com/o3de/o3de)。  |
| [应用程序、工具、Gem 和组件](format/#applications-tools-gems-and-components) | 仅对第一次出现的页面进行粗体处理。同一页面上的后续匹配项将保持未格式化。  |
| [代码、命令和 API 对象](format#code-commands-and-apis) | 对命令和 API 对象使用 `code-style` 格式。对多行代码使用代码块。 |
| [术语](format/#terminology) | 使用术语时保持一致。将新术语斜体化，然后给出定义。O3DE 独有的术语必须包含在 [术语页面](../terminology#o3de-specific-terms) 中。 |
| [商标术语](format/#trademark) | 根据商标标题和术语的来源使用情况，正确设置商标标题和术语的格式。提供指向来源相关材料的链接。 |
| **附加内容** |
| [图像](media#adding-images-with-markdown-syntax) | 静态图像必须为`.png`格式。图表必须为 `.svg` 格式。 |
| [动画图像](media.md#animated-images) | 使用短视频演示功能的功能。首选使用小于 512 KB 的`.mp4`文件。视频大小不得超过 1 MB。使用 `.gif` 格式已弃用，不再允许使用。 |
