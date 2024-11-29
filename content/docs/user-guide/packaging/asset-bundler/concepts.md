---
description: ' 了解 O3DE Asset Bundler 术语和概念。Asset Bundler 有助于更高效地捆绑游戏发布所需的资源。 '
title: Concepts and Terms
weight: 700
---

以下术语和概念与 O3DE 资产打包器相关：

**Asset ID**
所有产品资产都分配有一个资产 ID。资产 ID 由源资产的 UUID 和唯一标识产品的子 ID 组成。Sub ID 通常由资产生成器生成。它们必须稳定才能在构建中运行，并且互斥。每个产品都应该有一个唯一的 sub ID，该 ID 不会更改。资产 ID 数据结构在`dev\Code\Framework\AzCore\AzCore\Asset\AssetCommon.h`中定义。另请参阅 **Source GUID**。

**Asset Browser**
Asset Browser 程序是 O3DE Editor 的一部分，可显示资源的当前状态。您可以使用此程序浏览一组资产，例如跟踪哪些资产已处理以及哪些资产将被处理。

**Asset Bundle**
资源包由分组到单个压缩文件中的产品资源组成。该捆绑包挂载在 O3DE 游戏中，以便可以在运行时加载和使用资产。资源包是`.pak`文件，使用`.zip`文件格式。

**Asset List**
资产列表包含产品资产的信息。此文件是使用 Asset Bundler 生成的。该信息包括列表中每个资产的指纹。指纹包括修改时间和文件哈希。可以从种子列表、单个种子生成资产列表，也可以通过对其他资产列表使用比较操作来生成资产列表。资源列表文件的文件扩展名为`.assetlist`。另请参见 **Seed** 和 **Seed List**。

**Builder**
资产生成器是一个 C++ 模块，用于将源资产转换为产品资产。例如，处理纹理的生成器也可能将源资产 PNG 文件转换为产品资产 DDS 文件。生成器会发出有关源资产和产品资产的元数据，例如依赖项。有关更多信息，请参阅 [创建自定义 Asset Builder](/docs/user-guide/assets/builder).

**Copy Job**
复制作业是一种资产处理任务，可将源资产复制到缓存而不对其进行修改。复制作业有两种方式：
+ 当您创作生成器时。如果生成器将源资产作为产品资产发出，则 Asset Processor 会将源复制到缓存中。
+ 使用`AssetProcessorPlatformConfig.ini`中定义的其他资产处理规则时。这些附加规则可以包括复制作业，并用于将许多文件类型复制到缓存中。
作为最佳实践，请为您的资产创建 Asset Builder，以便您可以添加产品依赖项。虽然将复制作业添加到`AssetProcessorPlatformConfig.ini`很容易，但以后很容易忘记更新依赖项的文件。

**Leaf Asset**
“叶”资产是不依赖于其他产品资产的产品资产。另请参阅 **Product Dependency**。

**Product Asset**
产品资产是在运行时游戏中加载和使用的游戏内容。产品资产是在资产处理期间使用生成器从源资产生成的。产品资产通常是源资产的修改版本，例如经过压缩并针对运行时进行优化的图像内容。产品资产引用其产品依赖项。另请参阅 **Source Asset**。
使用 O3DE 资产系统运行游戏时，只有产品资产可用。源资产不可用。这包括在 O3DE 编辑器或特定于平台的启动器中运行游戏。使用 Asset Bundler 打包游戏时，仅捆绑产品资源。产品资源只能从资源缓存或 Asset Bundle 中获得。
使用切片时，请记住，在运行时只能使用动态切片。非动态切片（即使它们是产品资产）也不应包含在捆绑包中。
有关资产生命周期的更多信息，请参阅 [使用 Asset Pipeline 和资产文件](/docs/user-guide/assets/).

**Product Dependency**
当一个产品资产引用另一个产品资产时，就会出现产品依赖关系。捆绑资源时，引用资源和引用的资源都应包含在捆绑包中。通过引用产品资产的资产 ID 创建依赖项。另请参阅 **Source Dependency**。
在资产打包和捆绑期间，产品依赖关系通常是相关的。建立产品依赖关系后，捆绑就变成了“发布游戏关卡 1 及其引用的所有内容”的任务。例如，您有一个通过动态切片生成的角色。此角色具有模型，模型具有材质，该材质具有纹理。在这种情况下，材质需要加载纹理才能工作，因此材质对纹理具有产品依赖性。模型需要加载材质才能工作，因此模型对材质具有产品依赖性。角色需要加载模型才能工作，因此角色的动态切片对模型具有产品依赖性。捆绑时，您可以包含字符，并期望包含所有依赖项。

