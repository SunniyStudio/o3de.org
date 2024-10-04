---
linkTitle: 元数据重定位系统
title: 元数据重定位系统
description: 描述了促进Open 3D Engine (O3DE)元数据重定位系统的类，该系统可在不中断引用的情况下移动或重命名资产文件。

weight: 100
---

## 它是什么

元数据重定位允许在不中断引用的情况下移动/重命名文件。 它将源资产的 UUID 保存在源文件旁边的 .meta 文件中。 通过移动/重命名元数据文件，资产处理器（AP）可以始终跟踪 UUID。

设计文档: https://github.com/o3de/sig-content/blob/main/rfcs/rfc-77-metadata-asset-relocation.md

用户文档: **TBD**

## 为何需要
内容创建者经常希望重命名和移动文件，以改进项目组织和修正错别字。但是，重命名和移动文件也存在挑战：内容往往会引用其他内容，重命名或移动一个文件可能会破坏其他文件对该文件的引用。元数据重定位可通过 UUID 稳定资产识别，该 UUID 在文件移动和重命名时保持不变，这样文件引用就能自动处理该文件操作。

## 为什么使用旁证文件
通过旁证文件稳定资产 ID，为资产重新定位提供了一个低摩擦的工作流程，可以在 O3DE 工具之外完成，包括团队可能已经在使用的源控制工具。其他解决方案要么涉及留下面包屑文件或面包屑信息，这可能会成为长期维护的头痛问题，要么在每次重新定位或重命名文件时尝试进行参考修复，这就需要定制工具来管理参考修复。在这两种情况下，内容创建者仍可能在不使用这些系统的情况下移动文件，从而破坏引用。旁证文件是可见和可发现的，因此内容创建者更有可能使用旁证文件移动资产，从而使项目中资产之间的引用保持长期稳定。

## 高级设计

### 系统的关键部分：

* `AzToolsFramework::MetadataManager` - 处理元数据文件的创建，并提供向元数据文件读/写任何通用值的 API。
* `AzToolsFramework::UuidUtilComponent` - 提供了一个为文件分配 UUID 的小型应用程序接口。 任何工具都可以使用它，在创建资产前为资产分配 UUID。
* `AssetProcessor::UuidManager` - 为资产申请 UUID 的中心位置。 为还没有元数据的资产分配 UUID，以及从现有元数据文件中读取 UUID。 还可用于查询传统的 UUID。
* `AssetProcessor::AssetCatalog::BuildRegistry` - 主要是迭代所有资产并调用 GetUuid，从而加载和缓存所有元数据文件。 此外，它还负责更新数据库，以处理源、源依赖或产品依赖表中任何过时的 UUID。
* `AssetProcessor::ProductOutputUtil` - 处理产品重命名（以扫描文件夹 ID 为前缀），避免因多个文件具有相同的相对路径而导致磁盘上的文件名冲突。
* `AssetProcessor::SourceAssetReference` - 类，用于传递源资产路径，使消费系统能轻松访问相对路径、绝对路径和扫描文件夹 ID。 UUID 元数据允许多个文件具有相同的相对路径，因此仅用相对路径来引用文件已不再足够。

系统的设计允许按类型为资产启用基于元数据的 UUID。 因此，只有启用的资产类型才会生成 UUID 并保存在元数据文件中。 对于未启用的类型，UuidManager 将返回旧的基于路径的 UUID（除非该文件已经存在元数据文件，在这种情况下，它仍将使用元数据 UUID）。

元数据文件存储了随机生成的规范 UUID、所有传统 UUID 的列表（使用以前的生成方法）、元数据文件创建时间的时间戳（用于解决具有相同传统 UUID 的两个文件之间的冲突，较早的文件获胜）和原始文件路径。

要完全支持一种文件类型，就意味着每个能生成该类型文件引用的系统都必须使用 UUID（通常是通过 `Asset<T>` 引用）。 基于路径的引用不起作用。

## 单元测试

每个系统都有多个单元测试。 大部分都可以在这些地方找到：

* Code\Tools\AssetProcessor\native\tests\assetmanager\
    * DelayRelocationTests
    * AssetProcessorManagerTest
    * SourceDependencyTests
* Code\Tools\AssetProcessor\native\tests\UuidManagerTests
* Code\Framework\AzToolsFramework\Tests\Metadata\
    * MetadataManagerTests
    * UuidUtilTests

## 文件支持

以下是引擎支持的大多数文件类型的粗略列表，根据对其依赖关系的分析，按可重置性进行了分类。 测试和更深入的分析可能会发现其中一些类型需要更多的工作才能完全支持。

### 可能已经运行的文件
对这些文件的所有引用似乎都是通过 AssetID 完成的。 为这些源资产的产品资产生成的子 ID 似乎是稳定的，而不是基于源资产名称。

* animgraph
* azasset
* dat
* emfxworkspace
* exr
* filetag
* inputbindings
* motionset
* names
* pass
* pem
* physxconfiguration
* physxmaterial
* postfxlayercategories
* precompiledshader
* resourcepool
* scriptcanvas
* scriptevents
* stars
* surfacetagnamelist
* tfx
* ts
* uicanvas
* vegdescriptorlist
* setreg

### 需要做一些工作才能支持的文件
要完全支持这类资产的资产搬迁，需要对这些资产的使用进行一些改进。支持此类文件的工作包括 稳定产品资产子 ID，这样当文件重命名时，产品资产的子 ID 就不会发生变化，并更改其他资产对这些资产（或其产品资产）的引用，使用资产 ID 而不是相对路径。

* azshadervariant
* shader, materialpipeline, template
* material
* prefab
* fbx
* font, fontfamily
* ttf, otf
* sprite
* texture file types (jpg, png, dds, tif, etc)

### 未知
这些文件需要进一步分析，以确定支持是否有用或可行，以及需要多少工作（如有）才能完全支持它们。

* materialtype - 被 FBX 文件引用 - 怀疑这是基于代码的依赖关系
* shader, attimage - 由二进制pass文件和 shadervariantlist 引用
* tfxbone, tfxmesh - 由二进制 TFX 文件引用

* pak - 可被代码引用？ 遗留资产
* shadervariantlist - 由 shadervariantlist 引用。 着色器文件的相对路径，JSON 格式

### 过于通用
这些文件扩展名往往被各种不同类型的内容所使用，这意味着基于文件扩展名的重置支持方法在这里无法完全奏效。例如，XML 文件可以描述多种不同类型的内容，其中有些内容比其他内容更容易处理资产重置。要支持这类内容的重置，最简单的方法就是让内容类型使用特定的文件扩展名，而不是这些通用扩展名。例如，尽管预制件文件是 JSON 格式，但它们没有 JSON 扩展名。

* xml - 由uicanvas引用
* json - 由 materialtype引用
* txt - 没有现成的引用
* ini - 没有现成的引用，但可能被代码引用

### 不建议支持
在这些文件中，用户通常希望在文件之间使用基于路径的引用，而辅助元文件会被视为杂乱无章，而不是稳定这些路径的有用工具。例如，像 Python 这样的代码文件已经有了引用其他 Python 文件的既定模式，因此尝试使用元文件来稳定这里的引用是没有意义的。
* lua - 源代码文件，包含对其他 Lua 文件的文本引用

* py - 源代码（也被 FBX 文件引用）
* azsl, azsli, srgi - 着色器文件作为简单 JSON 数据文件中的相对路径字符串被引用
