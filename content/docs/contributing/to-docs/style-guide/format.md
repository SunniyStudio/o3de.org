---
title: Formatting O3DE Documentation
description: A reference for all of the typesetting and formatting rules for the Open 3D Engine (O3DE) documentation.
linktitle: Formatting
weight: 300
toc: true
---

**Open 3D Engine （O3DE）** 文档使用 **Goldmark Markdown 语法**编写。[Goldmark](https://github.com/yuin/goldmark)是 [Hugo](https://gohugo.io/)使用的 Markdown 解析器，[Hugo](https://gohugo.io/) 是用于 o3de.org 的站点构建器。

使用 Markdown，有时有多种方法可以实现相同的结果。例如，您可以将单词括在下划线(`_`)或星号 （`*`） 中以创建斜体。在这些情况下，最好在整个文档中使用一种方法。要使文档 Markdown 源文件和 O3DE 文档页面演示保持一致，请遵循以下基本文档标准。

## 主题标题

使用一系列哈希 (`#`) 来定义章节标题。H1 标题取自元数据 `title` 元素，该元素显示为页面标题。对于元数据标题(`title`) 和目录标题 (`linkTitle`)，请使用标准大写，而不是句子大写。

章节标题应为 H2 (`##`) 标题，章节标题使用句首字母大写。

小节标题应以 H3 (`###`) 标题开头，小节标题使用句首字母大写。

**示例**:

```markdown
---
linkTitle: Page Title
title: An O3DE Documentation Page Title
description: A topic about an Open 3D Engine feature.
weight: 100
---

## H2 for the first section title (Sentence title capitalization)

### H3 for a sub-section title (Sentence title capitalization)

...

## H2 for the second section title (Sentence title capitalization)

### H3 for sub-section title (Sentence title capitalization)

#### H4 for sub-section of a sub-section title (Sentence title capitalization)

```


## 文本格式

在 Markdown 中，可以有多种方法实现相同的结果。请遵循以下标准，使 Markdown 源文件和页面文档更易于读者一目了然地解析。Markdown 源格式的一致性也有助于文档的自动化工作。

### 粗体文本

要加粗文本，请将文本括在双星号中 (`**`)。

**示例**:

```markdown
This is **bold** text.
```

**结果**:

This is **bold** text.

### 斜体文本

要将文本设为斜体，请将文本括在单星号中(`*`)。

**示例**:

```markdown
This is *italic* text.
```

**结果**:

This is *italic* text.

### 内联代码

要将文本格式设置为内联代码，请将文本括在一个反引号中 (`` ` ``)。

**示例**:

```markdown
This is `code` text.
```

**结果**:

This is `code` text.

### 代码块

对多行代码使用代码块。某些语言支持语法高亮显示。语言在代码块的左反引号(` ``` `)之后指定。

编写代码块时，请确保包含语言标识符。对于 C++ 代码块，请使用 `cpp` 标识符。参见 Hugo 的 [色度高亮语言列表](https://gohugo.io/content-management/syntax-highlighting/#list-of-chroma-highlighting-languages)
 以获取其他语言标识符。

**示例**:

````none
```python
# Use the 'request' find the type of job via 'jobKey' to determine what to do
def on_process_job(args):
    try:
        # Get request information
        request = args[0]
         ...
```
````

**结果**:

```python
# Use the 'request' find the type of job via 'jobKey' to determine what to do
def on_process_job(args):
    try:
        # Get request information
        request = args[0]
         ...
```

{{< note >}}
代码块中的语法突出显示应符合 [对比准则](https://www.w3.org/WAI/WCAG21/quickref/?versions=2.0&showtechniques=141%2C143#contrast-minimum)
{{< /note >}}


### 链接

#### 相对和绝对链接

写入不带文件扩展名`.md`的链接。

|相对链接 |结果 |
| - | - |
| `[...](./)` |返回到当前目录的索引页。 |
| `[...](page-linked-from-index)` | 从索引页到索引目录中的页的链接。 |
| `[...](./page-linked-from-non-index)` | 从任何页面链接到同一目录中的页面。 |
| `[...](forward-directory-linked-from-index/)` | 从索引页到索引的子目录的链接。 |
| `[...](./forward-directory-linked-from-non-index/)` | 从任何页面到当前页面的子目录的链接。 |
| `[...](../)` | 返回到上一个目录的索引。|
| `[...](../link-to-page-in-previous-directory)` | 从任何页面链接到上一个目录中的页面。 |
| `[...](link#subheading)` | 链接到主题中的副标题。 |
| **绝对路径** | **结果** |
| `[...](/docs/guide/link-to-page)` | 链接到文档中的页面。 |


#### 外部链接

谨慎和谨慎地使用外部链接。仅链接到值得信赖和受人尊敬的网站。避免添加不必要的链接。

对于第三方产品，您必须提供指向来源的外部链接。仅对第一个 on-page 实例或最相关时执行此操作。

有关补充信息，请考虑是否需要外部链接。提供简短的解释可能就足够了，并且是首选，因为它使用户专注于当前主题。



### 引号和标点符号位置

当引用包含在句子中时，请将标点符号放在引用之外。

当引用是一个完整的句子时，请将标点符号放在引用内。

类型 |例
:--| :-----
Quote 在句子中 | ... “游戏就绪”的资源**.**
Quote 是一个完整的句子 | _"Focus is a matter of deciding what things you're not going to do **.** "_ <br>- John Carmack



## 信息结构

### 表格

**示例**：

```markdown
| Default column| Right-aligned column | Center-aligned column | Left-aligned column |
| - | -: | :-: | :- |
| Row | entry | entry | entry |
| Row | entry | entry | entry |
| Row with missing entry | entry | | entry |
```

**结果**:

| Default column| Right-aligned column | Center-aligned column | Left-aligned column |
| - | -: | :-: | :- |
| Row | entry | entry | entry |
| Row | entry | entry | entry |
| Row with missing entry | entry | | entry |

### 标签

**示例**：


```
{{</* tabs name="name-for-this-group-of-tabs" */>}}
{{%/* tab name="First tab" */%}}

First tab's content.

{{%/* /tab */%}}
{{%/* tab name="Second tab" */%}}

Second tab's content.

{{%/* /tab */%}}
{{</* /tabs */>}}
```


**结果**:

{{< tabs name="tabs-example" >}}
{{% tab name="First tab" %}}

First tab's content.

{{% /tab %}}
{{% tab name="Second tab" %}}

Second tab's content.

{{% /tab %}}
{{< /tabs >}}


### 列表

如果列表长度超过 4 项，或者列表包含标注短代码或图像，请在每个列表元素之间添加换行符以提高可读性。

#### 有序列表

当项目的顺序很重要时，请使用有序列表，例如顺序步骤的过程。为方便起见，您可以使用 '1.' 来描述有序列表中的所有项目。Goldmark 会自动对列表中的项目进行编号。

**示例**：

```markdown
1. Step one
1. Step two
1. Step three
1. Step four
```

**Result**:

1. Step one
1. Step two
1. Step three
1. Step four

#### 无序列表

如果项目的顺序是任意的，则使用无序列表，例如资产列表。

您可以使用 `*` 或 `-` 来描述无序列表中的项目。无论您使用什么，请在整个列表中保持一致。

**例**：

```markdown
* Item one
* Item two
* Item three
```

**结果**:

* Item one
* Item two
* Item three

#### 嵌套列表

嵌套列表通常用于过程中的子步骤或需求列表。缩进四个空格以嵌套列表。

对于步骤中的代码块，请再次缩进代码块，使其越过步骤的缩进。

对于步骤中的短代码，同样地将开始和结束短代码括号缩进到步骤的缩进之后。但是，请注意不要缩进包含的文本，因为它会导致短代码中的代码块。

**示例**：

````markdown
1. Step one
1. Step two
    * Item one
    * Item two
    * Item three
1. Code example
    ```
    A line of code or command
    ```
1. Callout example
    {{</* note */>}}
  A callout box.
    {{</* /note */>}}
````
**结果**:

1. Step one
1. Step two
    * Item one
    * Item two
    * Item three
1. Code example
    ```
    A line of code or command
    ```
1. Callout example
    {{< note >}}
  A callout box.
    {{< /note >}}

#### 定义列表

对列出一对术语及其定义的内容使用定义列表。例如，词汇表。

使用`:` 来描述列表中的每个定义。
  
**示例**：

```markdown
First Term  
: This is the definition of the first term.

Second Term  
: This is one definition of the second term.
: This is another definition of the second term.
```

**结果**:

First Term
: This is the definition of the first term.

Second Term
: This is one definition of the second term.
: This is another definition of the second term.


## 术语

### 将新术语斜体化

执行 |不要
:--| :-----
... to create *image based lighting (IBL)*. | ... to create **image based lighting (IBL)**.
A *prefab* is a collection of entities ... | A "prefab" is a collection of entities ...

### 商标

根据商标标题和术语的来源使用情况，正确设置商标标题和术语的格式。在第一个页面实例或最相关时提供指向源相关材料的链接。

## 应用程序、工具、Gem 和组件

### 大胆的应用程序和工具

对于应用程序和工具的第一个页面引用，请使用 **粗体** 文本。使用未格式化的文本进行后续引用。

对于以脚本形式提供的工具，请使用 `code style` 作为脚本名称，后跟无格式文本的脚本类型。对整个页面中的所有实例执行此操作。

类型 |例
:--| :-- 
Application | **O3DE Editor** is the primary development environment for .... Open O3DE Editor by launching it from...
Tool | The `o3de` Python script allows you to... To use the `o3de` Python script... | 

### 大胆的 Gems 和组件

对于 Gem 和组件的第一个页面引用，请使用 **粗体** 文本。使用未格式化的文本进行后续引用。 

**附加规则** 
： - 对于 Gem，将 Gem 的名称和单词 “Gem” 大写并 **粗体** 。
  - 对于组件，将组件的名称大写并 **粗体** 。对单词 “component” 使用小写和无格式的文本。

类型 |例
:--| :-- 
Gem | The **Multiplayer Gem** provides... Use the Multiplayer Gem to...
Components | The **Material** component adds... Also, you can use the Material component to...


## 用户界面、输入和热键

用户通过各种用户界面 （UI） 元素、输入和热键与 O3DE 交互。它们通常包含在任务和教程中，指示用户执行操作。

对于 UI 元素、输入和热键，请使用 **粗体** 文本。对整个页面中的所有实例执行此操作。

**附加规则** 
：对于格式化键：缩写键名称、删除空格并使用句子大小写。

类型 |示例 |避免
:--| :-- | :--
UI element | Choose **Edit**. | Click "Edit".
UI element | .. the **Play** button. | ... the Play button.
Input | Press **Enter**. | Press "Enter".
Input | .. **right-click** the asset name ... | ... right-click the asset name ...
Hotkey | Hold **Ctrl+Shift** ... | Hold `Control + Shift` ...


## 代码、命令和 API

### 内联代码和命令的代码样式

执行 |不要
:--| :-----
... set `enable_memory_tracking = True`. | ... set **enable_memory_tracking** = "True".
... enter the command `dump_vars`. | ... enter the command dump_vars.

### 编程对象的代码样式

执行 |不要
:--| :-----
... value of the `sys_maxfps` field. | ... value of the "sys_maxfps" field.
... use the `WorldRequestBus`. | ... use the "WorldRequestBus".
... in `AZ::Data::AssetData` derived classes. | ... in **AZ::Data::AssetData** derived classes.

### 粗体属性名称，并为其值设置代码样式

执行 |不要
:--| :-----
Set the **Color** property to `255,0,0`. | Set the Color property to "255,0,0".
For **Intensity Mode**, select `Candela`. | For `Intensity Mode`, select Candela.
Valid **Mass** values range from `0` to `Infinity`. | Valid Mass values range from `0` to Infinity.

### 不要包含命令提示符

执行 |不要
:--| :-----
`cmake --build ...` | `C:\> cmake --build ...`

### 将命令与输出分开

```shell
cmake --build <MyProject> --target Editor --config profile -- -m
```

输出类似于：

```console
Microsoft (R) Build Engine version 16.9.0+57a23d249 for .NET Framework
Copyright (C) Microsoft Corporation. All rights reserved.

  Checking Build System
  Building Custom Rule D:/O3DE/Code/Framework/AzCore/CMakeLists.txt
  unity_16_cxx.cxx
  unity_20_cxx.cxx
  unity_19_cxx.cxx
  unity_24_cxx.cxx
  unity_23_cxx.cxx
```

## 文件、目录和路径

### 文件名、目录和路径的代码样式

所有路径都应与平台无关，并使用 '/' 路径分隔符。使用相对路径时，请为读者提供上下文以了解路径的相对内容。

执行 |不要
:--| :-----
Open the project's `project.json` file. | Open the project's project.json file.
... in the `/<project>/levels` directory. | ... in the /\<project\>/levels directory.
Open the `/<project>/game.cfg` file. | Open the /\<project\>/game.cfg file.

### 用尖括号标记占位符

对占位符使用尖括号。使用括号内的文本告诉读者占位符代表什么。

**示例**：

```shell
git push origin <your-branch-name>
```

