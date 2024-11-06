---
linkTitle: Non-uniform Scale
description: ' 使用 Non-uniform Scale 组件可沿每个轴按不同的量缩放实体。 '
title: Non-uniform Scale 组件
---

**Non-uniform Scale** 组件允许实体沿每个局部轴缩放不同的量。 可以通过单击 [Transform](/docs/user-guide/components/reference/transform/) 组件上的 Add non-uniform scale 按钮来添加它。

**Non-uniform Scale** 组件与某些其他组件不兼容，这些组件如果不从根本上改变其特性，就无法进行非均匀缩放。例如，它与 [Sphere Shape](/docs/user-guide/components/reference/shape/sphere-shape/) 组件不兼容，因为不均匀缩放球体会产生椭球体并破坏球体的特征对称性。

为避免倾斜问题，非均匀缩放仅应用于当前实体，不会传播到其子实体。

## 兼容的组件
以下是与**Non-uniform Scale**兼容的组件：
+ **[Box Shape](/docs/user-guide/components/reference/shape/box-shape/)**
+ **[Polygon Prism Shape](/docs/user-guide/components/reference/shape/polygon-prism-shape/)**
+ **[Quad Shape](/docs/user-guide/components/reference/shape/quad-shape/)**
+ **[PhysX Primitive Collider](/docs/user-guide/components/reference/physx/collider/)** - 请注意，如果基元碰撞体的缩放不均匀，则会将其替换为凸近似值，这可能会略微降低性能。可以使用 [PhysX Primitive Collider](/docs/user-guide/components/reference/physx/collider/)组件上的 **Subdivision level**  设置来调整凸面近似的细节级别。
+ **[PhysX Mesh Collider](/docs/user-guide/components/reference/physx/mesh-collider/)**
+ **[PhysX Shape Collider](/docs/user-guide/components/reference/physx/shape-collider/)**
+ **[PhysX Static Rigid Body](/docs/user-guide/components/reference/physx/static-rigid-body/)**
+ **[PhysX Dynamic Rigid Body](/docs/user-guide/components/reference/physx/rigid-body/)**
+ **[PhysX Force Region](/docs/user-guide/components/reference/physx/force-region/)**
+ **[Decal](/docs/user-guide/components/reference/atom/decal/)**
+ **Mesh**

## 不兼容的组件
以下组件与 ***Non-uniform Scale** 组件**不兼容**，因为非均匀缩放会破坏组件所做的基本假设，或者因为它们需要大量工作才能正确支持非均匀缩放：
+ **[Capsule Shape](/docs/user-guide/components/reference/shape/capsule-shape/)**
+ **[Compound Shape](/docs/user-guide/components/reference/shape/compound-shape/)**
+ **[Cylinder Shape](/docs/user-guide/components/reference/shape/cylinder-shape/)**
+ **[Disk Shape](/docs/user-guide/components/reference/shape/disk-shape/)**
+ **[Sphere Shape](/docs/user-guide/components/reference/shape/sphere-shape/)**
+ **[Tube Shape](/docs/user-guide/components/reference/shape/tube-shape/)**
+ **Cloth**
+ **[PhysX Ball Joint](/docs/user-guide/components/reference/physx/ball-joint/)**
+ **[PhysX Fixed Joint](/docs/user-guide/components/reference/physx/fixed-joint/)**
+ **[PhysX Hinge Joint](/docs/user-guide/components/reference/physx/hinge-joint/)**
+ **[PhysX Prismatic Joint](/docs/user-guide/components/reference/physx/prismatic-joint/)**
+ **[PhysX Ragdoll](/docs/user-guide/components/reference/physx/ragdoll/)**
+ **[PhysX Character Controller](/docs/user-guide/components/reference/physx/character-controller/)**
+ **PhysX Character Gameplay**
+ **[Attachment](/docs/user-guide/components/reference/animation/attachment/)**
+ **[Actor](/docs/user-guide/components/reference/animation/actor/)**
+ **[Simple Motion](/docs/user-guide/components/reference/animation/simple-motion/)**
+ **Fly Camera Input**
+ **HDRi Skybox**
+ **Physical Sky**
<!-- + **[Blast Family](/docs/user-guide/components/reference/destruction/blast-family/)** -->
<!-- + **[Blast Family Mesh Data](/docs/user-guide/components/reference/destruction/blast-family-mesh-data/)** -->

以下组件目前 **不兼容** 因为它们尚不受支持，但没有导致兼容性难以添加的根本原因：
+ **Sequence**
+ **[Spline](/docs/user-guide/components/reference/shape/spline/)**
+ **[White Box](/docs/user-guide/components/reference/shape/white-box/)**
+ **[White Box Collider](/docs/user-guide/components/reference/shape/white-box-collider/)**
+ **Diffuse Probe Grid**
+ **Reflection Probe**

## EBus 请求总线接口
**NonUniformScaleRequestBus** 是用于 **Non-uniform Scale** 组件的请求总线。

有关使用事件总线 （EBus） 接口的更多信息，请参阅 [使用事件总线（EBus）系统](/docs/user-guide/programming/messaging/ebus/)。

将以下请求函数与 EBus 接口结合使用，以与其他组件进行通信。

### GetScale

返回实体的非均匀缩放。

**参数**
None

**返回值**
实体的非均匀缩放值。
Type: Vector3

### SetScale

设置实体的非均匀缩放。

**参数**
新的非均匀缩放值。
Type: Vector3

**返回值**
None

### RegisterScaleChangedEvent

为更改实体的非均匀缩放时引发的 **[AZ::Event](/docs/user-guide/programming/messaging/az-event/)** 注册处理程序。
**参数**
非均匀缩放更改事件的处理程序。
Type: NonUniformScaleChangedEvent::Handler

**返回值**
None

## 编辑器自动化
可以使用以下内容访问编辑器 **Non-uniform Scale** 组件的组件类型 ID 以进行编辑器自动化。
```
azlmbr.editor.EditorNonUniformScaleComponentTypeId
```

例如，可以添加 **Non-uniform Scale** 组件，并按如下方式修改缩放。

```
nonUniformScaleComponentId = azlmbr.editor.EditorNonUniformScaleComponentTypeId
azlmbr.editor.EditorComponentAPIBus(azlmbr.bus.Broadcast,'AddComponentsOfType', entityId, [nonUniformScaleComponentId])
azlmbr.entity.NonUniformScaleRequestBus(azlmbr.bus.Event, 'SetScale', entityId, azlmbr.math.Vector3(1.0, 2.0, 3.0))
```
