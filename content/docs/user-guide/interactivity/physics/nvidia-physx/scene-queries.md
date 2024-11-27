---
description: ' Create raycast and shape cast queries in Open 3D Engine. '
title: PhysX Scene Queries
weight: 300
---

您可以使用物理特性光线投射查询来确定特定线段是否与物理特性几何体相交。同样，形状转换查询（也称为扫描）测试沿线段拉伸的形状是否与物理几何体相交。这些查询的示例用途可能包括确定一个对象是否在另一个对象的前面，或测试视线。重叠查询是第三种类型的场景查询，用于确定固定形状是否与其他物理几何体相交。所有这些场景查询都在`AzPhysics::SceneInterface` 对象上执行，并且它们都针对整个场景进行测试。请注意，除了针对整个场景的查询外，还可以对单个 [**PhysX 模拟体**](/docs/user-guide/interactivity/physics/nvidia-physx/simulated-bodies/#raycast)执行光线投射。

每种类型的场景查询都可以使用`AzPhysics::SceneQueryRequest`对象来执行（三种场景查询类型中的每一种都有请求对象的专用化）。除了执行单个查询外，还可以使用`AzPhysics::SceneQueryRequests`对象（许多查询的容器）将一批查询收集到单个调用中。也可以同步或异步调用请求。

```
virtual SceneQueryHits QueryScene(SceneHandle sceneHandle, const SceneQueryRequest* request) = 0;
virtual SceneQueryHitsList QuerySceneBatch(SceneHandle sceneHandle, const SceneQueryRequests& requests) = 0;
[[nodiscard]] virtual bool QuerySceneAsync(SceneHandle sceneHandle, SceneQuery::AsyncRequestId requestId,
    const SceneQueryRequest* request, SceneQuery::AsyncCallback callback) = 0;
[[nodiscard]] virtual bool QuerySceneAsyncBatch(SceneHandle sceneHandle, SceneQuery::AsyncRequestId requestId,
    const SceneQueryRequests& requests, SceneQuery::AsyncBatchCallback callback) = 0;
```

场景查询的结果使用`AzPhysics::SceneQueryHit`对象的容器进行描述。

{{< note >}}
场景查询可能会降低性能。
{{< /note >}}

## 光线投射

光线投射查询是最常见的场景查询，它基于从开始位置沿光线方向发射光线指定距离的光线。

**例**
光线投射查询仅与五边形相交。

![Raycast query example in PhysX world.](/images/user-guide/physx/physx-raycast-shape-cast-queries-2.png)

光线投射查询是使用`AzPhysics::RayCastRequest`指定的。

**RayCastRequest 属性**

| 属性 | 说明 |
| --- | --- |
|  `m_distance`  |  沿射线测试交集的最大距离。  |
|  `m_start`  |  光线开始的世界空间点。  |
|  `m_direction`  |  投射光线的方向。此向量必须归一化。  |
|  `m_hitFlags`  |  用于请求返回特定点击字段或指示返回值中哪些点击字段有效的标志。  |
|  `m_filterCallback`  |  游戏提供的自定义回调函数，用于过滤掉特定对象。  |
|  `m_reportMultipleHits`  |  是返回查询中的所有点击（最多 `m_maxResults`），还是仅返回第一个点击。  |
|  `m_maxResults`  |  要返回的最大点击数（受 [全局配置](/docs/user-guide/interactivity/physics/nvidia-physx/configuring/configuration-global/) 的限制).  |
|  `m_collisionGroup`  |  指定要测试的图层。使用此选项可仅针对特定层进行测试。 |
|  `m_queryType`  |  包括静态对象和/或动态对象。  |

要执行光线投射查询，请使用`AzPhysics::SceneInterface`.

**示例 (C++)**

```
auto* sceneInterface = AZ::Interface<AzPhysics::SceneInterface>::Get();

AzPhysics::RayCastRequest request;
request.m_start = AZ::Vector3(-100.0f, 0.0f, 0.0f);
request.m_direction = AZ::Vector3(1.0f, 0.0f, 0.0f);
request.m_distance = 200.0f;

AzPhysics::SceneQueryHits result = sceneInterface->QueryScene(AzPhysics::DefaultPhysicsSceneName, &request);
auto numHits = result.size();
```

**示例 (Lua)**

