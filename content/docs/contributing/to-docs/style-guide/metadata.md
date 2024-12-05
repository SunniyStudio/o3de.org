---
title: Metadata Used in O3DE Documentation
description: A reference for the metadata fields used in Open 3D Engine (O3DE) documentation, including those required in the header.
linktitle: Metadata
weight: 400
toc: true
---

Open 3D Engine （O3DE）** 文档中的每个 Markdown 文件都必须以提供有关内容信息的 Hugo front matter（元数据）开头。有关所有可用变量，请参阅《Hugo Content Management Guide》中的 [Front Matter](https://gohugo.io/content-management/front-matter/)。在 O3DE 文档中，有五个常用的 front matter 变量，如下所示：

```yaml
---
linkTitle: Rigid Bodies
title: Dynamic Rigid Body Simulation with PhysX
description: An introductory tutorial for rigid body simulation with PhysX in Open 3D Engine (O3DE).
weight: 300
toc: true
---
```

将 front matter 变量放在 Markdown 源文件的顶部，并将它们用三个破折号 `---` 括起来。每个 O3DE 主题必须至少有 `linkTitle`、`title` 和 `description` 变量（按此顺序）。

变量 |用法
:--| :-----
`linktitle:` | 显示在链接（如目录）中的短标题。
`title:` | 显示在页面上的长标题和 H1 标题。
`description:` | 主题内容的简短描述。
`weight:` | 用于对列表内容进行排序的值，例如目录。较低的权重值在列表中排名靠前。最好对权重值使用 100 的增量，以确保将来可以正确插入和排序其他主题。
`toc:` | 如果为 true，则从页面右侧装订线中的章节标题生成目录。
