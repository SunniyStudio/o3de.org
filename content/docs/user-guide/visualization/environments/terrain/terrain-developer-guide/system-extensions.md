---
linktitle: 扩展地形系统
title: 扩展地形系统
description: 有关如何扩展地形系统以实现各种目标的信息。
weight: 400
---
地形系统的设计高度解耦，易于在多个不同层面进行扩展。

## Gradient 组件

Gradient组件是高度数据和表面重量数据的基础数据源。可根据需要创建新的梯度组件类型，以提供不同来源的数据（如流式卫星数据），或修改现有的梯度数据（如添加复杂的侵蚀过滤器）。

新的Gradient组件至少需要支持以下内容：
* 实现 `GradientRequestBus` 以通用地返回输入世界位置的浮点输出值。
* 在 `GetProvidedServices` 中声明它实现了 `GradientService`。
* 支持线程安全。理想情况下，这将意味着在查询 API 中使用带有`shared_lock`的`shared_mutex`，并在更改查询 API 方法中使用的底层数据的任何地方使用`unique_lock`。这将允许多个查询并行运行，同时安全地阻塞数据更改。
* 只要梯度中的任何数据发生变化，就调用 `DependencyNotificationBus::Events::OnCompositionChanged` 或 `DependencyNotificationBus::Events::OnCompositionRegionChanged` 。这将通知梯度 “上方 ”的所有内容需要刷新。

此外，新的梯度组件还应支持以下功能：

* 使用**Shape**组件来定义如何将梯度数据映射到世界空间。
* 支持**Gradient Transform Modifier**组件，以便对梯度数据执行任意变换。
* 按照惯例，目前所有渐变效果都会侦听`OnEntityVisibilityChanged`，并使用它来启用或禁用编辑器中的渐变数据。这样就可以在编辑时轻松打开或关闭渐变，但重要的是要认识到，无论编辑时的可见性设置如何，渐变在运行时仍处于活动状态。

## 地形组件

每个地形组件都可以用满足相同要求的新组件替换。例如，一个能向下传输真实世界卫星高度数据的新组件可以取代**Terrain Height Gradient List**。也可以将组件组合在一起，如简化的地形生成器组件，在一个组件中实现**Terrain Layer Spawner**、**Terrain Height Gradient List**和**Terrain Surface Gradient List**的功能。

任何替换组件都需要支持以下功能：
* 执行与被替换组件相匹配的请求总线。例如，`TerrainSpawnerRequestBus`、`TerrainAreaHeightRequestBus` 或`TerrainAreaSurfaceRequestBus`。
* 声明它实现了与`GetProvidedServices`中被替换的组件相匹配的每个服务。例如，`TerrainAreaService`、`TerrainHeightProviderService` 或`TerrainSurfaceProviderService`。
* 支持线程安全。
* 只要组件中的任何数据发生变化，就调用 `OnCompositionChanged` 或 `OnCompositionRegionChanged`。
* 调用 `TerrainSystemServiceRequestBus` 上相应的 API 来注册/取消注册/刷新地形区域。

## 地形物理集成

地形物理支持是通过一个分割的组件系统提供的。一半是`TerrainPhysicsCollider`，它了解用于地形数据的通用`TerrainDataRequestBus`和用于物理通信的通用 `HeightfieldProvider`总线。另一半是 `PhysXHeightfieldCollider`，它了解 PhysX 和高度场，但不了解地形。要将地形与新的物理系统集成，就需要实现一个新的 `HeightfieldCollider` 组件，以取代与 `HeightfieldProvider` 总线和新物理系统一起工作的 PhysX 版本。

## 地形渲染器

可以通过创建新的渲染级别组件、渲染实体组件、特征处理器和着色器来替换整个地形渲染器。更换呈现器不会影响基础地形系统或地形物理集成。

## 地形系统

底层的地形系统也可以完全替换，只要它实现了`TerrainDataRequestBus``TerrainSystemServiceRequestBus`，并使用`TerrainDataNotificationBus`发送更改通知即可。虽然没有明显的理由单独替换地形系统本身，因为它主要只是路由数据请求和通知。
