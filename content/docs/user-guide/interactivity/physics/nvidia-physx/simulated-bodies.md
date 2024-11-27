---
description: ' PhysX 模拟体。 '
title: PhysX 模拟体
weight: 300
---

`AzPhysics::SimulatedBody` 是许多 PhysX 组件继承自的基本类型：
+ **[PhysX Static Rigid Body](/docs/user-guide/components/reference/physx/static-rigid-body/)**
+ **[PhysX Dynamic Rigid Body](/docs/user-guide/components/reference/physx/rigid-body/)**
+ **[PhysX Character Controller](/docs/user-guide/components/reference/physx/character-controller/)**
+ **[PhysX Ragdoll](/docs/user-guide/components/reference/physx/ragdoll/)**

`AzPhysics::SimulatedBody`的接口包括各种常见操作，例如：
- 处理碰撞和触发器事件
- 获取 **Simulated Body** 所附加到的 **实体** 的实体 ID
- 获取 **实体** **Transform**
- 获取 **实体** **AABB**
- 针对特定的 **Simulated Body** 执行光线投射

此功能也通过 **SimulatedBodyComponentRequestsBus** 公开。

## Raycast
**Simulated Body** 光线投射查询测试从指定点沿指定方向指向的线段是否与 **Simulated Body** 相交（并且仅与该体）相交。将仅返回最近的匹配（如果有）。还可以执行 [PhysX 场景查询](/docs/user-guide/interactivity/physics/nvidia-physx/scene-queries/) ，以查询场景中的所有物理几何体。

光线投射查询是使用`AzPhysics::RayCastRequest`.

**RayCastRequest 属性**

| 属性 | 说明 |
| --- | --- |
|  `m_distance`  |  沿射线测试交集的最大距离。 |
|  `m_start`  |  光线开始的世界空间点。  |
|  `m_direction`  |  投射光线的方向。此向量必须归一化。 |
|  `m_hitFlags`  |  用于请求返回特定点击字段或指示返回值中哪些点击字段有效的标志。 |
|  `m_filterCallback`  |  **Simulated Body** 光线投射查询会忽略此属性。  |
|  `m_reportMultipleHits`  |  **Simulated Body** 光线投射查询会忽略此属性，因为只会返回最近的命中。  |
|  `m_maxResults`  |  **Simulated Body** 光线投射查询会忽略此属性，因为只会返回最近的命中。  |
|  `m_collisionGroup`  |  指定要测试的图层。使用此选项可仅针对特定层进行测试。  |
|  `m_queryType`  |  **Simulated Body** 光线投射查询会忽略此属性。  |

**示例 (C++)**
```
AzPhysics::RayCastRequest request;
request.m_start = AZ::Vector3(-100.0f, 0.0f, 0.0f);
request.m_direction = AZ::Vector3(1.0f, 0.0f, 0.0f);
request.m_distance = 200.0f;

AzPhysics::SceneQueryHit hit;
AzPhysics::SimulatedBodyComponentRequestsBus::EventResult(hit, entityId, &AzPhysics::SimulatedBodyComponentRequests::RayCast, request);
```

**示例 (Lua)**
```
request = RayCastRequest()
request.Start = Vector3(5.0, 0.0, 5.0)
request.Direction = Vector3(0.0, 0.0, -1.0)
request.Distance = 10.0
hit = SimulatedBodyComponentRequestBus.Event.RayCast(entityId, request)
```

**Script Canvas**

**Raycast （Single Body）** 节点可以在 **Script Canvas** 中用于执行 **Simulated Body** 光线投射查询。
