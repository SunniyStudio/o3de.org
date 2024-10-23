---
linkTitle: 资产生成器
title: 资产生成器
description: 资产生成器使用可配置的规则，从Open 3D Engine (O3DE)中的 Asset Processor 发送的作业中生成产品资产。
weight: 400
toc: true
---

**资产生成器**是**资产处理器**运行以生成产品资产的代码包。资产生成器可作为 Gem 发布，并可能使用第三方库来处理资产。例如，**Scene Processing** Gem包含处理`.fbx`文件的资产生成器，并使用[Open Asset Import Library](https://github.com/assimp/assimp)来解析`.fbx`文件。

{{< note >}}
开放式资产导入库支持[多种场景格式](https://github.com/assimp/assimp/blob/master/doc/Fileformats.md)。您可以编辑 `o3de/Registry/sceneassetimporter.setreg` 并在 `“SupportedFileTypeExtensions”`列表中添加格式扩展，尝试使用其他格式。
{{< /note >}}

## 资产创建器剖析

资产创建器有三个核心组件：用于创建器的**描述符**，以及用于**创建作业**和**处理作业**请求的处理程序。

![Simple Job](/images/user-guide/assets/pipeline/asset_builders/asset_builder_basics.png)

### 描述符

描述符为资产处理器提供识别资产生成器所需的信息，并声明资产生成器可处理的源文件类型。描述符包含受支持源资产的文件模式、通用唯一标识符（UUID）和版本号。资产处理器使用描述符将源资产筛选到相应的资产生成器。

资产生成器 UUID 还用于为其生成的产品资产创建子 ID。产品资产的子 ID 必须与其资产生成器的当前 UUID 一致。更改资产创建器的 UUID 会触发重新处理之前由资产创建器处理过的所有源资产。

### 创建任务

创建工作会为资产处理器生成资产处理工作。当资产处理器检测到新的或更新的源资产，并确定合适的资产生成器来处理源资产时，它会向资产生成器发送包含源资产信息（包括其路径）的`CreateJobsRequest`。资产创建器会回应一个`CreateJobsResponse`，其中包含`JobDescriptor`结构以及源和作业依赖关系。例如，如果`CreateJobsRequest`是要处理一个素材，那么 Asset Builder 就会在`CreateJobsResponse`中包含对所引用着色器源资产的依赖，以确保着色器在素材之前得到处理。

{{< note >}}
创建工作是一个单线程流程。在某些情况下，你可能会实现一个资产创建器，对其他资产创建器也支持的源资产类型进行专门处理。您可能需要检查源资产，以确定专门的资产创建器是否应处理源资产。在这种情况下，将检查资产作为流程作业的一部分，并根据结果提前退出流程，可以提供更好的性能，因为流程作业是多线程的。
{{< /note >}}

### 处理任务

处理任务生成产品资产和产品依赖关系。资产创建器从资产处理器接收到一个 `ProcessJobRequest`，其中包含要处理的源资产信息。资产生成器会回复一个 `ProcessJobResponse` 。`ProcessJobResponse`的功能是处理源资产并返回其创建的产品资产信息，包括子 ID 和产品依赖关系。

`ProcessJobResponse` 会将产品资产放入临时目录。资产处理器将产品资产移动到**资产缓存**，并在**资产数据库**和**资产目录**中填充用于跟踪产品资产、产品依赖关系和生成这些资产的作业的信息。

流程作业是多线程的。多个资产生成器可同时运行多个流程作业。


### 设置
在为生成器开发新功能或调试资产管道中的问题时，可以在调试模式下运行 Asset Builder。在调试模式下，Asset Builder 会创建测试作业或处理指定文件的作业。

{{< note >}}
在输入 AssetBuilder 的 -debug 命令之前，必须先启动 Asset Processor。
{{< /note >}}

使用资产生成器进行调试：
* 使用资产处理器设置说明，只使用一个资产创建处理器：
* 导航至构建文件夹目录，例如 `build\windows\bin\profile`
* 在命令行提示符下输入以下命令，以获取可能的选项列表： `AssetBuilder -help`
* 或者使用以下设置来帮助调试 AssetBuilder，例如 `AssetBuilder.exe --debug C:\o3de\o3de\Assets\Editor\Materials\ShaderList.xml --platform pc --tags dx12 --project-name AutomatedTesting --project-cache-path C:\o3de\o3de\AutomatedTesting\Cache`

| 设置                 | 说明                                                                                                                               |
|--------------------|----------------------------------------------------------------------------------------------------------------------------------|
| help               | 打印此帮助信息。                                                                                                                         |   
| task               | 要运行的任务。                                                                                                                          |
| project-name       | 当前项目的名称。                                                                                                                         |
| project-cache-path | 项目缓存文件夹的完整路径。                                                                                                                    |
| module             | 对于常驻模式，是生成器 dll 文件夹的路径，否则是要使用的单个生成器 dll 的完整路径。                                                                                   |
| port               | 可选项，用于连接资产处理器的端口号。                                                                                                               |
| id                 | 标识生成器的 UUID 字符串。 仅用于资产处理器直接启动资产创建器时的常驻模式。                                                                                        |
| input              | 对于非驻留模式，包含序列化作业请求的文件的完整路径。                                                                                                       |
| output             | 对于非驻留模式，要写入任务响应的文件的完整路径。                                                                                                         |
| debug              | 指定文件创建和处理任务的调试模式。 <br/>调试模式可选择使用 `-input`, `-output`, `-module`, `-port` 和 `-gameroot`。例如：`-debug Objects\Tutorials\shapes.fbx`。 |                                                                                                                                  
| debug_create       | 指定文件创建任务的调试模式。                                                                                                                   |
| debug_process      | 指定文件进程任务的调试模式。                                                                                                                   |
| tags               | 添加到调试平台的附加标签，用于任务处理。每个选项可提供一个标签。                                                                                                 |
| platform           | 用于调试的平台，例如 `pc`                                                                                                                  |

## 加载其他文件

有时，在创建生成器时，除了主要源文件外，可能还需要加载其他文件来完成特定流程。

### 了解文件位置

![File Locations](/images/user-guide/assets/pipeline/asset_builders/asset_builder_file_locations.png)

与资产处理相关的文件可能存在于这些位置：
1. 资产缓存是所有产品资产的存在位置。
1. 扫描目录是源资产的唯一存在位置。
1. 源文件（不是资产）可以存在于任何位置的任何驱动器上。

[扫描目录](/docs/user-guide/assets/pipeline/scan-directories/)由资产处理器监控，以查找新的和更新的源资产。扫描目录可由用户定义，但主要扫描目录位于以下位置：
1. 当前项目的目录。
1. 为项目启用的每个 Gem 的 `Assets`文件夹。

单个Gem、活动的 O3DE 项目和引擎本身都可以安装到不同的驱动器中。

#### 资产处理中的文件类型

![All files in asset processing](/images/user-guide/assets/pipeline/asset_builders/asset_builder_all_dependencies.png)

* 源资产
   * 源资产是存在于 O3DE 项目扫描目录中的文件，与资产创建器的注册模式相匹配，在调用 “创建作业 ”时会创建作业，并带有资产创建器的源资产。
   * 点击此处阅读有关 [源资产](/docs/user-guide/assets/pipeline/source-assets/) 的更多信息。
* 非资产源文件
   * 非资产源文件是在处理源资产时加载和引用的文件，可能在扫描目录中，也可能不在扫描目录中。
   * 资产源文件与非资产源文件的区别在于：
     * 文件不在扫描目录中、
     * 文件与任何资产创建器描述符的文件模式不匹配，或
     * 在为匹配的资产创建器调用创建任务时，文件没有生成任务
* 中间资产
   * 中间资产是作为资产处理工作的产物而生成的源资产。
   * 点击此处了解更多关于 [中间资产](/docs/user-guide/assets/pipeline/intermediate-assets/)的信息。
* 产品资产
   * 产品资产是资产处理任务的运行准备输出。

### 源资产和非资产源文件对其他文件的引用

![Source asset references](/images/user-guide/assets/pipeline/asset_builders/source_asset_references.png)

源资产和非资产源文件可以多种不同方式相互引用，在创建资产创建器时，每种情况都可能需要独特的处理方式。

源资产和非资产源文件可能有这些引用：
* 按 UUID 列出的源资产
* 通过路径引用源资产
  扫描目录中的非资产源文件（按路径） * 扫描目录外的非资产源文件（按路径
* 扫描目录外的非资产源文件，路径
* 产品资产，按资产 ID
* 产品资产，按路径

资产源文件和非资产源文件的路径引用可以通过以下方式定义：
* 相对于扫描目录
* 相对于带有引用的资产
* 绝对
* 相对于难以从源资产的内容或位置推断出的目录

相对于这些引用的工作方式，源资产有两种形式：
* 创建者可以修改和控制的格式。
  * 举例说明： 为 O3DE 定义的自定义格式，如 prefab 文件。
* 创建者无法控制的格式。
  * 例如：在 O3DE 之外定义的格式： 在 O3DE 之外定义的格式，如 FBX 文件。

在制作生成器时，如果源资产的格式是作者可以控制的，那么管理源资产如何引用其他文件就非常有用。
* 在引用源资产或产品资产时，最好使用 UUID 或资产 ID，因为它们比路径更稳定。点击[此处](/docs/user-guide/assets/pipeline/asset-dependencies-and-identifiers/#asset-identifiers)了解更多关于资产标识符的信息。

通常是在资产创建器中解决路径问题，以便在磁盘上找到文件。在处理从源资产到其他文件的路径引用时，请注意以下事项：
* 被引用的资产可能与引用资产不在同一驱动器上。这包括扫描目录，它们可能位于不同的驱动器上。这意味着相对路径可能无法解析。
* 有些操作系统的路径区分大小写，有些则没有。在资产生成器中解析路径时，重要的是要在不同操作系统中保持稳定。
* 与扫描目录相对的文件位置以及资产缓存中的文件位置对于团队中的所有成员来说都是稳定的，但扫描目录本身以及扫描目录之外的文件对于团队中的所有成员来说可能不具有相同的路径。
* 路径解析逻辑应该是一致的、可预测的。内容创建者希望了解处理资产时如何解析路径，以便更好地管理内容。

### 从产品资产到其他文件的引用

![Product asset references](/images/user-guide/assets/pipeline/asset_builders/product_asset_references.png)

产品资产只能有这些引用：
* 通过资产 ID 引用产品资产
  * 这是引用其他产品资产的首选方式，应尽可能避免按路径引用
* 产品资产通过资产的相对路径进行引用
* 产品资产通过相对路径指向资产缓存中的特定根目录，通常是平台的缓存根目录
* 项目目录中的非产品文件，预计位于特定位置
  * 这种引用并不常见，但在某些情况下，可能需要引用资产文件夹之外的文件，而该文件将出现在最终产品中。

虽然在技术上可以在产品资产中以其他方式引用文件，但最终可能会导致问题。运行与资产处理器连接的编辑器或游戏启动器的项目开发版本，是团队最习惯使用的交互方式。但是，当准备向最终客户交付一个项目时，由于内容已打包和捆绑，项目的布局和资产的放置将有所不同。在开发过程中，产品资产对其他文件的引用在项目发布版本中可能不起作用。例如，如果产品资产引用了源资产，那么在打包的发布构建中，源资产很可能不会出现在最终用户的机器上。

产品资产绝不能以绝对路径引用其他产品资产，因为这将导致生成产品资产的每台机器的产品资产内容的哈希值都是唯一的。在创建生成器时，应牢记项目资产管理的整个生命周期。保持产品资产在不同机器上的稳定性，可以确保为实时游戏生成补丁时收集修改资产步骤的准确性。您可以在此阅读更多有关[资产捆绑](/docs/user-guide/packaging/asset-bundler/overview/)的信息.

### 在流程作业中加载产品资产

如果一项工作的处理需要加载另一项工作的输出产品资产，则应针对该工作声明工作依赖关系，并尽可能在该依赖关系中定义特定产品。

在设置资产引用时，具有输出引用的源资产和它所引用的产品资产可能位于两个不同的驱动器根目录上，因此源资产和被引用的产品资产之间可能无法建立相对路径。

点击此处了解更多有关 [工作依赖性](/docs/user-guide/assets/pipeline/asset-dependencies-and-identifiers/#job-dependencies) 的信息。

### 在创建工作时加载产品资产

不建议在另一项工作的创建工作步骤中加载一项工作的产品资产。这是因为没有直接的工作流程来实现这一点。

如果可能，建议从其他作业的源资产而不是产品资产中加载所需的信息。然后，源依赖关系可用于确保作业按正确的顺序运行。

如果无法做到这一点，那么中间资产就可以提供管理这种处理顺序的途径。为此，应更新初始作业以输出中间资产，将其用作执行实际工作的作业的源资产。创建该中间资产的新作业应在中间作业的创建作业功能中加载产品资产，将其作为特定产品的作业依赖关系。

在此阅读有关 [中间资产](/docs/user-guide/assets/pipeline/intermediate-assets/) 的更多信息。在此阅读有关 [工作依赖项](/docs/user-guide/assets/pipeline/asset-dependencies-and-identifiers/#job-dependencies) 的更多信息。

如果您需要在资产处理的创建工作步骤中加载产品资产，建议您与 O3DE 社区讨论您的用例。请创建[票据](https://github.com/o3de/o3de/issues)或在[O3DE Discord](https://{{< links/o3de-discord >}})中联系我们。

### 加载其他源资产或非资产文件

有时，在处理源资产时，无论是创建作业还是处理作业步骤，都需要加载第二个源资产或源文件。在这种情况下，应使用源依赖关系。

源依赖关系在创建作业步骤中声明，当该依赖关系中的文件发生变化时，作业将重新运行。

点击此处了解更多有关 [源代码依赖性](/docs/user-guide/assets/pipeline/asset-dependencies-and-identifiers/#source-dependencies) 的信息。
