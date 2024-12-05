---
title: O3DE 文档编写指南
description: 用于编辑所有 Open 3D Engine （O3DE） 文档的语言、书写和复制标准，以确保一致性和可读性。
linktitle: Writing
weight: 200
toc: true
---

**Open 3D Engine （O3DE）** 文档项目服务于全球受众。这些指南有助于让每个人都能访问您的作品。

## 为辅助功能而写作

文档以美国英语编写。尽可能使用简单明了的词语。写出简短而完整的句子。将信息分解为较小的文本正文。

- **精确词语**：尽可能使用具有相同定义的精确词语，或使用基于其主要定义的词语。例如，仅在引用执行操作的次数时使用 “once”。不要使用“一次”来标记某个时间点，就像“下载完成后”一样。

- **简短而完整的句子**：将包含多个子句的长句分解为几个短句，以提高可读性。句子必须有主语、动词和宾语，末尾有标点符号。

- **形容词和副词**：保持形容词和副词靠近它们修饰的单词。

    | 要 |不要 |
    | :--| :----- |
    | First, select the entity. | Select the entity first. |
    | ... to quickly build proxy geometry. | ... to build proxy geometry quickly. |

- **-ing 单词**：谨慎使用以 *-ing* 结尾的单词。以 *-ing* 结尾的单词可以是动词、形容词或动名词，对于 ESL 读者来说可能是模棱两可的。如果您必须使用以 *-ing* 结尾的单词，请在单词之前或之后添加限定词，例如 *the*，以阐明该单词是动词还是形容词。以“rendering”这个词为例。“Rendering” 可用于许多上下文，并且可能会引起混淆：

    | Usage | Example |
    | :--| :-- |
    | Noun | ... view the rendering. |
    | Verb | ... rendering the scene. |
    | Adjective | ... the rendering path. |

- **-ed 单词**：谨慎使用以 *-ed* 结尾的单词。以 *-ed* 结尾的单词也可能是模棱两可的。使用限定词短语（如 *that is*）来阐明以 *-ed* 结尾的单词的用法。以 “based” 这个词为例。

    | Do | Don't |
    | :--| :-- |
    | Atom is a renderer *that is* based on ... | Atom is a renderer based on ... |

- **有用的词语**：使用可选的词语和短语来澄清，例如 *the*、*that*、*a*、*an*、*because*、*after*、*although* 和 *might*。

- **逗号**：使用逗号使句子更易于阅读和理解。在列表中使用连续逗号或 “Oxford” 逗号。在下面的示例中，请注意列表中的最后一项和前一项之间出现逗号。

    **示例**：“**Asset Processor** 检查新文件，检测更改的文件，并使用资产清单文件处理游戏就绪的资产。



## 语音和语气

尽可能使用 [主动语态](https://writing.wisc.edu/handbook/style/ccs_activevoice/) 和现在时动词。简单、尊重和专业地写作。

将用户称为“您”。

将 O3DE 软件、O3DE 社区或 O3D 基金会称为“我们”。例如，您可以说 “We recommend...”。

## 首字母缩略词、缩写词和拉丁语短语

使用首字母缩略词时，请以扩展形式介绍它们，后跟括号中的首字母缩略词。后续的页面引用只能使用首字母缩略词。

不要缩写常用词或拉丁短语;使用完整的单词或类似的单词。

**异常**
不需要拼写的首字母缩略词：
- 常见文件格式（示例：JSON、PDF、JPEG、PNG）
- 其他常见技术术语（示例：URL、ID）

| 要 |不要 |
| :-- | :-- |
| Welcome to Open 3D Engine (O3DE)! | Welcome to O3DE! |
| For example, Example: | e.g., Ex: |
| versus, compared to | vs. |


## 成语、俚语、口语或行话

不要使用成语、俚语、口语或行话。美国英语母语人士通常使用和理解许多单词和短语，可能难以翻译。

同样，避免开玩笑。

|类型 |定义 |避免 |
| :--| :-- | :-- |
| Idiom | A phrase established to have a meaning that is not discernible from the individual words. | Forward+ rendering provides *the best of both worlds*.
| Slang | Informal and nonstandard vocabulary. | *Chill* for a bit, while the O3DE project compiles.
| Colloquialism | Ordinary and familiar conversational words and phrases, especially those that might be specific to a region. | ... On Create will execute the function *ASAP*.
| Jargon | Specialized terms used in a particular field that are difficult for others to understand. | ... entering the *vertical-slice* phase of development.
