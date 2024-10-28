---
linktitle: 使用地形系统
title: 使用地形系统
description: 关于如何使用地形系统 API 的信息。
weight: 300
---

地形系统内部有许多组件、EBus 定义和 API。但是，如果作为开发人员，您的目标仅仅是_使用_地形系统，那么您需要的唯一方法就是定义在`TerrainDataNotificationBus`和 `TerrainDataRequestBus`上的方法。

## `TerrainDataNotificationBus`

`TerrainDataNotificationBus` 在地形系统首次激活、停用以及每次地形数据发生变化时提供通知。任何与地形系统保持某种形式同步状态的系统（如物理、渲染或导航）都需要监听此 EBus，以便在每次通知时更新其同步状态。

这就提出了几项要求。

1. 地形系统可随时激活或禁用，因此您应明确处理和测试这些情况。最简单的测试方法是启用和禁用**Terrain World**关卡组件。

2. 您的系统必须始终对数据变化作出反应。没有 “地形已加载完毕 ”的单一概念，因为所有数据都被分解成不同的片段，可以在不同的时间加载，而且数据可以随时动态地流入或流出。如果您的系统需要一个定义明确的 “完成 ”点，您就需要根据具体产品的要求，自己明确定义和跟踪这一状态。

3. 地形的整体大小是根据当前加载的数据动态变化的，没有硬编码的边界。任何使用地形数据的系统都应保持同样的灵活性，不需要任何硬编码限制。

4. 数据变化的大小可能与整个地形一样大。如果您的系统需要根据地形变化进行更新，您可能会希望将这些更新设置为异步并行，以获得最佳的无阻塞性能。异步进程应设计成可中断的，因为在更新前一个通知的过程中，很容易收到新的数据更改通知。要了解这一点，只需在编辑器中拖动带有**Terrain Layer Spawner**的实体，即可改变其位置。拖动产卵器时，产卵器周围的整个方框将不断生成变化通知。请参阅**Terrain Physics Collider Component**，了解根据 `OnTerrainDataChanged` 通知创建可中断异步更新的示例。

## `TerrainDataRequestBus`

`TerrainDataRequestBus` 提供了查询任意数量地形数据以及获取/设置全局地形设置的方法。任何需要地形信息的系统都可以使用此 EBus 来查询数据。

要有效地使用应用程序接口，就必须更详细地了解应用程序接口各方面的工作原理。

### 地形世界网格

底层地形数据可以是任何分辨率，包括程序数据（如随机数生成器）的 “无限 ”分辨率，不同地形层生成器的数据分辨率可以因位置而异。地形系统为高度数据和表面数据定义了独立的概念世界网格，以便将所有输入数据归一化为由网格间距定义的单一输出分辨率。

网格间距由`Height Query Resolution`和`Surface Data Query Resolution`设置控制。查询分辨率为 1 米意味着在整个世界范围内，无论基础数据的分辨率是多少，地形都将以一致的 1 米间距查询基础数据。

不过，地形系统本身可以在任何位置进行查询，而不仅仅是在概念网格上。`Sampler`类型可以精确控制与这些网格相关的底层数据的查询方式。

`Default` / `Bilinear` 采样器设置使用查询分辨率对请求位置周围的四个网格角进行采样，然后执行双线性滤波，生成输出值。

大多数系统在查询时都应使用 `Bilinear` 采样器设置。不过，还有另外两种采样方法可供专门使用。

`Clamp`采样器设置会在请求值之前将请求的查询位置钳位到地形网格上。`Bilinear`采样器会为网格点之间的每个位置返回无限范围的内插值，而`Clamp`采样器则不同，它总是为网格点之间的每个位置返回相同的值。由于它只获取单个网格点，而不是四个网格角，因此运行速度是 `Bilinear`采样器的 4 倍。不过，只有直接在网格点上查询时，返回的值才会与渲染和物理表示相匹配。因此，只有在直接查询地形网格点时才能使用此设置。

`Exact`采样器设置会获取所请求的查询位置，并将其直接传递到底层数据源，完全忽略地形网格。与 `Clamp`类似，它比 `Bilinear`快 4 倍，但只有在直接查询网格点时才会产生匹配值。您不应使用它来查询网格点以外的位置，因为数据可能与渲染或物理表示不一致。不过，有需要的用户可以使用该功能。

下图展示了采样器之间的差异。圆圈代表底层数据源值，在本例中每 0.25 米就有一个不同的值。正方形是不同高度查询分辨率设置下的地形网格。黑色实心圆圈是我们正在查询高度的位置。蓝色圆圈是用于计算结果的数据源值。

