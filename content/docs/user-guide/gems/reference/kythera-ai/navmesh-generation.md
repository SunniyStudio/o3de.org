---
linkTitle: 导航网格生成
title: Kythera AI 导航网格生成
description: 在 Open 3D Engine （O3DE） 中使用 Kythera AI Gem 生成导航网格
weight: 300
toc: true
---

## 边界设置

要生成导航网格，您需要在关卡中至少有一个导航网格边界。

要声明导航网格边界，请创建一个具有 **Polygon Prism Shape** 组件的实体来形成边界，并使用 **Bounds Navigate Path** 组件对其进行标记以生成导航网格。

![Navmesh bounds setup](/images/user-guide/gems/kythera-ai/navmesh-generation-bounds-setup.png)

## 排除卷

如果边界对象选中了 **Exclusion Volume** 属性，则会导致在边界内生成导航网格 **not**。与排除体积相交的切片将被裁剪。

![Navmesh exclusion volume](/images/user-guide/gems/kythera-ai/navmesh-generation-exclusion-volumes.png)

为了更好地区分导航网格边界和排除体积，您可以设置 Polygon Prism Shape 组件的颜色。在以下示例中，排除卷的颜色为红色：

![Exclusion volume, red visualization](/images/user-guide/gems/kythera-ai/navmesh-generation-exclusion-volumes-red.png)

## 配置

导航网格生成的设置在`NavMesh.xml`文件中配置。全局`NavMesh.xml`放置在项目的`Scripts`或`Levels`目录中。对于特定于关卡的导航网格，可以在关卡的子目录中放置一个 `NavMesh.xml`。

此文件中可以指定多个导航网格。例如，可以为小角色和大角色指定单独的导航网格。

必须在`.xml`的`NavMeshNames`部分中指定每个导航网格，声明其名称和类型。请参阅以下示例 `NavMesh.xml` 文件。

```xml
<NavMeshParams>
 <DefaultNavMeshName type="string">Default</DefaultNavMeshName>
 <DefaultNavMeshType type="string">MediumSizedCharacters</DefaultNavMeshType>
 <NavMeshNames type="bb">
  <Default type="bba">
   <Entry0 type="string">MediumSizedCharacters</Entry0>
  </Default>
  <Boss type="bba">
   <Entry0 type="string">LargeSizedCharacters</Entry0>
  </Boss>
 </NavMeshNames>
 <NavMeshTypes type="bb">
  <MediumSizedCharacters type="bb">
   <AgentHeight type="float">1.720000</AgentHeight>
   <AgentMaxClimb type="float">0.700000</AgentMaxClimb>
   <AgentMaxSlope type="float">45.000000</AgentMaxSlope>
   <AgentRadius type="float">0.600000</AgentRadius>
   <CellHeight type="float">0.050000</CellHeight>
   <CellSize type="float">0.100000</CellSize>
   <Regenerate type="bool">false</Regenerate>
  </MediumSizedCharacters>
  <LargeSizedCharacters type="bb">
   <AgentHeight type="float">1.800000</AgentHeight>
   <AgentMaxClimb type="float">0.700000</AgentMaxClimb>
   <AgentMaxSlope type="float">45.000000</AgentMaxSlope>
   <AgentRadius type="float">2.000000</AgentRadius>
   <CellHeight type="float">0.050000</CellHeight>
   <CellSize type="float">0.100000</CellSize>
   <Regenerate type="bool">false</Regenerate>
  </LargeSizedCharacters>
 </NavMeshTypes>
</NavMeshParams>
```
## 过滤物理对象以生成导航网格

Kythera AI 可以筛选在生成导航网格时考虑哪些物理对象。这是通过使用 **PhysX Collision Group** 来执行的。与指定碰撞组碰撞的对象将被视为导航网格生成，而不与碰撞组碰撞的对象将被忽略。通过在 `NavMesh.xml` 的每个导航网格类型参数中设置`NavmeshPhysicsCollisionGroup`参数，可以基于每个导航网格类型设置碰撞组。请参阅 PhysXTest 演示关卡，了解此功能的实际应用示例，其中导航网格生成会忽略“幽灵树篱”。

## 生成导航网格

要生成或重新生成导航网格，请选择 Kythera 工具栏上的 **Regenerate Navmesh** 按钮 (![Regenerate navmesh icon](/images/user-guide/gems/kythera-ai/toolbar-generate-navmesh.png))或使用 console 命令`kyt_Generate`。

## 保存导航网格

要将导航网格保存到磁盘，请选择 Kythera 工具栏上的 **Save Navmesh** 按钮 (![Save navmesh icon](/images/user-guide/gems/kythera-ai/toolbar-save-navmesh.png))，或使用 console 命令 `kyt_SaveTiles`。

对于较小的关卡，通常不需要保存导航网格。小的导航网格将在运行时自动生成。

## 可视化

要启用导航网格调试绘制，请选择 Debug Draw Navmesh （调试绘制导航网格） 按钮(![Debug draw navmesh icon](/images/user-guide/gems/kythera-ai/toolbar-debug-draw-navmesh.png))，或将控制台变量`kyt_DrawMaster`和`kyt_DrawNavMesh`设置为`1`。大于 `1` 的值将更改导航网格的绘制方式的细节。

可以使用 Kythera 工具栏中的组合框选择要可视化的导航网格。默认值为 `Default navmesh`。或者，您可以将控制台变量`kyt_DrawNavMeshName`设置为要可视化的导航网格的名称。

## 将导航网格分配给代理

为代理分配一个导航网格，以便在全局 `Profiles.xml` 文件中使用。

```xml
<NavMeshName type="string">Default</NavMeshName><NavMeshType type="string">MediumSizedCharacters</NavMeshType>
```

如果未在代理的配置文件中指定导航网格名称或类型，则使用`NavMesh.xml`中的`DefaultNavMeshName`和/或`DefaultNavMeshType`的值。
