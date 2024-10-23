---
linkTitle: 中间资产
title: 中间资产
description: 在Open 3D Engine (O3DE)中，中间资产允许将构建器连锁在一起，并为资产处理增加了离散步骤，从而提高了构建器的可重用性。
weight: 600
toc: true
---

中间资产允许将构建器连锁在一起，并为资产处理增加了离散步骤，从而提高了构建器的可重用性。

## 中间资产如何工作

当**资产处理器**完成一项作业时，它会检查输出标志，并将任何标有 IntermediateAsset 标志的输出复制到一个新的`<Cache>/Intermediate Assets`目录中。 这是一个扫描目录，资产处理器会像监控其他扫描目录一样，监控要处理的文件。 当中间资产被复制到该目录时，资产处理器会对其进行扫描，并将其发送到任何匹配的构建器，就像源资产一样。

有关中间资产如何运作的更多技术细节，请参阅： https://github.com/o3de/sig-content/blob/main/rfcs/rfc-46-intermediate-asset-products.md

## 使用中间资产

* 在生成器的 CreateJobs 函数中，创建一个平台标识符设置为 `AssetBuilderSDK::CommonPlatformName` 的 `JobDescriptor` 并将其添加到 `response.m_createJobOutputs` 中。 这表明作业将在所有平台上运行。每个作业只需要一个 JobDescriptor，与活动平台无关。
* 在 ProcessJob 函数中，处理文件并输出结果。创建一个 JobProduct 并设置 `m_outputFlags = AssetBuilderSDK::ProductOutputFlags::IntermediateAsset`.  将其添加到响应中： `response.m_outputProducts.push_back(jobProduct)`。

这就是输出一个中间资产供另一个构建器使用的全部要求。 创建器可以像这样串联起来，根据需要进行任意多个处理步骤。

## 限制
* 输出必须是中间资产或产品资产，不能两者兼而有之。 如果想同时输出这两种资产，则需要在 CreateJobs 中创建两个独立的任务。
* 产品资产不能输出到通用平台。
* 中间资产必须输出到通用平台。
 * 由于上述限制，在输出两种类型的产品时，ProcessJob 功能将在每个文件中运行多次，并且必须注意为当前工作请求输出正确的类型。

如果要输出特定平台的中间件，推荐的方法是输出所有名称带有平台前缀的中间件，然后输出一个额外的清单文件，该文件只需引用所有其他输出即可。例如，你可以创建一个作业，输出 `pc_myshader.bin`、`mac_myshader.bin` 和 `myshader.shadermanifest`。然后，你可以创建另一个生成器，使用 `*.shadermanifest` 作为输入模式来消耗这些内容。
