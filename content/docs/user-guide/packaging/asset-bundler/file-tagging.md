---
description: ' 使用 Open 3D Engine Asset Editor 创建文件标签规则，用于在资源包中包含和排除文件，以便 Asset Bundler 进行处理。
  规则由模式匹配逻辑和文件标签字符串组成。 '
title: 使用文件标记系统包含或排除资源
weight: 400
---

O3DE 使用文件标记系统在处理的各个阶段包含或排除文件。此系统使用文件标签规则来选择与指定模式匹配的文件。文件标签与每个规则相关联，以用作文件标签查询中的键。这是通过使用 O3DE FileTag API 完成的。您可以使用 O3DE 资产编辑器创建自己的自定义文件标签规则。当您需要对在处理步骤中应包含或排除哪些文件时进行额外控制时，这非常有用。例如，作为资产捆绑的一部分，消除在使用 **Missing Dependency Scanner** 后发现的“误报”非常有用。

**主题**
+ [创建文件标签规则](#creating-file-tag-rules)
+ [使用FileTag API](#using-the-filetag-api)

## 创建文件标签规则

使用 O3DE 资产编辑器将自定义文件标签规则添加到`exclude.filetag` 或 `include.filetag`，具体取决于您是排除还是包含资产文件。这两个`.filetag`文件都位于`dev\Engine`目录中。文件标记规则由两个必需部分组成：
+ **File Pattern** 定义与此规则匹配的文件。支持的模式包括：
  + **Exact** \(例如, `readme.txt`\)
  + **Wildcard** \(例如, `*.cfxb`\)
  + **Regex** \(例如, `.*/gems?/?.*/gem.json`\)
+ O3DE 或您自己的代码用作键的一个或多个 **File Tags** ，用于引用此规则定义的文件模式匹配。

您可以使用 **Comment** 字段添加有关文件标记规则的更多信息，例如记录其使用情况。

{{< note >}}
某些文件标签在 O3DE 中具有指定的用途。各种工具可能要求您使用特定的标签，例如 `editoronly` 和 `shader`。可以在`dev\Code\Framework\AzFramework\AzFramework\FileTag\FileTag.h`中的`FileTagsIndex`枚举中找到常用标记的完整列表。
{{< /note >}}

**如何创建文件标签规则**

1. 在 O3DE 编辑器中，选择**Tools**, **Asset Editor**.

1. 选择 **File**, **Open**, 并从`Engine`目录选择 `exclude.filetag` 或 `include.filetag`。

1. 打开 **Definition**, 找到标记为 **File Tag Map**的行，点击 '**+**' 按钮添加新的子元素。

![Start a new file tag rule by adding a new element to the File Tag Map.](/images/user-guide/assetbundler/asset-bundler-filetag-new-element.png)

1. 在 **New Key** 字段中输入所需的文件匹配模式。

![Specify the desired file matching pattern.](/images/user-guide/assetbundler/asset-bundler-filetag-new-key.png)

1. 从列表中打开您的新文件模式键，然后选择相应的 **File Pattern** 类型。

1. 添加一个或多个 **File Tags** 以与您的新文件模式关联。

![Complete the new file tag rule by selecting a File Pattern type and entering one or more File Tags.](/images/user-guide/assetbundler/asset-bundler-filetag-example.png)

1. 选择 **File**, **Save** 以保存新的 File 标签规则。

## 使用 FileTag API

您可以使用 C++ FileTag API 编写自己的逻辑来确定是包含还是排除文件。以下示例使用文件标记系统忽略与`ignore` 和 `shader`标记关联的模式匹配的文件。

```
   bool IsIgnored(const char* szPath)
   {
       using namespace AzFramework::FileTag;
       AZStd::vector<AZStd::string> tags{ "ignore", "shader" };

       bool shouldIgnore = false;
       QueryFileTagsEventBus::EventResult(shouldIgnore, FileTagType::exclude, &QueryFileTagsEventBus::Events::Match, szPath, tags);

       return shouldIgnore;
   }
```

{{< note >}}
在前面的示例中，它显示了对 ID`FileTagType::exclude`的`QueryFileTagsEventBus`的查询。这意味着此查询正在使用`exclude.filetag`文件中指定的文件标记规则。
{{< /note >}}

您可以在`dev\Code\Framework\AzFramework\AzFramework\FileTag`目录中找到 FileTag API。在该目录中，`FileTag.h` 声明了上一个示例中使用的 `Match` 方法。还有其他方法，例如 `GetTags`，您可以使用它们编写更复杂的逻辑。您可能会发现使用 `FileTagComponent.h` 中的 `excludeFileComponent` 辅助类很有用。此组件类会自动为您加载默认排除文件，将文件标记类型设置为`FileTagType::exclude`，并在激活时连接到 `QueryFileTagsEventBus`。
