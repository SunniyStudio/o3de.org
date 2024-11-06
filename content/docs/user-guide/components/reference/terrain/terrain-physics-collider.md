---
title: Terrain Physics Heightfield Collider 组件
linktitle: Terrain Physics Heightfield Collider
description: 'Open 3D Engine (O3DE) Terrain Physics Heightfield Collider 组件参考。'
weight: 100
---

**Terrain Physics Heightfield Collider** 组件以高度场和材质分配的形式向物理系统提供地形数据。 您可以通过修改同一实体上的[Axis Aligned Box Shape](/docs/user-guide/components/reference/shape/axis-aligned-box-shape)组件来配置碰撞器的尺寸。

## 提供者

[Terrain Gem](/docs/user-guide/gems/reference/environment/terrain)

## 依赖

[Axis Aligned Box Shape](/docs/user-guide/components/reference/shape/axis-aligned-box-shape)  
[PhysX Heightfield Collider](/docs/user-guide/components/reference/physx/heightfield-collider)

PhysX Heightfield Collider （PhysX 高度场碰撞器） 仅在将此组件与 PhysX 一起使用时是一个依赖项。在实现替代物理系统时，**地形物理高度场碰撞器** 需要该物理系统的配套高度场碰撞器组件。

## 属性

![Terrain Physics Heightfield Collider component properties](/images/user-guide/components/reference/terrain/terrain-physics-heightfield-collider-component.png)

| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **Default Surface Physics Material** | 选择默认由未映射表面使用的 [物理材质](/docs/user-guide/gems/reference/environment/surface-data) 。 | Material: Physics Material | (default) |
| **Surface to Material Mappings** | 要映射在一起的 [surface tags](/docs/user-guide/gems/reference/environment/surface-data) 和物理材质的数组。 |  |  |
| **Surface Tag** | 选择一个 [表面标签](/docs/user-guide/gems/reference/environment/surface-data)以映射到 [物理材质](/docs/user-guide/interactivity/physics/nvidia-physx/materials)。 | Surface:  Surface Tag | (unassigned) |
| **Material Asset** | 选择要应用于表面的 [物理材质](/docs/user-guide/interactivity/physics/nvidia-physx/materials)。 | Material: Physics Material | (default) |

## 用法

物理系统通过 **Terrain Physics Heightfield Collider** 组件接收有关地形的碰撞信息。此组件采用关联的 **Axis Aligned Box Shape** 组件中的区域，并在物理系统中创建一个高度场碰撞器。高度场碰撞器的物理材质分配因顶点而异，具体取决于底层地形表面权重和表面类型到此组件上的物理材质映射。

由于地形系统使用 O3DE 中的抽象物理 API，因此 **Terrain Physics Heightfield Collider** 组件需要与之交互的配套高度场碰撞器组件，以将地形数据转换为已实现的任何物理系统的物理数据。例如，将此组件与 NVIDIA PhysX 一起使用时，需要 **PhysX Heightfield Collider** 组件与 **Terrain Physics Heightfield Collider** 位于同一实体上，才能在 PhysX 中创建地形高度场。

如果方便，**Terrain Physics Heightfield Collider** 可以与 **Terrain Layer Spawner** 组件存在于同一实体上，但这不是必需的。碰撞器不直接与生成器相关联。此组件可以与任何区域一起使用，无论它与多个刷怪箱重叠、单个刷怪箱或根本没有刷怪箱。将碰撞体保存在与生成器不同的实体上的主要优点是，物理碰撞体可以在与地形不同的时间和大小上动态生成和消失。这使得在世界中拥有一个完全可见的大型地形，而物理世界中只存在一个存在于玩家周围的小子区域，以便更好地控制性能和内存使用。

您可以通过在表面下拉菜单中选择表面类型，然后在材质下拉菜单中选择物理材质类型来分配映射到特定物理材质的地形表面类型。多个地形表面类型可以映射到相同的物理材质。任何未映射到特定物理材质的地形表面类型都将自动映射到 **Default Surface Physics Material**。

## HeightfieldProviderRequestsBus

'`HeightfieldProviderRequestsBus`' 是一条内部系统总线，仅用于物理系统和高度场组件之间的通信。通常，如果您要为备用物理系统实现新的 Heightfield Collider （高度场碰撞器） 组件，则只需使用此 EBus。将以下请求函数与 '`HeightfieldProviderRequestsBus`' 事件总线接口结合使用，以与 Terrain Physics Heightfield Collider 组件进行通信。

| 方法名称 | 说明 | 参数 | 返回值 | 脚本化 |
|-|-|-|-|-|
| `GetHeightfieldGridSpacing` | 返回 heightfield 的分辨率。 | None | Resolution: Vector2 | No |
| `GetHeightfieldGridSize` | 以行数和列数的形式返回 heightfield 的大小。 | None | Row Count: Integer; Column Count: Integer | No |
| `GetMaterialList` | 返回此组件使用的表面数组。 | None | Array of Physics Materials Indexes: I | No |
| `GetHeights` | 将 heightfield 作为 float 值数组返回。 | None | Array of Heights: Float | No |
| `GetHeightsAndMaterials` | 返回 heightfield 中的高度数组，以及每个点的物理材质索引。 | None | Array of Heights: Float, Physics Material Indexes: Integer | No |
| `UpdateHeights` | 返回特定边界内 heightfield 数组的子部分。 | Bounds: Aabb | Array of Heights: Float | No |

## HeightfieldProviderNotificationBus

'`HeightfieldProviderNotificationBus`' 是一个内部系统总线，仅用于物理系统和高度场组件之间的通信。通常，如果您要为备用物理系统实现新的 Heightfield Collider （高度场碰撞器） 组件，则只需使用此 EBus。使用“`HeightfieldProviderNotificationBus`”事件总线接口注册以下通知函数，以接收来自 Terrain Physics Heightfield Collider 组件的通信。

| 通知名称 | 说明 | 参数 | 返回值 | 脚本化 |
|-|-|-|-|-|
| `OnHeightfieldDataChanged` | 当影响高度场的任何地形数据发生更改时发出通知。 | Dirty Region: Aabb, Change Mask: 包含用于设置更改的“`Settings`”、用于高度更改的“`HeightData`”、用于表面数据更改的“`SurfaceData`”和用于表面类型到材质映射更改的“`SurfaceMapping`”的位域。  | None | No |
