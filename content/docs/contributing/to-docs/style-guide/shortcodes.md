---
title: O3DE 文档中使用的 Hugo 简码
description: Open 3D Engine （O3DE） 文档中使用的短代码列表。
linktitle: 简码
weight: 600
toc: true
---

使用复杂元素（如嵌入式视频链接和标注）丰富 **Open 3D Engine （O3DE）** 文档页面，请使用 Hugo [简码](https://gohugo.io/content-management/shortcodes)。Markdown 不支持这些复杂的元素，但你可以用其他语言编写短代码，比如 HTML。您可以在 O3DE 文档存储库的 `/layouts/shortcodes`目录中查看当前可用的短代码。

## 标注框

O3DE 文档支持四种不同的标注短代码：**注意** `{{</* note */>}}`， **提示** `{{</* tip */>}}`， **注意** `{{</* caution */>}}` 和 **重要** `{{</* important */>}}`。要使用 call-out 短代码，请将要显示的文本括在左短代码括号和右短代码括号中。

### 注意

使用`{{</* note */>}}` 来引起读者对澄清或信息的关注。对于快捷方式和最佳实践，请考虑改用 `{{</* tip */>}}` 。

例如:

```markdown
{{</* note */>}}
你仍然可以在这些标注中使用 Markdown。
{{</* /note */>}}
```

The output is:

{{< note >}}
你仍然可以在这些标注中使用 Markdown。
{{< /note >}}

### 提示

使用`{{</* tip */>}}`来引起人们对快捷方式或最佳实践的注意。虽然信息应包含与上述内容相关的实用建议，但它不应包含对完成步骤或使用功能至关重要的信息。

例如：

```markdown
{{</* tip */>}}
You can spawn multiple instances of `AssetBuilder.exe` and attach them to Visual Studio.
{{</* /tip */>}}
```

输出为：

{{< tip >}}
您可以生成多个 `AssetBuilder.exe` 实例并将它们附加到 Visual Studio。
{{< /tip >}}

### 注意

使用 `{{</* caution */>}}` 来提醒人们注意重要信息以避免陷阱。

例如：

```markdown
{{</* caution */>}}
The call-out style only applies to the line directly above the tag.
{{</* /caution */>}}
```

输出为：

{{< caution >}}
标注样式仅适用于标签正上方的行。
{{< /caution >}}

### 重要

使用 `{{</* important */>}}` 来表示需要遵循的关键信息。

例如：

```markdown
{{</* important */>}}
避免在标注中使用多个段落。
{{</* /important */>}}
```

输出为：

{{< important >}}
避免在标注中使用多个段落。
{{< /important >}}

### 列表中的标注框

您可以在列表中使用标注。确保缩进第一个 call-out 短代码，如以下说明中所述：

```markdown
1. Use the note shortcode in a list.
1. A second item with an embedded note.

    {{</* note */>}}
Warning, Caution, Tip, and Note shortcodes, when embedded in lists, need to be indented to the level of the list item; four spaces for the first level, eight spaces for the second level, and so on.

Lists that contain shortcodes should have newlines between each list item and before and after the shortcode.
{{</* /note */>}}

1. A third item in a list
1. A fourth item in a list
```

输出为：

1. 在列表中使用 note 短代码
1. 带有嵌入注释的第二个项目

    {{< note >}}
Warning、Caution、Tip 和 Note 短代码嵌入到列表中时，需要缩进到列表项的级别;第一级有 4 个空格，第二级有 8 个空格，依此类推。

包含短代码的列表在每个列表项之间以及短代码前后应有换行符。
{{< /note >}}

1. 列表中的第三项
1. 列表中的第四项
