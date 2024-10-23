---
title: 定义和使用组件服务
description: 了解如何在 Open 3D Engine 中定义和使用组件服务。
weight: 400
---

组件可以选择性地指定其提供、不兼容、依赖或需要的服务列表。创建组件时，可以使用此服务规范来定义不同组件之间的关系。在编辑和运行时，组件实体系统都会使用这个列表来有条件地添加和删除组件。服务规范还定义了激活实体时组件被激活的顺序。具体来说，提供另一个组件所依赖的服务的组件会首先被激活。

下面的示例显示了一个服务规范。

```cpp
static void GetProvidedServices(AZ::ComponentDescriptor::DependencyArrayType& provided)
{
      provided.push_back(AZ_CRC_CE("ProvidedService"));
      provided.push_back(AZ_CRC_CE("AnotherProvidedService"));
}

static void GetRequiredServices(AZ::ComponentDescriptor::DependencyArrayType& required)
{
      required.push_back(AZ_CRC_CE("RequiredService"));
      required.push_back(AZ_CRC_CE("AnotherRequiredService"));
}

static void GetIncompatibleServices(AZ::ComponentDescriptor::DependencyArrayType& incompatible)
{
      incompatible.push_back(AZ_CRC_CE("IncompatibleService"));
}

static void GetDependentServices(AZ::ComponentDescriptor::DependencyArrayType& dependent)
{
      dependent.push_back(AZ_CRC_CE("DependentOnService"));
}
```

**ProvidedService** -- 指定组件提供的服务。例如，`TransformComponent`可提供`TransformService`，而`TransformService`又可提供定位信息。

**RequiredService** -- 指定组件所需的服务。在激活该组件之前，提供所需服务的组件必须保证存在并处于活动状态。例如，音频组件可能需要知道它的位置，因此需要一个`TransformService`。由于这一要求，音频组件只能添加到拥有提供 `TransformService` 的组件的实体中。

**DependentService** -- 指定组件依赖但不需要的服务。组件实体系统保证，在组件本身被激活之前，提供依赖服务的组件已被激活。例如，音频组件可能依赖于 `physics`组件。如果实体具有物理特性，音频组件就可以向 `physics`组件查询物理材料信息。但是，音频组件并不要求物理材料存在。

**IncompatibleService** -- 指定不能与组件一起工作的服务。请参考这些示例：

+ 一个实体只能有一种类型的碰撞体。因此，`PrimitiveColliderService`会指定`MeshColliderService`与其不兼容，反之亦然。
+ 如果两个碰撞体组件本身已经提供了 `ColliderService` 服务，并因此将 `ColliderService` 指定为不兼容，也能达到同样的效果。将一个组件标记为与 `ColliderService` 不兼容，可确保不会有其他具有相同服务的组件被添加到实体中。
+ `IncompatibleService`规范常用于指定一个实体上不能存在多个相同的组件。
