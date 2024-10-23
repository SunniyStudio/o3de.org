---
linkTitle: 产品资产
title: 产品资产
description: 产品资产是 O3DE 项目中可随时运行的内容形式。
weight: 400
toc: true
---

**产品资产**是 O3DE 项目中内容的运行准备形式。

[资产处理器](/docs/user-guide/assets/assets/asset-processor/)使用[资产创建器](/docs/user-guide/assets/pipeline/asset-builders/) 从[源资产](/docs/user-guide/assets/pipeline/source-assets/)生成产品资产。

本页介绍从资产生成器生成产品资产的最佳实践。

# 确定性生成

最佳做法是确定性地生成产品资产。使用相同版本的资产生成器生成相同的源资产，每次生成的产品资产应该完全相同。这既适用于本地的同一贡献者，也适用于 O3DE 项目的所有贡献者。这样做有几个原因。

* 资产引用稳定性--如果产品资产对于项目的所有贡献者都是相同的，那么调试产品资产的问题就会更容易。
* 重新处理资产所耗费的时间 - 使用子 ID 声明与特定产品相匹配的作业依赖关系会导致每次产品更改时都要重新处理作业。建立该系统的目的是为了节省重新运行作业所花费的时间，只在需要运行作业时才运行。每次运行任务时都会更改的产品资产会规避该系统。
* 最小化资产捆绑大小 - [资产捆绑](docs/user-guide/packaging/asset-bundler/)是生成可发布版本构建过程中的一个步骤。资产捆绑的一个主要特点是使用产品资产哈希值来跟踪哪些产品资产自上次发布发布版构建版以来发生了变化。如果产品资产在每次生成时都发生变化，就会导致补丁上的资产捆绑包括自上次发布以来可能未进行过有意义修改的内容。

# 排放产品依赖项

[产品依赖关系](/docs/user-guide/assets/pipeline/asset-dependencies-and-identifiers/#product-dependencies) 有两个功能目的： 它们在资产加载过程中使用，以确保根据初始资产加载请求的依赖关系图加载必要的资产；它们在 [资产捆绑](docs/user-guide/packaging/asset-bundler/)过程中使用，以确保资产包包含所有相关资产。

# 调试输出标志

调试输出设置是资产创建器可以检查的一个标志，用于改变它们的输出方式和内容。它最常用于输出人类可读的产品资产信息，以协助调试资产创建器和产品资产的问题。

调试标记可通过作业请求中作业描述的作业参数访问： `request.m_jobDescription.m_jobParameters.find(AZ_CRC_CE("DebugFlag"));`

举例来说，当设置此标记时，场景生成器将生成多个调试输出文件。这些额外的产品资产文件既包含人类可读的场景图信息，也包含自动测试可轻松读取的格式化信息。

运行资产处理器时，可以通过在命令行中加入`--debugOutput`，或在资产处理器图形用户界面的 Settings页面上切换"Debug Output" 来设置该参数。

# 子 ID 生成

子 ID 是由源资产生成的特定产品资产的稳定标识符。产品资产的子 ID 与相关源资产的 UUID 相结合，定义了该产品资产的资产 ID。产品资产最好通过该资产 ID 进行引用。详见[引用其他产品资产](#Referencing-other-product-assets)。

在源资产发生变化时，稳定产品资产使用的子 ID 非常重要。这是因为子 ID 用于引用产品资产，因此如果子 ID 发生变化，引用就会中断。

# 引用其他产品资产

产品资产在运行时引用其他产品资产是很常见的。例如，材质引用图像文件作为纹理。

在决定如何跟踪产品资产中的这些引用时，资产 ID 将是最稳定的。资产 ID 是 UUID 形式的源资产标识符和用于跟踪特定产品资产的子 ID 的组合。当资产重命名或移动时，资产 ID 可以保持稳定不变。这意味着，如果一个产品资产通过资产 ID 引用了另一个产品资产，那么即使被引用的资产被重新定位，该引用也会保持稳定。您可以阅读有关资产重新定位的更多信息 [此处](/docs/user-guide/assets/pipeline/metadata/)。

您可以阅读更多有关从其他产品资产引用产品资产的信息 [此处](/docs/user-guide/assets/pipeline/asset-builders/#references-from-product-assets-to-other-files)。


