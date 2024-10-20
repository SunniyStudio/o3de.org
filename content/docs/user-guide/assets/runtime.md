---
linkTitle: 运行时资产系统
title: 运行时资产系统
description: 介绍Open 3D Engine (O3DE)运行时资产系统的使用和工作。
weight: 700
toc: true
---

运行时资产系统负责在 O3DE 编辑器/启动器中加载和管理资产。 一个资产对应一个由资产处理器处理成产品资产的文件。 每个资产都有一个称为 AssetId 的唯一 ID，该 ID 由源资产的 UUID 和产品资产的 SubId 组成。

## 资产引用

[AZ::Data::Asset](https://github.com/o3de/o3de/blob/development/Code/Framework/AzCore/AzCore/Asset/AssetCommon.h#L293) 类用于引用`AssetData`对象（资产），本质上是一个智能指针。 当对给定资产的所有引用都被释放时，该资产就会被卸载。 当一个 `Asset<T>` 类型保存到磁盘时，它会被保存为一个 AssetId，可用于以后再次加载资产。

## 运行时加载资产

[AZ::Data::AssetManager](https://github.com/o3de/o3de/blob/development/Code/Framework/AzCore/AzCore/Asset/AssetManager.h#L136) 系统负责在运行时加载和管理资产。


`GetAsset` 是用于加载现有资产的主要 API。 它返回一个 `Asset<T>` 引用。 每次只会在内存中加载一份资产副本，因此如果资产已经加载或正在加载，则不会发出新的加载请求；返回的 `Asset<T>` 将指向相同的 `AssetData`，并将在加载完成时（如果尚未完成）准备就绪。 `GetAsset` 是一个非阻塞异步 API，这意味着资产数据可能不会在调用完成后立即可用。

有两种方法可以处理等待资产加载完成的问题：阻塞方法是在 `Asset<T>` 引用上调用[BlockUntilLoadComplete](https://github.com/o3de/o3de/blob/18205539abf1b1d2eb3959c0a1c42a3eea16a455/Code/Framework/AzCore/AzCore/Asset/AssetCommon.h#L387)。 这将阻塞当前线程，直到资产加载完成。 另一种（也是推荐的）方法是连接到 [AssetBus](https://github.com/o3de/o3de/blob/18205539abf1b1d2eb3959c0a1c42a3eea16a455/Code/Framework/AzCore/AzCore/Asset/AssetCommon.h#L527) 并处理资产的 `OnAssetReady`/`OnAssetReloaded` 事件。

请注意，引擎目前不支持取消加载（在加载请求完成前取消加载），因此有可能调用 `GetAsset`，然后在等待调用 `OnAssetReady` 时丢弃结果。 最好不要依赖这种情况，因为这不是我们想要的功能。 资产引用的存储时间应与资产的加载时间一致（通常是在资产处于使用状态时）。


### 加载依赖项

默认情况下，`AssetManager` 会在发出资产加载完成的信号之前加载资产的所有依赖项。 资产依赖项记录在`AssetCatalog`中，由每个生成器发出的 “产品依赖项 ”决定。 依赖项有 3 种不同的加载设置（`资产加载行为`）：
1. Preload（预加载） - 依赖关系将在父资产就绪之前完成加载。
2. QueueLoad（队列加载）- 依赖项将在父资产启动时开始加载，但在父资产发出就绪信号之前无需完成加载。
3. NoLoad - 不要求加载依赖项。 如果依赖项已经加载，则任何引用仍将指向已加载的资产。

请记住，无论设置如何，AssetManager 都不保证资产完成加载的顺序。


### 热加载

资产系统支持热加载，即在磁盘上的资产发生变化时更新已加载的内存资产。 热加载允许内容创建者快速迭代资产，并在无需重启引擎的情况下看到实时更新的变化。 当资产处理器完成对资产的处理后，会向引擎发出通知；如果相关资产已经加载或正在加载，则会排队等待重新加载请求，然后资产管理器开始加载新更新的资产。 使用资产系统的每个系统都必须通过处理 [OnAssetReloaded](https://github.com/o3de/o3de/blob/18205539abf1b1d2eb3959c0a1c42a3eea16a455/Code/Framework/AzCore/AzCore/Asset/AssetCommon.h#L603)事件来单独实现对这一功能的支持。

通常情况下，项目会从加载的资产中复制数据，以执行额外的处理。资产重载的一个常见错误是不刷新任何复制的数据。例如，如果您的项目是一款使用 XML 文件定义玩家移动速度的游戏，在加载 XML 文件时，信息会被复制到玩家实体上的一个组件中，那么在发生重新加载事件时，也必须将信息重新复制到玩家实体上。否则，即使更新了 XML 文件以改变移动速度，玩家也将继续以旧的移动速度移动。

## 资产处理优先级

以下内容通常只与项目开发期间相关，因为在运行引擎之前，资产并不总是被提前处理。 在已编译和捆绑所有资产的发布项目中，这些概念并不适用。

### 关键资产

关键资产是启动发动机所需的资产，必须由资产处理器处理后才能完成启动。 资产处理器会优先处理关键资产，然后再处理其他类型的资产。 它们由资产创建者声明为 [任务描述符](https://github.com/o3de/o3de/blob/18205539abf1b1d2eb3959c0a1c42a3eea16a455/Code/Tools/AssetProcessor/AssetBuilderSDK/AssetBuilderSDK/AssetBuilderSDK.h#L446)的一部分。 例外情况是，如果关键资产与非关键资产有任务依赖关系，则关键资产将等待非关键资产处理完毕后再继续。 这可能会导致关键资产要等到所有其他关键资产都先处理完毕后，其依赖资产才能开始处理。 建议尽量减少这类资产，因为它们会大大影响启动引擎所需的时间。

### 资产任务升级

作为关键资产的替代方案，资产处理器支持 “任务升级”（Job Escalation），可用于使资产处理器优先处理给定资产，然后再处理其他资产。 `AssetSystemBus`提供了一组[CompileAssetSync](https://github.com/o3de/o3de/blob/18205539abf1b1d2eb3959c0a1c42a3eea16a455/Code/Framework/AzFramework/AzFramework/Asset/AssetSystemBus.h#L199) 应用程序接口，可用于请求资产处理器升级资产编译并阻塞直到编译完成。

有关非阻塞升级请求，请参阅 [EscalateAsset](https://github.com/o3de/o3de/blob/18205539abf1b1d2eb3959c0a1c42a3eea16a455/Code/Framework/AzFramework/AzFramework/Asset/AssetSystemBus.h#L248) API。

