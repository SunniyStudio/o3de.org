---
description: ' 编辑和更新默认依赖项文件，以确保游戏范围的资源始终打包为捆绑包的一部分。'
title: O3DE 项目的默认依赖项
weight: 600
---

在整个 O3DE 项目中，您将跨多个级别使用资产，或者找到需要包含的资产，无论它们是否是严格依赖项。为了处理这些用例，O3DE 支持 *默认依赖项文件*，它定义了在捆绑项目时始终需要的资产。Gem 还使用默认依赖项来确保始终包含其自己的关键资产。

默认依赖项为您提供了一种方便的方式来列出应作为整个项目的一部分捆绑在一起的资源，并且可以在生成任何资源列表时使用`--addDefaultSeedListFiles` 应用。当您将此参数用作资产捆绑器命令的一部分时，它会选取 O3DE 引擎、包含的 Gem 和您的项目的默认依赖项。

## 默认依赖项文件位置

有四个级别的默认依赖项文件可用。


****

| 默认依赖 | 文件路径 | 说明 |
| --- | --- | --- |
| Engine | Engine\\Engine\_Dependencies.xml | 为每个 O3DE 项目打包的依赖项。仅当使用同一安装创建多个项目时，才编辑此文件，这些项目需要包含特定资源，甚至基本游戏功能也依赖于这些资源。 |
| Gems | Gems\\gem\_name\\Assets\\gem\_name\_Dependencies.xml | 命名 Gem 所需的依赖项。创建新 Gem 时，请在此处包含所需的任何资源，无论它们是否在项目中明确使用。切勿编辑您未编写或自定义的 Gem 的默认依赖项文件。 |
| Project | project\_name\\project\_name\_Dependencies.xml | 项目范围的依赖项。这是您最常编辑的默认依赖项文件，应包括游戏范围的音频、在启动时预加载资源的配置信息或必须始终包含在项目中的其他资产等内容。创建新项目时，依赖项文件是从ProjectTemplates\\DefaultTemplate\\$\{ProjectName\}\\$\{ProjectName\}\_Dependencies.xml 模板创建的。 |

{{< important >}}
这些文件不是种子列表，不能使用修改种子列表的命令进行操作。默认依赖项由 Asset Processor 构建，用于在缓存中创建种子列表。然后，当您使用`--addDefaultSeedListFiles` 标志时，Asset Bundler 会选取这些种子列表。
 
这样做的另一个重要结果是，每次更改默认依赖项文件时，Asset Processor 都必须构建该文件，以便为 Asset Bundler 生成更新的种子列表。
{{< /important >}}

## 默认依赖项文件格式

默认依赖项文件以 XML 编写，仅包含两个元素： `EngineDependencies` 和 `Dependency`。默认依赖项文件的根元素应始终为 `<EngineDependencies version="1.0.0">`，包括 `version` 属性的这个特定值。`Dependency` 元素是 `EngineDependencies` 节点的唯一子元素，需要两个属性：`path` 和 `optional`. `Dependency` 元素没有子元素，包括没有文本内容。

如何处理每个`Dependency`节点取决于属性值：
+ `optional` - 此值为`true` 或 `false`，用于描述资产捆绑过程是否绝对需要列出的资产。 大多数情况下，您需要对没有通配符的资产路径使用 `true`，对包含通配符的路径使用`false`。
+ `path` - 此值是要包含的资产的路径，并接受使用“*”通配符。通配符以递归方式搜索所有子目录。

由于通配符匹配可能会捕获您不想作为默认资产包含的文件，因此您可以添加 *exclusion paths*： Dependencies，其中路径以 '：' 字符开头。排除路径中的资产不会作为默认依赖项包含在内。

以下是项目的默认依赖项列表示例，并进行了注释以明确每个节点处理哪些依赖项：

```
<EngineDependencies versionnumber="1.0.0">
    <Dependency path="libs/particles/preloadlibs.txt" optional="true" />        <!-- Include particle pre-loads if present -->
    <Dependency path="libs/gameaudio/wwise/*.xml" optional="false" />           <!-- Include all XML files under project_name\libs\gameaudio\wwise and its subdirectories... -->
    <Dependency path=":libs/gameaudio/wwise/levels/*.xml" optional="false" />   <!-- ...unless they're under project_name\libs\gameaudio\wwise\levels or its subdirectories. -->
</EngineDependencies>
```
