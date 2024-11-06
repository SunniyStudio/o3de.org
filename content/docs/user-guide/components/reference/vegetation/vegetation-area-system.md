---
linkTitle: Vegetation Area System
title: Vegetation Area System 组件
description: Vegetation Area System组件根据其在 **Open 3D Engine （O3DE）** 中配置的规则处理植被实例的放置和删除。
---

**Vegetation Area System** 组件根据其配置的规则处理整个世界中植被实例的放置和删除。

有关 Vegetation Area System 组件如何在植被系统中工作的更多信息，请参阅本主题中的 [技术详细信息](#technical-details) 部分。

{{< note >}}
这是一个系统组件，这意味着当您通过 Vegetation Gem 添加植被系统时，它已经存在。您可以通过 **Vegetation System Settings** 级别组件配置每个关卡的植被区域属性。
{{< /note >}}

## 提供者

[Vegetation Gem](/docs/user-guide/gems/reference/environment/vegetation/)

## 源代码

[`/Gems/Vegetation/Code/Source/AreaSystemComponent.h`](https://github.com/o3de/o3de/blob/development/Gems/Vegetation/Code/Source/AreaSystemComponent.h)

## 参数

| 属性 | 说明 | 值 | 默认值 |
| --- | --- | --- | --- |
| **View Area Grid Size** | 以摄像机为中心的滚动视图中托管网格每侧的扇区数 | 1 to 128 | 13  |
| **Sector Point Density** | 扇区内每一侧的等距植被实例网格放置点的数量 | 1 to 64 | 20 |
| **Sector Size In Meters** | 每个扇区每侧的大小（以米为单位）。 | 1 to 1024 | 16 |
| **Thread Processing Interval** | 处理排队线程任务之间的延迟（以毫秒为单位）。 | 0 to 5000 | 500 |
| **Sector Search Padding** | 在枚举植被实例时增加周围区域的搜索半径。 | 0 to 2 | 0 |
| **Sector Point Snap Mode** | 控制植被放置点是位于像元的拐角还是中心。 | `Corner`, `Center` | `Corner` |

## AreaSystemRequestBus

文件: [`Gem/sVegetation/Code/Include/Vegetation/Ebuses/AreaSystemRequestBus.h`](https://github.com/o3de/o3de/blob/5783c18cc8d65f75737159f4cdcf6019d0e8dcfc/Gems/Vegetation/Code/Include/Vegetation/Ebuses/AreaSystemRequestBus.h)

| 请求名称 | 说明 | 参数 | 返回值 | 可脚本化 |
| --- | --- | --- | --- | --- |
| `RegisterArea` | 将 Vegetation Area 实体添加到系统以进行处理。 | `AZ::EntityId`, `AZ::u32`, `AZ::Aabb&` | None | No  |
| `UnregisterArea` | 从系统中删除 Vegetation Area 实体。 | `AZ::EntityId` | None | No  |
| `RefreshArea` | 将 Vegetation Area 排队以更新其系统内的数据，并将扇区区域标记为脏区域以进行重新处理。 | `AZ::EntityId`, `AZ::u32`, `AZ::u32`, `AZ::Aabb&` | None | No  |
| `RefreshAllAreas` | 更新所有已注册区域的缓存数据，并导致整个系统重新填充。 | None | None | No  |
| `ClearAllAreas` | 强制删除每个扇区中的所有缓存数据和实例，并导致整个系统重新填充。 | None | None | No  |
| `MuteArea` | 指示 Vegetation Area System 在重新填充扇区时忽略指定区域，因为它在其他位置进行管理。由 Vegetation Layer Blender 控制的区域就是这种情况。 | `AZ::EntityId` | None | No  |
| `UnmuteArea` | Re-enables direct processing of a Vegetation Area that was previously muted. | `AZ::EntityId` | None | No  |
| `EnumerateInstancesInOverlappingSectors` | 访问与给定边界重叠的每个植被扇区中包含的所有实例，直到回调函数另有决定。此外，扇区边界由 Area System 组件配置中的 **Sector Search Padding** 值定位。 | `AZ::Aabb&`, `AreaSystemEnumerateCallback` | None |  No |
| `EnumerateInstancesInAabb` | 为与指定边界相交的每个 Vegetation 实例调用回调函数 | `AZ::Aabb&`, `AreaSystemEnumerateCallback` | None | No  |
| `GetInstanceCountInAabb` | 获取轴对齐边界框 （AABB） 中包含的当前实例数。 | `AZ::Aabb&` | `AZStd::size_t` | Yes |
| `GetInstancesInAabb` | 获取 AABB 中包含的实例列表。 | `AZ::Aabb&` | `AZStd::vector<Vegetation::InstanceData>` | Yes |


## 技术细节

Vegetation Area System （植被区域系统） 组件跟踪已注册的植被区域，并使用它们来填充摄像机周围的区域。
该区域是一个可配置大小的扇区网格，其中包含有关环境及其中存在的植被的详细信息。
每个扇区通过在子格网上的每个点执行向下交集测试来分析重叠表面，以收集和缓存表面数据样本。
这些样本代表了有关植被种植位置的所有可用点。

当相机移动或发生其他事件以修改植被区域或表面的状态时，Vegetation Area System （植被区域系统） 组件会收到通知。
然后，它将处理扇区和植被区域的任务排入另一个线程上的队列。
位于区域之外的扇区将被销毁。
评估区域内的新扇区或剩余扇区，以确定是否需要填充。

正在填充的每个扇区将按优先级顺序遍历所有重叠的植被区域。
每个植被区域都会获得该区域的可用点列表，每个点都可以检查和索取。
如果是这样，则会从循环中删除该点。
当植被区域声明一个点时，其他植被区域不能使用该点。
当没有更多可用点或已处理所有植被区域时，扇区将完成其处理。

例如，**Vegetation Layer Spawner** 组件会分析每个点，并根据其配置、形状和其他筛选器确定是否可以声明该点。
如果一切成功，Vegetation Layer Spawner 将请求在该位置创建一个对象或 **植被实例**。
这些实例创建和销毁请求将发送到 **Vegetation Instance System** 组件。