| 采样类型 | 说明 | Height Query Resolution 1.0 m | Height Query Resolution 0.5 m | Height Query Resolution 0.25 m |
| - | - | - | - | - |
| `Bilinear` | (0.75，0.75）的高度查询收集了 A、B、C 和 D 点的高度，并通过内插计算出最终结果。| {{< image-width src="/images/user-guide/visualization/environments/terrain/bilinear-1.0m.svg" width="400" alt="Bilinear sampling at 1.0 meters." >}} | {{< image-width src="/images/user-guide/visualization/environments/terrain/bilinear-0.5m.svg" width="400" alt="Bilinear sampling at 0.5 meters." >}} | {{< image-width src="/images/user-guide/visualization/environments/terrain/bilinear-0.25m.svg" width="400" alt="Bilinear sampling at 0.25 meters." >}} |
| `Clamp` | (0.75, 0.75）的高度查询返回 A 点的高度，A 点是查询位置向下舍入的地形网格点。 | {{< image-width src="/images/user-guide/visualization/environments/terrain/clamp-1.0m.svg" width="400" alt="Clamp sampling at 1.0 meters." >}} | {{< image-width src="/images/user-guide/visualization/environments/terrain/clamp-0.5m.svg" width="400" alt="Clamp sampling at 0.5 meters." >}} | {{< image-width src="/images/user-guide/visualization/environments/terrain/clamp-0.25m.svg" width="400" alt="Clamp sampling at 0.25 meters." >}} |
| `Exact` | (0.75，0.75）的高度查询返回 A 点的高度，无论地形网格大小如何，该高度始终是该位置底层数据源的值。 | {{< image-width src="/images/user-guide/visualization/environments/terrain/exact-1.0m.svg" width="400" alt="Exact sampling at 1.0 meters." >}} | {{< image-width src="/images/user-guide/visualization/environments/terrain/exact-0.5m.svg" width="400" alt="Exact sampling at 0.5 meters." >}} | {{< image-width src="/images/user-guide/visualization/environments/terrain/exact-0.25m.svg" width="400" alt="Exact sampling at 0.25 meters." >}} |

### `terrainExists` 标签

由于地形数据只存在于任意授权区域内，因此可以查询地形系统中不存在地形的点。只要存在高度数据，地形就被定义为存在。在查询地形数据时，应始终对每个查询结果检查该标志，以确保数据确实代表有效的地形。

### 查询优化

以下是一些关于如何从地形查询 API 中获得最佳性能的提示：

* 地形查询 API 可让您查询高度、法线和表面数据的不同组合。查询的数据越多，查询成本就越高，因此应尽量限制查询次数，以获取您打算使用的最少数据量。当您需要同一位置的多种类型数据时，能让您一次获取多种类型数据的 API 通常比单独调用单个 API 更快。例如，`GetSurfacePoint`比单独调用 `GetHeight`, `GetNormal`, 和 `GetSurfaceWeights`更快。
* 查询多个输入位置时，批量 API（`QueryList`、`QueryRegion`）通常比调用单个 API（`GetHeight` 等）快 80-90%。
* `QueryRegion` / `QueryRegionAsync` 是最快的查询 API，因为它们不需要在调用前额外建立位置输入列表。
* 只要注意将输入位置或输入区域与地形网格精确对齐，使用 `Exact` 或 `Clamp` 采样器可将查询速度提高 4 倍。不对齐会产生不准确的结果。
* `TerrainAreaExistsInBounds`提供了一种快速方法来确定 AABB 内是否存在地形。如果系统所关心的区域不包含任何地形，则可以用它来完全规避更昂贵的按点查询。

### 射线检测查询

`TerrainDataRequestBus`提供了一个`GetClosestIntersection`方法，用于对地形执行单次射线播报。通常情况下，碰撞光线投射应通过物理系统执行，但该方法可作为物理光线投射不合适时的替代方法。例如，在编辑器中将物体放置到地形上时就可以使用这种方法。

{{< tip >}}
`GetClosestIntersection` 提供了任意方向的通用射线播报，但如果您的系统只进行直下射线播报以获取某个位置正下方的地形信息，请使用其他查询 API，如 `GetSurfacePoint` 或 `QueryList`。它们能产生更快的结果。
{{< /tip >}}

##  表面数据系统

还有一种间接使用地形系统的方法，那就是通过`Surface Data` API。如果您的系统设计用于处理曲面，无论曲面是来自形状、网格、地形还是其他地方，`Surface Data`应用程序接口都提供了一种无需直接连接地形即可获取通用曲面数据信息的方法。

要使用该系统，可使用`SurfaceDataSystemRequestBus`上的`GetSurfacePoints*` API 来查询高度和曲面数据信息，并使用`SurfaceDataSystemNotificationBus`上的 `OnSurfaceChanged` API 来监听更改通知。这些 API 使用抽象曲面标记来查询曲面，因此地形数据会与其他与地形使用相同曲面标记的曲面数据混在一起。