**Runtime**
独立游戏称为运行时。它可以通过特定于平台的启动器或在 O3DE 编辑器中运行。运行时使用 O3DE 资产系统仅访问与游戏捆绑在一起的那些产品资产。

**Scan Folder**
在游戏运行时扫描 Scan 文件夹以查找资产。在`AssetProcessorPlatformConfig.ini`文件中指定了一组扫描文件夹。扫描文件夹是在 Asset Processor 启动时定义的。在运行时，无法添加、删除或重命名它们。

**Seed**
种子是添加到种子列表中的产品资产。将产品资产定义为种子可以提高捆绑资产或装载到游戏运行时的效率。在验证或调试资源包时，种子也很有用，例如在 Asset Validation Gem 中使用“种子模式”来跟踪资源依赖关系。另见 **Seed List**。

**Seed List**
种子列表是一个 `.seed`文件，其中包含一组种子，供捆绑系统放入资产列表中。种子列表还用于构建资产依赖关系图表。另请参阅**Asset List File**.
对于简单的游戏包，种子列表通常与关卡文件一起使用。例如，您有一个具有三个级别的游戏。级别 1 和 2 是生产级别，另一个是开发测试级别。您希望在发布游戏时包含两个生产级别以及它们引用的所有文件。您可以使用 Asset Bundler 创建`MyGame.seed`资产列表文件，然后将 1 级和 2 级产品资产作为种子添加到此文件中。

**Source Asset**
源资产是未与特定产品关联的资产的实例。使用资产时，您可以编辑源资产，然后使用资产生成器生成资产的特定产品实例。在创作编辑时代码时，您可能需要访问仅限编辑器的产品资源或源资源。如果您的逻辑实际上是在编辑文件，请使用源资源。如果您的逻辑仅读取文件，请在资源缓存中使用仅限编辑器的产品资源。源资源存储在源资源文件夹中，通常在 `dev\ProjectName\` 下。

**Source Dependency**
当一个源资源引用另一个源资源时，将发生源依赖关系。源资产通过引用源资产的 ID 来列出每个依赖项。另请参阅 **Product Dependency**。
通常，在编辑时创建内容期间和开发工具管道时，源依赖项是相关的。源依赖项用于以下场景：
+ 更改资产时，必须更新依赖于已更改资产的所有资产。

在资产所依赖的所有源资产完成处理之前，资产不会进行处理。
例如，您正在使用模型和材料。模型源资源依赖于材质源资源。但是，模型的产品资产不需要材料的相应产品资产。在这种情况下，必须先生成相关材质，然后才能处理模型源资产（即 FBX 文件），才能将产品资产输出为 CGF 文件。

**Source GUID**
源 GUID 用于标识资产。它是通过对源资产文件的路径（相对于 scan 文件夹）进行哈希处理生成的。从技术上讲，它是一个 UUID 值，但通常称为 GUID。另请参阅 **Asset ID**。
源资产与 Gem 关联，并且源 GUID 在 Gem 的资产集中是唯一的。如果来自不同 Gem 的源资源具有相同的相对路径、文件名和扩展名，则它们可以具有相同的 GUID。引用源 GUID 时，将为所有查询返回最高优先级 scan 文件夹中的文件，并将其复制到缓存中。

**Relative Paths**
某些 O3DE 工具操作采用相对路径作为输入。有两种类型的相对路径：
+ 缓存相对路径 - 告知各种系统在资源缓存中的何处查找预处理过的资源。每个使用缓存相对路径的 O3DE 工具都将相对于资产缓存的不同子目录。例如，来自绝对路径`C:\O3DE\dev\Cache\SamplesProject\pc\samplesproject\levels\samples\advanced_rinlocomotion\level.pak`的缓存相对路径（这不包括平台和项目）将是 `levels\samples\advanced_rinlocomotion\level.pak`。
+ 引擎根相对路径 - 指定位于`C:\O3DE\dev\`下目录中的文件。 例如，engine-root-relative path 允许您表示位置`C:\O3DE\dev\SamplesProject\textures\UIEditor_Sample\ButtonNormal.tif` 为 `SamplesProject\textures\UIEditor_Sample\ButtonNormal.tif`.
有关详细信息，请参阅 [资产 ID 和文件路径](/docs/user-guide/assets/pipeline/asset-dependencies-and-identifiers/)。
