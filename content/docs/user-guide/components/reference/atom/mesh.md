---
title: Mesh 组件
linktitle: Mesh
description: 'Open 3D Engine (O3DE) Mesh 组件参考'
toc: true
---

**Mesh** 组件指定了一个要在场景中渲染的模型。模型资产使用 [AssImp](https://www.assimp.org/) 支持和处理。

## 提供方 ##

[Atom Gem](/docs/user-guide/gems/reference/rendering/atom/atom/)


## Mesh 组件属性

![mesh-component-base-properties](/images/user-guide/components/reference/atom/mesh/mesh-base-properties-ui.png)

| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **Model Asset** | 设置该组件的模型资产。 | Model Asset | None |
| **Sort Key** | 透明模型首先按排序键绘制，然后是深度。使用它可以强制某些透明模型在其他模型之前或之后绘制。 | -2,147,483,648 to 2,147,483,647 | `0` |
| **Exclude from reflection cubemaps** | 如果启用，模型将不会在烘焙反射探针立方图中显示。 | Boolean | `Disabled` |
| **Use Forward Pass IBL Specular** | 只使用影响最大的探测器（基于实体的位置）和全局 IBL 立方地图，在前向传递中渲染基于图像的照明 (IBL) 镜面反射。这种方法可以降低渲染成本，但只推荐用于受最多一个反射探针影响的静态物体。 | Boolean | `Disabled` |
| **Use ray tracing** | 在光线跟踪计算中包含该模型。 | Boolean | `Enabled` |
| **LOD Type** | 请参阅 [LOD Type](#lod-type) 下方。 | `Default`, `Screen Coverage`, `Specific LOD` | `Default` |
| **Add Material Component** | 添加 [Material 组件](/docs/user-guide/components/reference/atom/material/) 到 实体的按钮。 | | |
| **Model Stats** | 显示模型中每个 LOD 的网格数、顶点数和三角形数。 | | |


### LOD Type
**LOD Type** 决定渲染时如何选择细节级别（LOD）。

{{< tabs name="lod-type-ui" >}}
{{% tab name="Default" %}}

![lod-type-default](/images/user-guide/components/reference/atom/mesh/lod-type-ui/mesh-default.png)

**LOD Type**: `Default` 使用配置的**默认方法**自动选择 LOD。O3DE 出厂时使用 Screen Coverage 方法作为默认方法。如果将来更改了*默认方法*，组件将自动使用新的默认方法。

{{% /tab %}}
{{% tab name="Screen Coverage" %}}

![lod-type-screen-coverage](/images/user-guide/components/reference/atom/mesh/lod-type-ui/mesh-screen-coverage.png)

**LOD Type**: `Screen Coverage` 会根据 LOD 所覆盖的屏幕大致比例来确定要渲染的 LOD。

| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **LOD Configuration - Minimum Screen Coverage** | 实体覆盖屏幕区域的最小比例。如果实体小于最小覆盖范围，就会被删除。 | 0.0 to 1.0 | 1.0f / 1080.0f |
| **LOD Configuration - Quality Decay Rate** | 网格质量的衰减速度。 <br><br>`0` - 始终保持最高质量的 LOD。 <br><br>`1` - 立即降到最低质量的 LOD。 | 0.0 to 1.0 | 0.5 |

{{% /tab %}}
{{% tab name="Specific LOD" %}}

![lod-type-specific-lod](/images/user-guide/components/reference/atom/mesh/lod-type-ui/mesh-specific-lod.png)

**LOD Type**: `Specific LOD` 指定要渲染的 LOD，覆盖自动 LOD 计算。

| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **LOD Configuration - LOD Override** | 设置要渲染的特定 LOD。LOD 的数量取决于资产的 LOD 数量。 | LOD 0 (最高细节) 到 LOD *n*, 其中 *n* 是最低细节 LOD | LOD 0 (最高细节) |

{{% /tab %}}
{{< /tabs >}}

## MeshComponentRequestBus

| 方法名称 | 说明 | 参数 | 返回值 | 可脚本化 |
|-|-|-|-|-|
| `SetModelAsset` | 设置组件使用的模型资产。 | Model Asset: Asset<T> | None | No |
| `GetModelAsset` | 返回组件使用的模型资产。 | None | Model Asset: Asset<T> | No |
| `SetModelAssetId` | 通过资产 ID 设置组件使用的模型。 | Model AssetId: AssetId | None | Yes |
| `GetModelAssetId` | 返回组件使用的模型的 AssetId。 | None | Model AssetId: AssetId | Yes |
| `SetModelAssetPath` | 通过路径设置组件使用的模型。 | Asset Path: String | None | Yes |
| `GetModelAssetPath` | 返回组件使用的模型路径。 | None | Asset Path: String | Yes |
| `GetModel` | 返回组件使用的模型实例。 | None | Model: Instance<T> | No |
| `SetSortKey` | 见 [排序键](#mesh-component-properties) | Draw Item Sort Key: Integer | None | 见 |
| `GetSortKey` | 见 [排序键](#mesh-component-properties) | None | Draw Item Sort Key: Integer | Yes |
| `SetLodType` | 见 [LOD 类型](#lod-type) | LOD Type: Enum | None | Yes |
| `GetLodType` | 见 [LOD 类型](#lod-type) | None | LOD Type: Enum | Yes |
| `SetLodOverride` | 见 [LOD Type: Specific LOD Tab](#lod-type) | LOD Override: Integer | None | Yes |
| `GetLodOverride` | 见 [LOD Type: Specific LOD Tab](#lod-type) | None | LOD Override: Integer | Yes |
| `SetMinimumScreenCoverage` | 见 [LOD Type: Screen Coverage Tab](#lod-type) | Minimum Screen Coverage: Float | None | Yes |
| `GetMinimumScreenCoverage` | 见 [LOD Type: Screen Coverage Tab](#lod-type) | None | Minimum Screen Coverage: Float | Yes |
| `SetQualityDecayRate` | 见 [LOD Type: Screen Coverage Tab](#lod-type) | Quality Decay Rate: Float | None | Yes |
| `GetQualityDecayRate` | 见 [LOD Type: Screen Coverage Tab](#lod-type) | None | Quality Decay Rate: Float | Yes |
| `SetVisibility` | Sets if the model should be visible (true) or hidden (false). | Visibility: Boolean | None | No |
| `GetVisibility` | 返回可见性。如果模型是可见的（true），这只意味着它没有被显式隐藏。任何正在渲染的视图可能仍然看不到该模型。如果模型不可见（false），则无论模型是否在视图区域内，都不会被任何视图渲染。 | None | Visibility: Boolean | No |
| `SetRayTracingEnabled` | 见 [使用光线追踪](#mesh-component-properties) | Is Ray Tracing Enabled: Boolean | None | Yes |
| `GetRayTracingEnabled` | 见 [使用光线追踪](#mesh-component-properties) | None | Is Ray Tracing Enabled: Boolean | Yes |
| `GetWorldBounds` | 返回模型在其世界位置的轴对齐包围盒。 | None | World Bounds: Aabb | No |
| `GetLocalBounds` | 返回模型空间中轴对齐的包围盒。 | None | Local Bounds: Aabb | No |
## MeshComponentNotificationBus

| 方法名称 | 说明 | 参数 | 返回值 | 可脚本化 |
|-|-|-|-|-|
| `OnModelReady` | 加载模型时通知侦听器。如果首次连接到 `MeshComponentNotificationBus` 时模型已加载，则连接时将发生 `OnModelReady` 事件。 | Model Asset: `Asset<T>`, Model: `Instance<T>` | None | Yes |
| `OnModelPreDestroy` | 当该组件的模型实例即将被释放时通知侦听器。 | None | None | No |