```
physicsSystem = GetPhysicsSystem()
sceneHandle = physicsSystem:GetSceneHandle(DefaultPhysicsSceneName)
scene = physicsSystem:GetScene(sceneHandle)
request = RayCastRequest()
request.Start = Vector3(5.0, 0.0, 5.0)
request.Direction = Vector3(0.0, 0.0, -1.0)
request.Distance = 10.0
request.ReportMultipleHits = true
hits = scene:QueryScene(request)
numHits = hits.HitArray:Size()
```

**Script Canvas**

以下节点在 **Script Canvas** 中可用：
- **Raycast (Local Space)** 返回光线从源实体沿指定方向在本地空间中投射的第一个实体。
- **Raycast (World Space)** 返回光线在世界空间中从指定方向的起始位置投射的第一个实体。
- **Raycast Multiple (Local Space)** 返回光线在局部空间中从源实体沿指定方向投射的所有实体。

## Shapecast

形状投射查询与光线投射查询类似，不同之处在于形状投射查询采用形状以及点和方向。形状沿射线扫掠以形成体积。与此卷相交的任何内容都将从查询中返回。

**示例**
形状转换查询呈球体形状，并与矩形和五边形实体相交。

![Shapecast query example in PhysX.](/images/user-guide/physx/physx-raycast-shape-cast-queries-3.png)

光线投射查询是使用`AzPhysics::ShapeCastRequest`.

**ShapeCastRequest 属性**

| 属性                     | 描述                                                                                                            |
|------------------------|---------------------------------------------------------------------------------------------------------------|
| `m_distance`           | 要测试的沿 `m_direction` 的最大距离。                                                                                    |
| `m_start`              | 在形状转换开始的世界空间中变换。                                                                                              |
| `m_direction`          | 投射方向。向量必须归一化。                                                                                                 |
| `m_hitFlags`           | 用于请求返回特定点击字段或指示返回值中哪些点击字段有效的标志。                                                                               |
| `m_shapeConfiguration` | 应沿射线扫掠的形状。                                                                                                    |
| `m_filterCallback`     | 游戏提供的自定义回调函数，用于过滤掉特定对象。                                                                                       |
| `m_reportMultipleHits` | 是返回查询中的所有点击（最多 `m_maxResults`），还是仅返回第一个点击。                                                                    |
| `m_maxResults`         | 要返回的最大点击数（受 [全局配置](/docs/user-guide/interactivity/physics/nvidia-physx/configuring/configuration-global/)限制）。 |
| `m_collisionGroup`     | 指定要测试的图层。使用此属性可仅针对特定层进行测试。                                                                                    |
| `m_queryType`          | 包括静态对象和/或动态对象。                                                                                                |

支持长方体、胶囊体和球体几何体，并且有帮助程序函数可用于在`AzPhysics::ShapeCastRequestHelpers`中使用这些形状创建查询。C++ 也支持凸网格几何结构，但目前不支持脚本。

要执行 shapecast 查询，请使用 `AzPhysics::SceneInterface`.

**示例 (C++)**

```
auto* sceneInterface = AZ::Interface<AzPhysics::SceneInterface>::Get();

AzPhysics::ShapeCastRequest request = AzPhysics::ShapeCastRequestHelpers::CreateSphereCastRequest(1.0f,
    AZ::Transform::CreateTranslation(AZ::Vector3(-20.0f, 0.0f, 0.0f)),
    AZ::Vector3(1.0f, 0.0f, 0.0f),
    20.0f,
    AzPhysics::SceneQuery::QueryType::StaticAndDynamic,
    AzPhysics::CollisionGroup::All,
    nullptr);

AzPhysics::SceneQueryHits hits = sceneInterface->QueryScene(AzPhysics::DefaultPhysicsSceneName, &request);
```

**示例 (Lua)**

```
physicsSystem = GetPhysicsSystem()
sceneHandle = physicsSystem:GetSceneHandle(DefaultPhysicsSceneName)
scene = physicsSystem:GetScene(sceneHandle)

boxDimensions = Vector3(1.0, 1.0, 1.0)
startPose = Transform.CreateTranslation(Vector3(0.0, 0.0, 5.0))
direction = Vector3(0.0, 0.0, -1.0)
distance = 10.0
queryType = 0
collisionGroup = CollisionGroup("All")
request = CreateBoxCastRequest(boxDimensions, startPose, direction, distance, queryType, collisionGroup)

hits = scene:QueryScene(request)
```

**Script Canvas**

以下节点在 **Script Canvas** 中可用：
- **Box Cast** 返回具有框几何体的形状转换查询命中的第一个实体。
- **Capsule Cast** 返回具有胶囊体几何体的形状投射查询命中的第一个实体。
- **Sphere Cast** 返回具有球体几何体的形状投射查询命中的第一个实体。

## Overlap

重叠查询更简单，因为它们不采用方向或距离。重叠查询只返回在世界中指定位置与形状相交的所有对象。

重叠查询是使用`AzPhysics::OverlapRequest`.

**OverlapRequest 属性**

| 属性 | 说明 |
| --- | --- |
|  `m_pose` |  在形状的世界空间中变换。  |
|  `m_shapeConfiguration`  |  用于重叠的形状。  |
|  `m_filterCallback`  |  自定义回调函数，用于过滤掉特定实体。  |
|  `m_unboundedOverlapHitCallback`  |  允许重叠查询返回无限个结果，并通过回调进行处理。  |
|  `m_maxResults`  |  要返回的最大点击数（受 [全局配置](/docs/user-guide/interactivity/physics/nvidia-physx/configuring/configuration-global/) 的限制， 并假设`m_unboundedOverlapHitCallback`未使用 ).  |
|  `m_collisionGroup`  |  指定要测试的图层。使用此属性可仅针对特定层进行测试。  |
|  `m_queryType`  |  包括静态对象和/或动态对象。  |

支持长方体、胶囊体和球体几何体，并且有帮助程序函数可用于在`AzPhysics::OverlapRequestHelpers`中使用这些形状创建查询。C++ 也支持凸网格几何结构，但目前不支持脚本。

要执行重叠查询，请使用`AzPhysics::SceneInterface`.

**示例 (C++)**
```
auto* sceneInterface = AZ::Interface<AzPhysics::SceneInterface>::Get();

AzPhysics::OverlapRequest request = AzPhysics::OverlapRequestHelpers::CreateBoxOverlapRequest(AZ::Vector3(3.0f),
    AZ::Transform::CreateTranslation(AZ::Vector3(13.0f, 0.0f, 0.0f)));

AzPhysics::SceneQueryHits results = sceneInterface->QueryScene(AzPhysics::DefaultPhysicsSceneName, &request);
```

**示例 (Lua)**
```
physicsSystem = GetPhysicsSystem()
sceneHandle = physicsSystem:GetSceneHandle(DefaultPhysicsSceneName)
scene = physicsSystem:GetScene(sceneHandle)

boxDimensions = Vector3(1.0, 1.0, 1.0)
pose = Transform.CreateTranslation(Vector3(0.0, 0.0, 5.0))
request = CreateBoxOverlapRequest(boxDimensions, pose)

hits = scene:QueryScene(request)
```

**Script Canvas**

以下节点在 **Script Canvas** 中可用：
- **Overlap Box** 返回与盒体几何体重叠的 Entity ID 数组。
- **Overlap Capsule** 返回与胶囊体几何体重叠的实体 ID 数组。
- **Overlap Sphere** 返回与球体几何体重叠的实体 ID 数组。

## SceneQueryHit
场景查询的结果使用 `AzPhysics::SceneQueryHit`对象进行描述。单个查询可能会返回多个结果，这些结果包含在`AzPhysics::SceneQueryHits`对象中。批处理查询的结果使用`AzPhysics::SceneQueryHitsList`对象进行描述，该对象是`AzPhysics::SceneQueryHits`对象的容器。

**SceneQueryHit 属性**

| 属性 | 说明 |
| --- | --- |
| `m_resultFlags` | 指示以下哪些属性有效的标志。 |
| `m_distance` | 从查询源到点击的距离（仅对光线投射和形状投射查询有效）。 |
| `m_bodyHandle` | 被击中的`AzPhysics::SimulatedBody`的句柄。 |
| `m_entityId` | 被命中的正文的实体 ID。|
| `m_shape` | 被击中的身体形状。 |
| `m_physicsMaterialId` | 被点击的形状（或面）上的 物理 材质 ID。 |
| `m_position` |命中在世界空间中的位置（仅对光线投射和形状投射查询有效）。 |
| `m_normal` | 击中表面的世界空间中的 [normal](https://en.wikipedia.org/wiki/Normal_(geometry))。（仅对 Raycast 和 Shapecast 查询有效）。 |
